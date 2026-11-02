+++
categories = [ "Guides" ]
date = 2022-05-04T04:00:00.000Z
description = "Let the nice robots at GitHub handle sending Venmo requests to your friends."
draft = false
images = [ "/uploads/recurring-venmo-payments-smaller.png" ]
tags = [ "automation", "venmo", "ifttt", "github-actions" ]
title = "Automate Recurring Venmo Payments with Github Actions and IFTTT"
_template = "blog_post"
+++

Like so many tinkerers, I love to [automate small parts of my life, while ignoring any actual work](https://xkcd.com/1319/) I should be doing. After stumbling on [Joe’s Venmo automation](https://joeprevite.com/send-automatic-recurring-payments-on-venmo), I immediately swept all the important things off my plate to have some fun. Now let’s set up automatic payments on [Venmo](https://venmo.com/) in less than an hour — with email notifications for peace of mind!

![Clock + robot snake + Venmo](/uploads/recurring-venmo-payments-smaller.webp#center)

## Why not Nocode (Zapier, IFTTT, etc.)

You’ll need to know how to code a small amount for this guide. This setup is an alternative to the Zapier Venmo integration and IFTTT Venmo integration, which don’t seem to exist anymore (if they ever did). Venmo shut down their developer APIs and [have had them in maintenance mode since 2016](https://venmo.com/developers), so unlikely for anything to change on that front. Normally I look to [nocode tools](https://www.nocode.tech/) first for projects, but unfortunately in this case, you’ll need to know how to run code online.

## Ingredients

The 3 pieces you will need to set this up (in <1 hour):

- A scheduled trigger (GitHub actions, AWS Cloudwatch, etc.)
- A place to run a Python script + it’s dependency (GitHub actions, PythonAnywhere, Node.js with [Autocode](https://autocode.com/) is in exploration)
- A free or cheap notification service (IFTTT, AWS SNS, etc.)

I use [GitHub Actions](https://github.com/features/actions) + Python script + [IFTTT](https://ifttt.com/) for sending emails, but feel free to step off the beaten path if you have other ideas.

## How to set up automatic payments on Venmo

Diving into the meat & potatoes you came for, how to actually get this dang thing running.

1. **(Optional) 💌 Set up IFTTT notifications**
    
    > ℹ️ *If you don’t care about being notified, skip this step and remove the `notify()` code in script.*
    > 
    
    ![Example of the IFTTT notification email](/uploads/ifttt-notification-email.png#center)
    
    IFTTT allows a [variety of target services](https://ifttt.com/explore/services), I originally used SMS, then converted to email since it’s not an urgent alert.
    
    - Sign up for IFTTT account (might require downloading of the app).
    - Configure the [Webhooks service](https://help.ifttt.com/hc/en-us/articles/115010230347-Webhooks-service-FAQ) in IFTTT.
    - Then set up your chosen notification service. My example script sends metadata about the Venmo request sent in the `value1` field IFTTT provides. If you use the same, you'll have to make sure that is mapped correctly to your chosen notification service.

        ![IFTT applet set-up screen](/uploads/ifttt-webhook-applet-bg.png#center)

    - ✅ Use cURL or your favorite web request tool to test your webhook and ensure notifications are sending correctly.
    - Save your webhook URL as a Secret so the workflow can access it. In the browser, go to `GitHub Repo -> Settings -> Secrets -> New Repository Secret`.

2. 🔐 **Get Venmo access token**

    >⚠️🚨
    > *This token has the same power with your Venmo account as you do, so be very careful with it! It will never expire unless you logout manually, as [explained in the docs](https://github.com/mmohades/Venmo#usage). 
    > 
    > I plan to update this guide to use [Payment Links](https://venmo.com/paymentlinks) (a [deeplink](https://en.wikipedia.org/wiki/Deep_linking) into the Venmo app that doesn’t require account access) in the near future, but for now, you are forewarned.*
    > ⚠️🚨
    >

    You’ll have to be comfortable working with Python dependencies to run these scripts locally.

    - Install the [Venmo client library](https://github.com/redloon/venmo/blob/master/main.py#L32) `venmo-api`.
    - Run this [Python script](https://github.com/redloon/venmo/blob/master/fetch_v_token.py) to pull your access token. You may need to handle MFA.

    ```
    from venmo_api import Client
    # https://github.com/mmohades/Venmo
    
    print("Login using account to get an access token for the API.")
    email = input('Venmo Email:')
    if '@' not in email:
        print('! yo what the heck you sure thats an email')
        raise SystemExit
    password = input('Venmo Password:')
    
    Client.get_access_token(username=email, password=password)
    ```

    - Save the token as the Secret `VENMO_ACCESS_TOKEN` by going to `GitHub Repo -> Settings -> Secrets -> New Repository Secret`.

3. **(Optional) 🗺 Map Venmo usernames to IDs**

    Then Venmo API requires payment requests to be tied to an ID, rather than the `@username` we humans use in the app. I did the mapping of usernames during setup and [added them as environment secrets](https://github.com/redloon/venmo/blob/f375c57ec30d668c4ce1c40403cd06c9be843f1d/main.py#L32), but you can just as easily add 1 extra LOC to convert during regular runs.

    ```
    user = venmo_client.user.get_user_by_username(username)
    # Key is the 'user.id' part
    venmo_client.payment.request_money(amount, note, target_user_id=user.id, privacy_setting=PaymentPrivacy.PRIVATE)
    ```

4. **🐍 Modify the Python script**

    [The script I use](https://github.com/redloon/venmo/blob/master/main.py#L32) has a simple entry point, then defines a separate function for each Venmo request I want it to make, YMMV. You’ll need to make some small adjustments based on how many requests you need it to make & how you fetch Venmo user IDs. The most important bits can be boiled down to this `main.py`:

    ```
    # main.py
    
    import os
    from venmo_api import Client, PaymentPrivacy
    # requests is pulled in by venmo_api
    import requests
    
    TOKEN_KEY = 'VENMO_ACCESS_TOKEN'
    IFTTT_WEBHOOK_KEY = 'IFTTT_WEBHOOK'
    
    def main():
        # load the access token from environ
        access_token = os.environ.get(TOKEN_KEY)
        if access_token is None:
            raise ValueError('[!] EVERYTHING IS BROKEN WE CANT GET ACCESSS AHHHHHH')
    
        venmo_client = Client(access_token=access_token)
    
    ....
    		# <venmo user id from mapping step>
    		c_user_id = os.environ.get('C_USER_ID') 
    		amount = 20.0
    		note = f'Monthly Internet'
    		try:
    	    venmo_client.payment.request_money(amount, note, target_user_id=c_user_id, privacy_setting=PaymentPrivacy.PRIVATE)
    			print('Successfully requested for internet $$')
    		  msg = f'Requested ${amount} Internet from C.'
    		except Exception as e:
    			print(e)
    			msg = '[!] Failed to request $$ for internet from C.'
    		notify(msg)
    
    ....
    
    def notify(msg):
        # notify with IFTTT webhook
        ifttt_webhook_url = os.environ.get(IFTTT_WEBHOOK_KEY)
        if ifttt_webhook_url is None:
            raise ValueError('[!] Dang, no webhook so cant notify myself :(')
        
        json_data = {
            'value1': msg
        }
        resp = requests.post(ifttt_webhook_url, json=json_data)
        print(resp.status_code)
        if resp.status_code >= 300:
            raise ValueError(f'Webhook resp was {resp.status_code}; not a success. Body: {resp.text}')
    
    main()
    ```

5. **👨‍💻 Configure GitHub Secrets & Actions**

    > *ℹ️ Cron actions get disabled after 60 days of no activity on the repository - so either plan to push a commit every 50 days, or follow my approach and just wait until GitHub emails you.*
    > 

    ✋ At this point you should have your `VENMO_ACCESS_TOKEN` saved in Secrets, along with the optional `IFTTT_WEBHOOK` or `USER_IDs` values. if you didn’t, go back and do it.

    Now create your [workflow definition](https://github.com/redloon/venmo/blob/master/.github/workflows/recurring.yml) at `.github/workflows/<workflow-name>.yml` . This is where you will define your schedule in `cron`. My example shows monthly Venmo requests.

    ```
    # <workflow-name>.yml
    
    name: Run Recurring Venmo
    
    on:
      schedule:
        # 5/6pm EDT on the 10th of the month.
        - cron: "0 10 10 * *"
      workflow_dispatch:
    
    jobs:
      run:
        runs-on: ubuntu-latest
    
        steps:
          - uses: actions/checkout@v2
          - name: Set up Python 3.7
            uses: actions/setup-python@v2
            with:
              python-version: 3.7
          - name: Install dependencies
            run: |
              pip install pipenv
              pipenv install
          - name: Run script 
            # https://pipenv.pypa.io/en/latest/advanced/#automatic-loading-of-env; this is somewhat silly but I wanted to finish quick
            run: |
              echo VENMO_ACCESS_TOKEN=$VENMO_ACCESS_TOKEN >> .env
              echo GF_VENMO_USER_ID=$GF_VENMO_USER_ID >> .env
              echo C_USER_ID=$C_USER_ID >> .env
              echo IFTTT_WEBHOOK=$IFTTT_WEBHOOK >> .env
              pipenv run python main.py
            env:
              VENMO_ACCESS_TOKEN: ${{ secrets.VENMO_ACCESS_TOKEN }}
              GF_VENMO_USER_ID: ${{ secrets.GF_VENMO_USER_ID }}
              C_USER_ID: ${{ secrets.C_USER_ID }}
              IFTTT_WEBHOOK: ${{ secrets.IFTTT_WEBHOOK }}
    ```

6. **🏖️ Sit back and relax - with your automation in place, you’re now saving MINUTES each month!** 🤣

    But hey, we learned something neat and learned some new automation tricks. Time well spent.

    ![XKCD was it worth the time matrix](https://imgs.xkcd.com/comics/is_it_worth_the_time.png#center)

## 🧪 Experiments & potential improvements

- Writing Node.js function on [Autocode](https://autocode.com/) after switching to Payment Links.

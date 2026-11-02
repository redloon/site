+++
categories = []
comments = false
date = 2020-05-02T06:00:00Z
showpagemeta = false
showrelated = false
slug = ""
tags = []
title = "Projects"

+++
Projects listed below are in a very rough chronological order.

Project Status Guide (**_24-26_** _in total_):

* 🤫 Large secret ones I can't publicly tie my name to without plausible deniability **(0-2)**
* 🏃‍♂️ Active **(7)**
* ⏳ Functional, but not regularly contributing **(3)**
* ☠ Killed **(7)**
* 📚 Archive **(7)**

***

* [**🍳 Cooking for Lazy Fucks**](https://cookingforlazyfucks.com/)
  * **Status: 🏃‍♂️**
  * Recipes for lazy people.
* [**💻 Building on Slack**](https://buildingonslack.com)
  * **Status: 🏃‍♂️**
  * Learning in Public and publishing my notes & resources on building Apps on Slack, using Workflow Builder to the fullest, as well as more generic thoughts on using Slack in the workplace.
* [**💻 Tech Debt**](https://justtechdebt.com/)
  * **Status: 🏃‍♂️**
  * Editor for a small satire site - The Onion for developers & tech folks.
* [**💻 Hugo Shortcodes**](https://hugoshortcodes.netlify.app/)
  * **Status: 🏃‍♂️**
  * A collection of Hugo shortcodes gathered into one place, so it's easy for msyelf or others to find what they need for their next site.
* [**💻 Indie Lurker Compass**](/indie-lurker/) - _previously at indielurker.com_
  * **Status: 🏃‍♂️**
  * A living swipe file for indie hackers, indie lurkers, & founders who get stuck with indecision, perfectionism, or impostor syndrome. Examples and resources to help you put one foot in front of the other and get your experiments out the door 🚪. (Oct 2025: moved under this personal website instead of living separately).
* [**🎙Facade Project Podcast**](https://facadeproject.com/)
  * **Status: 🏃‍♂️**
  * A podcast with a couple friends to explore new things.
* [**💻Free on Your Birthday**](https://freeonyourbirthday.co)
  * **Status: ☠**
  * I missed out on getting free food on my birthday, so I started collecting all the ones I could find into an Airtable database. A No-code project.
  * _Why did I kill it?_ - Realized I have reached a point in my life where I want to invest my time in projects that go beyond saving someone a couple bucks on their birthday. I used to be all about finding bargains & being cheap, hence the existence of **Deals on Tap/Madtown Deals**, but eventually something clicked and I realized that I was missing out on long-term opportunities and investing in myself just to get a discount on wings 🍗.
* **✍ Writing**
  * **Status: 🏃‍♂️**
  * I started writing on this blog, [**Dev.to**](https://dev.to/idontremember), and [**Medium**](https://maybekq.medium.com/). Whether or not anyone reads it, it has been incredibly helpful to process thoughts instead of letting them endlessly bounce within my skull.
* [**👩‍💻Thyme**](https://redloon.github.io/thyme/)
  * **Status: ⏳**
  * An open source [**Ballerina lang**](https://ballerina.io/ "Ballerina Lang site") module to humanize working with the `stdlib` time module. [_Github_](https://github.com/redloon/thyme).
* [**💻Equation Finder**](https://equation-finder.netlify.app/)
  * **Status: ⏳**
  * A tool to help you understand what equations you have available base on given quantities. For solving problems in Physics, Math, Chemistry, and more.
* **Yugioh card scanner & price checker**
  * **Status: 📚**
  * CLI tool and a [**website**](https://yugiohprices.netlify.app/ "Yu-Gi-Oh Card Prices")
* [**Deals on Tap**](https://dealsontap.netlify.app/ "Deals on Tap: bar specials at UW-Madison")
  * **Status: ☠**
  * collecting bar specials on the UW-Madison campus for my friends to use.
* **ECE 551 Digital System Design and Synthesis Project: Digit Recognizing Simple Neural Network**
  * **Status: 📚**
  * Neural network on an fpga to detect hand-written digits.  Our professor handled creating the scaffolding of the input/output
    and generating the weights, then our group built all of the central logic for detection.
* **CS 506 Software Engineering Project: Lucidata - the SMS Information Hub, Part 2**
  * **Status: 📚**
  * [_Github - App_](https://github.com/frizzkitten/lucidataReact)
  * [_Github - Server_](https://github.com/redloon/506-Project)
  * As part of this course, we are formed into groups around proposals and my SMS Information Hub idea that we prototyped as LifeText _(see below)_ was selected. We decided the most user-friendly and space efficient (since texts are small and expensive through Twilio) would be to have an app that handles structuring and sending/receiving messages so the user only has to care about what information they actually want.  It is a React-Native app with a Django server for the backend.
* **Madtown Deals**
  * **Status: ☠**
  * [_Website_](https://madtowndeals.com)
  * Website to track the many regular deals that exist only on chalkboards or rarely updated bar websites.  Serverless API with Go and AWS services using the Serverless framework, see the [**blog post**](/blog/madtown-deals/) for more information.
* **Gymnastics Text**
  * **Status: ☠**
  * [_GitHub_](https://github.com/frizzkitten/gymnastics-text)
  * This service allows the UW Gymnastics Club to interact with our carpool Google Sheet using SMS rather than relying on having data/wifi.  My friends wanted to take the lead on making the Node app, so I worked on setting up an easy work environment with Docker and a local run Bash script, as well as taking care of the app deployment details as well as assisting with debugging and testing of the app itself.
* **Web Scraping**
  * **Status: 📚**
  * [_GitHub_](https://github.com/redloon/web-scraping)
  * _Faculty scraper_ - Though only a short project, I feel it is worth mentioning because it's not the number of lines or the time spent on a project that make it useful, but it's impact. A friend had been asked to grab a filtered list of names and emails from an online database, which would have taken days (3500ish names) to go through.  I wrote a scraper that both filters out the unneeded depts and sorts the list into a .csv file and delivered them a handy list of 2500 names to their undying gratitude (I got food, totally worth it).
  * _IT book downloader_ - Website of free technology e-books in pdf form, many looked very interesting so I figured I'd scrape all 7300 titles.  What's the point of having an NFS server if you don't have any files right? :) **Update** This philosophy falls apart pretty fast when said server is borked in the same week.
* **Ansible Laptop Configuration**
  * **Status: ☠**
  * [_GitHub_](https://github.com/redloon/AnsibleLaptop)
  * After having to reinstall Linux because Windows had messed up yet again, I had hoped to find a tool that could help automate all of the setup and customization that goes into a laptop.  Luckily, I found a lot of neat articles suggesting to use Ansible directed at localhost to accomplish this.  It's a somewhat slow/iterative process as I think of more pieces that I want in certain ways, but a good chunk of the base packages and tools I use are functional. I have a bootstrap script in the git repo so I just have to clone the repo and run the installation.  I test it in a VM snapshot after a clean install so I can be 100% sure things are working as expected.
  * Hand in hand with this is my dotfiles repo, to store even more configuration!  Not only does the Ansible repo install with a single script, but it calls the dotfiles repo which also installs in one command.  Wowza!  It's so automated I love it!
* **LifeText: SMS Information Hub**
  * **Status: ☠**
  * [_Frontend_](https://www.lifetext.us)
  * [_GitHub_](https://github.com/redloon/MHacks2017)
  * SMS based API hub to allow access to information when you don't have data.  MHacks X project.  We used the Twilio API to handle SMS, then used Python's built in difflib to make it so texts can be somewhat natural sounding as long as they include something close to spelled like a key word.  API's were Dark Sky for weather, Wikipedia, and NewsAPI.  It's hosted on AWS in a Docker container for ease of setup and rebuilding.  The frontend we added later on in the hackathon to give users a way to adjust the emergency contacts that want listed, but it ended up causing the most trouble so we had to pivot and build it just as a mock.  We'd like to continue on and see what this could become if we integrated social media and had a strong focus on countries/ areas that rarely have data connection.
* **sms2sheets**
  * **Status: ☠**
  * [_GitHub_](https://github.com/redloon/sms2sheets)
  * Connects the Twilio SMS API with the Google Sheets API to let me input data into my finance sheet when I don't have wifi or my laptop. It is a Flask server running from a Docker container on AWS.  I would like to eventually rework the sheets connection to act more like a database, whereas right now the structure of the sheet is from when I was manually inputting all the data.
* **ECE 353 Microprocessors Project: Tiva Missile Command**
  * **Status: 📚**
    <a href='http://www.youtube.com/watch?feature=player_embedded&v=cX48qmks5qg' target='_blank'><img class='center' src='http://img.youtube.com/vi/cX48qmks5qg/0.jpg' alt='YouTube video' width='240' height='180' border='10'/></a>
  * [_Video link_](https://www.youtube.com/watch?v=cX48qmks5qg)
  * Using a Tiva Launchpad and a number of drivers we helped write over the length of the course, my partner and I decided on the game Missile Command to emulate with our microprocessor.
* **Copundrum**
  * **Status: 📚**
  * [_Chat Bot_](https://redloon.github.io/copundrum/)
  * Chat bot project from the MadHacks Hackathon (Spring 2017).  Got the Iphone theme from Codepen and created the bot to have a variety of words it watches out for, and if it doesn't know how to process it responds with either a pun or uses the Giphy API to respond with a (hopefully) relevant gif.  We also incorporated the ability for it to play tic tac toe.
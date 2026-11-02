+++
date = 2018-02-16T05:00:00Z
categories = ["Archive"]
images = ["/uploads/madtowndeals-02.18-smaller.png"]
title = "Madtown Deals"
+++

My money-saving baby is finally in an acceptable state, [Madtown Deals](https://madtowndeals.com).  

Originally conceived as an hour-long quick project to just keep track of deals for myself, it has grown into a tool my friends are actually trying to use and talk about.

![Madtown Deals website in February 2018, showing a weekday selector and a searchable list of bar specials.](/uploads/madtowndeals-02.18-smaller.png#center)

This first iteration is pretty basic, with the API just sending all the deals in a block and letting the front end handle filtering/searching.  Upcoming changes will include pages showing all the suggestions from users and a page showing deals from the current day (eventually also by time).  More of the work will also be shifted to the backend, as well.  I also hope to borrow a friend who is a designer/understands UX to help make sure it's not too funky of a setup.  Long-term goal is to create an app for it, giving users multiple ways to access this information.  

For specifics on the structure of the site, it's a frontend hosted on Netlify.com with Ajax calls to my AWS API Gateway.  This in turn is hooked up to AWS Lambda functions written in Go, which access all of the data stored in AWS DynamoDB.  Not super complicated, but it gets the job done.  

From this point forward everything that happens is a bonus, as I was satisfied with it once a friend told me they used it successfully.

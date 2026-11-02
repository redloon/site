+++
title = "Add a Random Page Button to Hugo Site"
date = 2022-02-13T05:00:00.000Z
description = "Let users read a random page on your site. A quick guide on how to add this feature to your site in no time."
tags = [ "Hugo", "jamstack" ]
+++

I wanted to add a random button to one of my Hugo sites after remembering how handy it is on [XKCD](https://xkcd.com). Turns out it's a really simple process with Hugo, just need to generate a JSON file and then write a simple script to choose your random article.

## What are we making?

![Portfolio website with a randomizer button](/uploads/hugo-site-random-button-small.webp#center)

## How-to

### 1. Create JSON list of pages

Hugo conveniently lets you output JSON data, so we create one at the root which will get deploy alongside the rest of our static files. Couple caveats for this example:

1. my filters are simple because the site only has one page archetype and I wanted all of them. You can definitely get mroe creative with Hugo's `where` and [other functions](https://gohugo.io/functions/).
2. This index for site search was already in the theme I was using. If you just want the randomizer, you can drop all the extra data fields like `tags` and `contents`.

   # layouts/_default/index.json

   {{- $.Scratch.Add "index" slice -}}
   {{- range where site.RegularPages "Type" "in" site.Params.mainSections -}}
   {{ $date:= .PublishDate.Format "02"}}
   {{- $.Scratch.Add "index" (dict "title" .Title "date" $date "tags" .Params.tags "image" .Params.image "categories" .Params.categories "contents" .Plain "permalink" .Permalink) -}}
   {{- end -}}
   {{- $.Scratch.Get "index" | jsonify -}}

I needed to add a small param in my `config.toml`, here's an [article if you want to dive a bit deeper](https://zwbetz.com/build-a-search-bar-for-your-hugo-blog-with-a-json-index-and-some-vanilla-js/) and understand more of what is going on.

    [outputs]
        home = ["json", "html"]

#### Prod JSON output

For reference, the JSON that the above Hugo code will end up outputting.

    [
       {
         "categories": [
           "Developers"
         ],
         "contents": "text of the document",
         "date": "07",
         "image": "images/post/article-1.png",
         "permalink": "https://permalink",
         "tags": [
         	"Software Development"
         ],
         "title": "Title of the most recent article"
       },
     ...
     ]

### 2. Use JSON in an HTML page

You can add something similar to this in your HTML pages. I have this in a `partial` where the button is displayed on the page.

    <script>
     var searchIndexData = [];
     // fetch on page load from the search index
     let json_path = window.location.origin + '/index.json'
    
     fetch(json_path).then(function (response) {
     	return response.json();
     })
     .then(function (data) {
     	searchIndexData = data;
     })
     .catch(function (err) {
     	console.log(err)
     });
    
      
     function sendToRandomArticle() {
     let randIndex = Math.floor(Math.random() * searchIndexData.length);
     let randArticle = searchIndexData[randIndex]['permalink'] + '?utm_source=RandomButton';
     window.location.href = randArticle;
     }
    
    </script>
    ...
    ...
    <button type="button" class="btn btn-primary" onclick='sendToRandomArticle()'>Random</button>

That's it! Told you it was simple.

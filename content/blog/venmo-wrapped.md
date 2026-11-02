+++
categories = [ "Guides" ]
date = 2023-01-04T06:00:00.000Z
description = "Not overwhelmed yet by all the Spotify Wrapped clones? Follow along to make your own Venmo Wrapped!"
draft = false
images = [ "/uploads/venmo-wrapped-2022.jpg" ]
tags = [ "venmo", "wrapped", "data" ]
title = "Make Your Own Venmo Wrapped"
toc = true
_template = "blog_post"
+++


I'm no data scientist, but inspired by Spotify Wrapped (and even more so by [Jimmy John's Wrapped](https://twitter.com/BBarberie/status/1608476950108209158)) - let's continue the [descent into year-end madness](https://news.yahoo.com/spotify-wrapped-created-end-menace-194534713.html) and make a Venmo Wrapped!

{{< bootstrap-panel title="Venmo's Past Folly" >}}
_Unfortunately, I can't promise this wil be as exciting as when Venmo left everyone's transactions public for the world to explore - making it easy to identify drug dealers, rough locations & schedules, and more -> [https://publicbydefault.fyi/](https://publicbydefault.fyi/)._
{{< /bootstrap-panel >}}

## My Venmo Wrapped 2022

To give you a preview of what sort of info you might find in your own Venmo Wrapped, here's mine for 2022 (omitting any sensitive details like specific people or amounts) and a [**copyable Sheets template**](https://docs.google.com/spreadsheets/d/1MotwaQm1jyDeqBVdD-ZTFTSHyvJyyI9gSwjt3B9_Wa0/edit?usp=sharing):

{{< figure src="/uploads/venmo-wrapped-2022.jpg" alt="Venmo Wrapped 2022 summary with transaction counts, a weekday chart, and percentages of notes with emojis or jokes." caption="My 2022 snapshot. The main statistics are listed in text below." >}}

* ⏳ Oldest incomplete request is **1,332 days** _(May 10, 2019)_
* 👯 **45** people interacted with
* 🏢 **2** businesses
* 🤷 **49/64** of my requests went to one person
* 😶 **20%** of notes I received had emojis
* 🃏 **25%** of notes I received were a joke of some sort (e.g. `no u`, `💩`)
* 📅 I sent the majority of my requests on **Sun & Mon** _(63%)_

If this piqued your interest at all, read on to make your own! I'll add a list farther down of other interesting stats you might want to explore.

## Google Sheets Template

To make your own, there is a [**copyable Sheets template**](https://docs.google.com/spreadsheets/d/1MotwaQm1jyDeqBVdD-ZTFTSHyvJyyI9gSwjt3B9_Wa0/edit?usp=sharing). Directions for how to gather your transactions & update the sheet are below.

## How to make your own Venmo Wrapped

Making your own Wrapped only takes a few steps:

1. Gathering the CSV file of transactions from the Venmo website.
   1. Go to your [`Venmo Statements` tab](https://account.venmo.com/statement).
   2. In a new tab, use the URL `https://account.venmo.com/api/statement/download?startDate=2022-01-01&endDate=2022-12-31&csv=true`{{< new_tab_link title="[↗️]" url="https://account.venmo.com/api/statement/download?startDate=2022-01-01&endDate=2022-12-31&csv=true" >}}  
   to download the full year of transactions history. [_⚠️ Why download this way._](#-why-download-this-way)
2. Copy them into either the [**Google Sheet template**](https://docs.google.com/spreadsheets/d/1MotwaQm1jyDeqBVdD-ZTFTSHyvJyyI9gSwjt3B9_Wa0/edit?usp=sharing) I made _(or one of your own)_.

   > ℹ️ _The data needs a small amount of cleanup before it's ready._
   > * _In the transactions file, you can ignore the extra meta information above the line starting with `,ID,Datetime`. You can also skip the last line that has `In case of errors...`. Only copy the actual transactions in between to your Sheet._
   > * _For the `Amount (...)` fields, we need to make sure Sheets understands they are numbers, so we have to do a find & replace for `+ $` to an empty string \`\`. My Sheets automatically understood the negatives were numbers, though you may need to find & replace `- $` with `-` if it doesn't._
3. Update your Venmo name in the specified Sheet cell. This is used in queries like _"# of charges I (you!) sent"_.

A bunch of default stats will populate, then the sky is the limit for digging through your own data!

### ⚠️ Why download this way

Why use the URL I suggested above to get transactions? To save you the annoyance of downloading 12 files and manually combining them.

Venmo only gives the option to download 1 month of transactions through CSV at a time - or so it seems. I noticed their API doesn't restrict you from requesting more than 1 month, so by modifying the URL params (called `startDate` and `endDate` in the URL above) we can have it download the entire year in one swoop.

[🔙](#how-to-make-your-own-venmo-wrapped)

### Venmo terminology

To make sure we're on the same page, my understanding of some of the vocabulary used in their CSV file [_(if I'm wrong, please let me know!)_](#make-your-own-venmo-wrapped):

* **Payment** - Money was directly sent. Negative values are when I (you) paid someone else.
* **Charge** - Money was requested, and then accepted. Negative values are when someone else requested me (you).
  * _To the best of my knowledge, the `date` on a Charge is when it was accepted & paid._

### Useful Google Sheets-fu

If you want to expand on the concepts in Venmo Wrapped, there's a few utilities in Google Sheets I picked up to make things easier:

* `query()` is super handy, especially if you have more SQL experience than spreadsheets. There's a [helpful blog post with examples](https://www.benlcollins.com/spreadsheets/google-sheets-query-sql/), and the [Google reference doc](https://developers.google.com/chart/interactive/docs/querylanguage#overview) if you need to dive deeper.
* `{;} and {,}` is called Arrays - I was able to use this to line up the days of my two WeekDay queries (to split Payments & Charges into 2 columns) and easily create a chart out of the combo.

## Potentially interesting data points

After showing a few friends, I started getting feedback with ideas - I haven't explored them, but maybe one of the curious citizens of the internet will find them interesting:

* Time of day trends, similar to Spotify's _"Your morning aesthetic was X_"
* What time of day am I most likely to pay?
* day/week/month of the year you were most active
* Most common words/emojis in the notes
* Top 3 "givers" vs "moochers" - basically people that give you money, and people that take your money
* Who paid the slowest/fastest? *_(I don't believe this is possible with existing CSV data provided by Venmo)_
* < Your great idea here >...

## Make your own Venmo Wrapped

Feel free to borrow the [Google Sheets Template](https://docs.google.com/spreadsheets/d/1MotwaQm1jyDeqBVdD-ZTFTSHyvJyyI9gSwjt3B9_Wa0/edit?usp=sharing) I used, and let me know what you come up with on [Twitter @maybekq](https://twitter.com/maybekq) or [email](mailto:kevinquinnfun@notxss.anonaddy.com)! I'd love to see what the data wizards of the world come up with, or if there are bugs in my queries.

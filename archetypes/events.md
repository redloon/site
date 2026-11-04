+++
title = "{{ replace .Name "-" " " | title }}"
date = {{ .Date }}
draft = false
noindex = true
event_key = "REPLACE_ME"
emoji = "🎉"
timezone = "America/Chicago"
location = "TBD"
# location_url = "https://maps.app.goo.gl/..."
# cost = "Free"
# bring = "Whatever you want"
# food = "Provided / BYO / nearby options"
# dress = "Casual"
# rain_plan = "Cancelled if raining — check back"

[[event_dates]]
  date = "2006-01-02T19:00:00Z"
  # duration_hours = 3  # optional — defaults to 2
  # note = "Optional small note shown under the date"

[build]
  list = "local"
  render = "always"

[sitemap]
  disable = true
+++

Event details go here.

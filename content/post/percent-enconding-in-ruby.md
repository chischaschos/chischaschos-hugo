+++
date = "2018-04-23T13:00:00-07:00"
title = "TIL percentage encoding in ruby"
draft = false
tags = ["til", "notes"]
+++


## Motivation
Document what options exist in Ruby whenever in I want to do [Percent-Encoding](https://en.wikipedia.org/wiki/Percent-encoding).

## What is Percent Encoding?
- A. K. A. URL encoding.
- Used to create URIs (both URLs and URNs).
- Used to prepare data of the `application/x-www-form-urlencoded` media type.
- There is a *reserved character set*: `!*'();:@&=+$,/?#[]`


## What do we have in the Ruby library?

## References
-[URI.escape vs CGI::escape](https://stackoverflow.com/questions/2824126/whats-the-difference-between-uri-escape-and-cgi-escape)

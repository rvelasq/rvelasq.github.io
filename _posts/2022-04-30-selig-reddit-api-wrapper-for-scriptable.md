---
layout: post
title: "Selig - Reddit API Wrapper for Scriptable"
categories: [scriptable]
tags: javascript, scriptable, reddit, api
comments: true
---

Bask in 2017, I started using a new Reddit app called [Apollo](https://apolloapp.io) by [Christian Selig](https://christianselig.com). It boasts IOS-native UI design languange so it really feels like an app that really belongs to IOS. I have been using it since.

This has become an inspiration of creating a Reddit client of sorts, but not on the same road as Apollo.

<!--more-->

## Story Time

As a Shortcuts & Scriptable user, I often explore how can I use them for online services that provide an API. It's easy when the API provides access via API keys that grant access to all features. Tricky ones are those that use OAuth2, where the user needs to authenticate in order to use API endpoints in the context of the user. 

So I did my research, using Spotify as target, and have written about the outcome over at the [Automators forum](https://talk.automators.fm/t/building-a-general-purpose-oauth-redirect-proxy-for-shortcuts-and-scriptable/4420). That turned out quite well but the downside is a proxy website is needed to redirect the flow back to Shortcuts or Scriptable.

Then one day, I found that `https://open.scriptable.app` exists to trigger scripts using the url `https://open.scriptable.app/run/Script_Name`. What more, you can even pass parameters to it via the query string. This is good, I can use it as the proxy instead.

Which brings be back to Apollo. With it being a great IOS citizen, I have wished that it would also have Shortcuts support so that I can also automate reading data or posting to Reddit. Since I don't think that's happening, time to get my hands dirty and build a Reddit client myself.

## Selig was born

In the succeeding days, I have worked on the script until the point where I feel it has fulfilled it's 2 objectives - (1) to read Reddit data and (2) to be able to post text, images and videos.

Source code and setup instructions can be found at [https://github.com/supermamon/scriptable-selig](https://github.com/rvelasq/scriptable-selig). 


Below are some screenshots:

![On-boarding](https://github.com/rvelasq/scriptable-selig/raw/master/docs/img/05-first-run.png)

![Configuration](https://github.com/rvelasq/scriptable-selig/raw/master/docs/img/06-client-details-entry.png)

![Main Menu](https://github.com/rvelasq/scriptable-selig/raw/master/docs/img/08-menu.png) |




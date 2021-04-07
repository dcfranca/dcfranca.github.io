---
layout: post
title: Browser Isolation
---

CloudFlare has just announced what they call zero-trusted browsing.

The services, aiming on business, has the goal of making the employee experience feel less restrictive in comparison to regular organization policies, while keep the browsing safe.
The way it works is different than regular remote browsing, and they claim it is what it makes it faster and a frictionless experience.

If they delivery what they are promising it might set another level on business security without compromising the employee experience, as browsers has become the main attack vectors, but it all comes with a cost, security is not free after all.

This technology came from their acquisition of a company called S2 Systems, it's a proprietary patent that attempts to tackle the main issues of the current remote browser isolation technologies.

The current technologies are "Pixel Pushing" and "DOM Reconstruction".

* Pixel Pushing: The rendered page is transmitted pixel by pixel, similar to remote desktops, this approach tend to be very slow and produces a terrible user experience.
* DOM Reconstruction: The server "cleans" the DOM and send it back to the client, this approach is not as slow as the first one, but can break pages or even let malicious code pass undetected.

S2 System introduced what they call "Network Vector Rendering" or NVR, this technology utilizes Chromium [Skia 2D library](https://skia.org/) to send compressed Skia draw commands to any HTML5 compliant browser.
In order to run it the local browser will have a NVR Web Assembly library responsible for draw the commands on the local window, and on the cloud it runs a headless Chromium-based browser.

I'm curious to try this approach, My only experience with browser isolation was running Citrix and its awful usability.
Hopefully, CloudFlare will set the bar higher for this one, and make organizations a bit more... secure.

If you want to read more about CloudFlare browser isolation, check their [official page](https://www.cloudflare.com/teams/browser-isolation/)


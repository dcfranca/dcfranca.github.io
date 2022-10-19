---
layout: post
title: The confusion about Kubernetes requests
---


There is a famous quote in computer science:

> "There are only two hard things in Computer Science: cache invalidation and naming things."

Name things is hard because you need to consider the function of the thing you are naming, not only in the present but also in the future, and yet to predict all possible confusions that might arise in people's minds.

Recently, working on a project that required bringing awareness about resource utilization on a Kubernetes cluster, I noticed a common theme for confusion: Kubernetes `resources requests` and `resources limits`.

After analyzing ~~hundreds~~ **three** messages, I came to the conclusion that the confusion has a clear culprit: the bad naming for `requests`, `limits` is not that bad, `requests` on the other hand.

**Requests** is an extremely generic term, and everyone knows that the rule #1 of naming is: **DO NOT** use a generic name.
**Requests** can mean anything, and the context of resources adds absolutely *zero* to the clarification.

I'm sure all the confusion would have gone away if, instead of requests, the Kubernetes team had picked a name that fits the function better, like `guaranteed` or `reserved`, because that is what it does, reserve or guarantees the resources specified there.

Then instead of `resources requests`, it would be `resources reserved`, it looks much clearer now, isn't it? People would see and immediately understand that this is the amount reserved for their pod, and there is no more need to decode what the hell `requests` mean in this context.

But now it is too late; there's no going back. We will need to live with it until the end of the days, or until some `Kubernetes` breaking change; who knows?

So next time you need to understand it, just replace `requests` with `reserved`; that is the amount of resources *(CPU/Memory)* your pod will have **reserved**, **guaranteed**, and the `limit` is the amount it can't exceed.

Doesn't it make it easier?
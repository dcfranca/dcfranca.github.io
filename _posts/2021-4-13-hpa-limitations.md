---
layout: post
title: When Horizontal Auto Scaler is not fast enough
---

Recently I was listening to a podcast with the brazilian team responsible for probably the most popular fantasy game in Brazil: [Cartola FC](https://globoesporte.globo.com/cartola-fc/).
Cartola FC is a fantasy game based on real football matches, and in a country that loves football as much as Brazil you can imagine the load they receive when a match is ongoing.
The number of requests on their application has a very high variability, depending on whether there is a football match happening or not and whether some popular players have scored.

![_config.yml]({{ site.baseurl }}/images/neymar.jpg)

During these moments when a popular player has scored, the users wanna quickly check their status on the game, and then their API access can go, in a couple seconds, from ~30k requests por second to ~230k.
Of course, when it happens they want to be able to have enough resources to not frustrate the user (he will be frustrated enough watching his selected players missing the goal).

There are already one big factor that makes this team "unique", their infrastructure is on-premises. In a world where most of the computing power is centred on Amazon, Google and Microsoft it is sure a risk decision per se.
They are asked about how they scale their pods when those spikes happen, and they comment that they don't use things like [Kubernetes Horizontal Auto Scaler](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/), and the reason is quite simple, with the spike happening in a couple seconds the auto scaler is not fast enough to provision the extra pods.

Reading online about the *Kubernetes* HPA most of time its limitations are not mentioned and it sounds more like magic than actual technology.

At my current company we have a similar issue, as a tv provider, when a new football championship or Formule 1 season is about to start our APIs experience a ten fold increase in traffic, and even with HPA in place, sometimes they are just not fast enough.

While a pod is being prepared to become ready for new requests, all the load is put on the back of the current pods, eventually they can't handle the requests anyre and then they become unhealthy, so *Kubernetes* takes them out of the pool and the problem only gets worse.

There is also other limitations like costs, nodes available and database connections, one failure can propagate the error down the hill becoming harder to recovery.

It is an ongoing and learning process, and we are playing with different scenarios and solutions in order to optimize the resources usage.


A few things that can help to mitigate the issue:


## Scheduled provisioning
When an known event that increases traffic is about to happen you can provision before hand the necessary pods, increase the number of minimum pods, this is something that gets better as more data of past events you have.
You also can use tools for load testing, i.e: [Locust](https://locust.io/) to simulate the number of requests you expect.
Unfortunately, not always it is possible to predict when it will happen.

## Higher liveness probe
This way your pod is not killed while it waits for the new pods to enter the pool, but make sure that you don't let it too long to have stalled pods.

## Increase sensibility of the HPA trigger metrics
Another approach you can try is to make the pods more sensible to trigger the scale more often and give more time for the current pods to handle the requests.

In summary, sometimes, scaling your application in a proper way is more of an art than a science, and you have to weight all the trade-offs in each possible solution, mix them, test and analyze to find the optimum way according to your situation, and then keep re-evaluating it and improving it, as new solutions and tools never stop coming.

*Kubernetes HPA* is an extraordinary tool, but it needs to be separated from the hype, like mostly things in this industry, but this a topic for another post.

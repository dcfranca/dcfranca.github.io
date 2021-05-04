---
layout: post
title: Google FLoC {Insert some F*K pun here}
---

I'll have to put a disclaimer on the beginning of this post: I don't like Google, its practices, its mindset, their business model and everything in between. I have *degoogled* myself as much I could, and I keep looking for better alternatives in the sense of usability, privacy and security.
However, on this post I'd like to go beyond the buzz created by their *FLOC ID* recently announced and already under attack for all the sides, based on its reception (Brave/Vivaldi/Firefox, all already said they won't implement it) it looks like it is an *dead-on-arrival technology*. Does it really deserve all the hate?


FLoC(Federated Learning of Cohorts) is part of the [Privacy Sandbox Proposals](https://www.privacysandbox.com/#:~:text=One%20proposal%20in%20the%20Privacy%20Sandbox%20is%20to,method%20is%20called%20Federated%20Learning%20of%20Cohorts%20%28FLoC%29.) aiming to satisfy advertisers needs without third-party cookies and other tracking mechanisms.
The idea behind FLoC is to cluster large groups of people to make them unidentifiable as individuals, but still relevant to ads in the form of a generic profile that they call FLoC cohort. In this sense, FLoC is more of a profiling tool than a tracking one.

Is it even possible? After the announcement a lot of criticism has been thrown on the proposal, some of them are valid points, some of them seem to not understand how it works.

EFF was one of the first ones to [attack the proposal](https://www.eff.org/deeplinks/2021/03/googles-floc-terrible-idea), they raise a valid point; Google is saying that a cohort ID can't fingerprint a user due to how generic it is (on its first implementation it uses a 8-bit identification, which makes it only 256 possible cohorts), however, in combination with other fingerprinting techniques it can helps to identify a user even easier than now.
Google, on the broader Privacy Sandbox, has other proposals to mitigate fingerprinting:

* Creating *Privacy Budget* limits, where the website has a limited access to information that could be used for fingerprinting.
* The User-Agent won't give the full information that is shared Today, instead they will expose an API, which will be limited by the *Privacy Budget* above.
* IP Security will make the IP available on request and part of the *Privacy Budget*

However, as pointed by EFF, none of these are currently implemented, and FLoC is already on Original trial, so for now we don't have the other benefits, only the cons of the FLoC being used in conjunction with other techniques for fingerprinting.

The cohort is based on your web history, renewed weekly, and supposed hiding sensitive information to avoid homogeneity attacks, websites categorized as "sensitive data", like medical or adult content won't be showed there(the sensitive categories are the same already defined by Google for advertisement), this is achieved by ensuring that no cohort has more users who has visited sensitive websites more often than the general population, this approach is explained in more details in [this paper](https://docs.google.com/a/google.com/viewer?a=v&pid=sites&srcid=Y2hyb21pdW0ub3JnfGRldnxneDo1Mzg4MjYzOWI2MzU2NDgw#:~:text=In%20the%20context%20of%20the%20FLoC%20API%2C%20a,visited%20a%20website%20about%20a%20rare%20medical%20condition.).

They also will be auditing running frequently to identify cohorts that could make correlations leading to discrimination, again, not clear how efficient this can happen.
FLoC delegates most of computing to the browser, keeping it locally, which will be responsible to calculate your cohort applying a SIMHash algorithm and expose it to the website via a browser API as below:

```javascript
cohort = await document.interestCohort();
url = new URL("https://ads.example/getCreative");
url.searchParams.append("cohort", cohort);
creative = await fetch(url);
```

Another criticism is that as the cohort changes overtime, a website could use to collect more and more information of the user, creating a much broader profile. Google has a proposal for tackling this on their [Longitudinal Privacy](https://github.com/WICG/floc#longitudinal-privacy) section: Sticky the same exposed cohort for each website you visit. Therefore, the first time you visit a website it will gain access to your cohort, which will remain the same on next visits, even if your local one has changed. However, this is just a proposal and raises other concerns per se.

Also, using a [**Default Allow**](https://github.com/WICG/floc#opting-out-of-computation) is enough to raise some eyebrows.
There is also some concerns about Google enrolling users on the trial without consent, both from user's and from the websites.

In summary, I think there are some good ideas, ideas that may work better in terms of privacy than the current third-parties cookies approach (not a high bar, I know). However, for it to make sense, not only the FLoC from the *Privacy Sandbox* needs to be implemented, but most, if not all, of its proposals.
Adding only FLoC to the mix is not replacing third-party cookies or fingerprinting, it is just adding another layer of data collection while preserving the *web status quo*, not the user's privacy.
I don't think FLoC is where we should start this. Maybe starting from the [*Privacy Budgets Limits*](https://github.com/bslassey/privacy-budget) and [*Trust Token API*](https://github.com/WICG/trust-token-api) will have a better reception?

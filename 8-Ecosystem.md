# Ecosystem

The ecosystem around Zemanta and our DSP product is very hard to grasp, if you're new to the advertising industry in general. The topics covered below will help you get a better grasp of the daily glossary used in code, meetings and conversations. 

## Native

Zemanta is building a "native" DSP. Native advertising essentially means - advertising that does not break user's flow. 

Analogy: you buy an automobile magazine for the purpose of looking at great pics of cars. In the context of such a magazine, if you'd encounter an advertisment that would advertise funeral services, that adverisement would break your flow and intention of looking at great car pics. On the contrary, if you'd encounter an advertisment of a great car in an automative magazine and that would not break your flow and intention. 

Similar to the analogy above, banner ads break user's flow of seeking out content on the web. A native ad, does not break your flow and looks like relevant content for you to digest. 

## Supply and Demand

You'll hear these two terms a lot and you'll also encounter them directly in code. 

* **Supply** - our business partners that supply us with inventory which means space to place ads on websites and apps against payment
* **Demand** - advertisers using our platform, who have money to ship ads to websites

## Publisher

Ads are shown on web sites and in apps. In the advertising ecosystem, we use the term "publisher". Don't get confused, this is not a person or an entity in the context of news making or book printing.

## SSP - Supply Side Platform

This is a type of a business in the advertising ecosystem that builds technology to run ads for publishers. Publishers on their own don't develop this tech and usually partner with one or more SSPs to supply themsleves with the following capabilities:

* Displaying ads on various placements on their website
* Running directly sold campaigns i.e. some large publishers might have their own ad sales teams who sell ad campaigns directly to advertisers
* Making their ad placements available to other businesses in the advertising ecosystem covered later on in this guide


## Exchange

We've mentioned that SSPs enable publshers to make their ad placements available to other businesses in the ad ecosystem. Such a busines is usually an "ad exchange". Exchanges partner up with publishers and SSPs for the purpose of collecting as many of "requests to show ads" comming in from a diverse set of websites and apps offering those requests. 

The exchange's responsibility is to forward each such request to show an ad to other businesses who might offer money for those impressions, perform an auction and forward an ad of the winner of such an action back to the publisher or SSP.

### Real Time Bidding

The responsibilty of an exchange described above is essentialy what **R**eal **T**ime **B**idding protocol (RTB) does.

* Entity (either SSP or a publisher) forwards a request to show an ad to an exchange
* Exhange forwards a **bid request** to all other entities interested in such a request for an ad 
* Bidders respond with a **bid response** that includes: price offered for such an ad, the ad itself
* Exchange combines all bid responses and determines a winner based on an auction
* Exchange sends the winning ad to the SSP or publisher and notifies the winner asynchronously for billing purposes


## DSP - Demand Side Platform

DSPs offer tehnology directly to adverisers and advertising agencies that offer holistic advertsing services to to businesses who want to advertise their products and services. 

Entities plugged into exchanges are usually DSPs who implement the RTB protocol and offer means to run adversing camapigns utilizing OpenRTB as the main protocol .








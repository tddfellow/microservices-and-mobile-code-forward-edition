# MicroServices and Mobile

(Code Forward 2.0, Warsaw)

**slides: [bit.ly/microservices-mobile-2-0](http://bit.ly/microservices-mobile-2-0)**



<img src="../my-presentation-template/me.jpeg" class="photo-me">

## Oleksii Fedorov

Software Crafter

[@waterlink000](https://twitter.com/waterlink000)

Note:
I am Software Engineer, Software Crafter, I practice eXtreme programming and LEAN and I call myself TDD Fellow. Also, I blog and mentor.


<!-- -- data-background="url(./Pivotal_WhiteOnTeal.png) no-repeat center" data-background-size="cover"  -->

Note:
I work at Pivotal Labs as a software engineer and a consultant.



## Tale of a
# Single IF Statement


## Backstory

- App with places
- Opening hours
- 3rd party API

Note:
Johan and I were working on the mobile app. App was showing certain places on the map. Users were interested in the opening hours. We were using the external API provider to get the places' data. We were using the `opening-hours` field. API was returning opening hours for today. That is exactly what our users were interested in.


<div class="presented-code">
{
  ...
  "opening-hours": [
    "9-14",
    "17-23"
  ],
  ...
}
</div>

Note:
In the documentation it was a list of `starting hour` and `end hour` pairs. For example: `9-14, 17-23`.


## Everything worked fine

> This is fine!

Note:
Everything worked fine. Then we started getting bug reports for our app. Some places were showing really weird long numbers instead of hours. We struggled debugging this problem. It seemed non-reproducible. Finally, we got a hold of a specific place.


<div class="presented-code center-text">
Opening Hours: 1002740001 h -  h
</div>

Note:
It would consistently show weird ten-digit number instead of opening hours. It was very challenging to find an error in our code. Everything seemed to work as expected. Finally, we realized, that the problem was not in our code. It was in our understanding how the external API worked. The API was indeed returning a ten-digit number instead of opening hours. It wasn't mentioned in the API documentation. We have contacted the support.


<div class="presented-code">
1002740001  ->  24/7
1002740002  ->  only workdays
1002740003  ->  only holidays
...
</div>

Note:
It turned out to be an undocumented feature of the API. Special values to indicate special cases, such as `24/7`, weekend-only, and others. They have sent us a list of 25 special numbers with their meaning. We had to support these special cases. We also had to render them differently in our mobile app.


## Quick Fix for 24/7

- one commit
- one test
- one `if` statement

Note:
This was already going on for weeks. Our users were frustrated. We had to do something fast. We did a quick search on our logging system. One of the special numbers occurred much more often than others. Like twenty times more often. It was `24/7` value. So we decided to deploy a quick fix for that number. It took us only one git commit to do that. One unit test and one if statement. We have deployed it.


# 😊

Note:
And it immediately made majority of our users happy.


# 😞

Note:
No, it didn't. I have just lied to you. We had to wait for Apple to review our new release for 5 days. Wouldn't it be great to be able to release immediately? Wouldn't it be great to deliver value as soon as you can?



# Fast
### Iterations to Production
# For Everyone

(on mobile platforms)

Note:
Wouldn't it be great to have very fast iterations to production on mobile platforms?



## As Industry
#### We Need to
# Advance
#### and
# Reject Manual Review
## As Bad Practice



### The Problem:
# Mobile Waterfall

> That is Waterfall. Right there.

_ [tddfellow.com/blog/2016/12/07/mobile-waterfall-being-agile-again/](http://www.tddfellow.com/blog/2016/12/07/mobile-waterfall-being-agile-again/) _


#### 2-4 Day
# Manual
#### Review Releases

Note:
1) package build; 2) upload to vendor; 3) push button in vendor UI; 4) vendor verifies security and TOS concerns in a half-automated way; 5) wait half week; 6) reject; 7) fix problem; 8) repeat.


#### Bug Fix?
# half a week


#### Testing Assumption
## Quickly
#### With Canary Release?
# forget it


#### You Should Be Getting Your Feedback
# in Minutes
## not days


#### Extensive End-To-End Testing Which Is
# Slow And Flaky


#### Extensive
# Manual
#### QA before release



#### Question:
## Aren't Beta Users Enough?


#### Counter-Question:
## Can We Deploy 10 Versions
# At The Same Time?


# Getting our Feedback Back


## Build the Responsive Web App

> The (Native) App is Dead; Long Live the (Web) App

_ https://thejournal.com/articles/2015/10/05/the-app-is-dead.aspx _


### Web App - Pros

- No knowledge of the mobile platform required
- Write code only once: for web and mobile
- Only one deployment strategy
- No "installation" required
- Portable
- Linkable and shareable
- Always up-to-date


### Web App - Cons

- Native app features are unavailable
- Users do not discover the app where used to
- Lower chances of regular usage (on avg.)
- Can not execute in a background mode
- Unlikely to save the app to the home screen
- Slow performance


## Build Progressive Web App

> Utilizes modern web tech to deliver native-like experience


### Progressive Web App - Pros

- Just like a web app
- Emulated native app look and feel
- \* Has access to native features
- "Installs" to the home screen
- Higher chances of regular usage


### Progressive Web App - Cons

- Teaching user to "Save to home screen"
- Slow performance
- Cross-platform support is low, e.g.

  _ on some platforms: _

  - can not execute in background
  - can not send push notifications
  - can not access hardware features


### Example Progressive Web App

**these same slides: [bit.ly/microservices-mobile-2-0](http://bit.ly/microservices-mobile-2-0)**


## Run the Hybrid App

> Best and Worst of Native & HTML5


### Hybrid App - Pros

- Mostly, still a web app
- Native app
- Native look and feel is emulated
- All native & hardware features
- Kind of portable
- Installs via the application store
- Regular usage is on par with Native App
- \* Quicker deployment cycles


### Hybrid App - Cons

- Slow performance
- Requires more development skills


## Roll Your Own Solution


- Responsive Web App
- Progressive Web App
- Hybrid App
- ### Roll Your Own



## Domain-Specific Language

deployed to your servers


### Parsed & Interpreted

by your mobile application


### Example

When user likes an item  
We would like to show them recommendations


<div class="presented-code">

{
  "name": "RecommendationService",
  "subscribe": {
    "event": "liked_item",
    "actions": [
      {
        "request":
          "/recommendations/${event.item_id}",
        "publish": "rcv_recommendations"
      }
    ]
  }
}
</div>


<div class="presented-code presented-code--with-highlights">

{
  <span class="presented-code__highlight">"name": "RecommendationService"</span>,
  "subscribe": {
    "event": "liked_item",
    "actions": [
      {
        "request":
          "/recommendations/${event.item_id}",
        "publish": "rcv_recommendations"
      }
    ]
  }
}
</div>


<div class="presented-code presented-code--with-highlights">

{
  "name": "RecommendationService",
  <span class="presented-code__highlight">"subscribe"</span>: {
    <span class="presented-code__highlight alternative">"event": "liked_item"</span>,
    "actions": [
      {
        "request":
          "/recommendations/${event.item_id}",
        "publish": "rcv_recommendations"
      }
    ]
  }
}
</div>


<div class="presented-code presented-code--with-highlights">

{
  "name": "RecommendationService",
  "subscribe": {
    "event": "liked_item",
    <span class="presented-code__highlight">"actions": [
      {
        "request":
          "/recommendations/${event.item_id}",
        "publish": "rcv_recommendations"
      }
    ]</span>
  }
}
</div>


<div class="presented-code presented-code--with-highlights">

{
  "name": "RecommendationService",
  "subscribe": {
    "event": "liked_item",
    "actions": [
      {
        <span class="presented-code__highlight">"request"</span>:
          <span class="presented-code__highlight alternative">"/recommendations/${event.item_id}"</span>,
        "publish": "rcv_recommendations"
      }
    ]
  }
}
</div>


<div class="presented-code presented-code--with-highlights">

{
  "name": "RecommendationService",
  "subscribe": {
    "event": "liked_item",
    "actions": [
      {
        "request":
          "/recommendations/${event.item_id}",
        <span class="presented-code__highlight">"publish"</span>: <span class="presented-code__highlight alternative">"rcv_recommendations"</span>
      }
    ]
  }
}
</div>


<div class="presented-code">
$ curl example.org/srv/recommendations
{
  "name": "RecommendationService",
  "subscribe": {
    "event": "liked_item",
    "actions": [
      {
        "request":
          "/recommendations/${event.item_id}",
        "publish": "rcv_recommendations"
      }
    ]
  }
}
</div>


<div class="presented-code">

{
  "name": "RecommendationRenderService",
  "subscribe": {
    "event": "rcv_recommendations",
    "actions": [
      {
        "render": { "view": "item_list",
            "data": "${event.response.items}" }
      }
    ]
  }
}
</div>


<div class="presented-code presented-code--with-highlights">

{
  <span class="presented-code__highlight">"name": "RecommendationRenderService"</span>,
  "subscribe": {
    "event": "rcv_recommendations",
    "actions": [
      {
        "render": { "view": "item_list",
            "data": "${event.response.items}" }
      }
    ]
  }
}
</div>


<div class="presented-code presented-code--with-highlights">

{
  "name": "RecommendationRenderService",
  <span class="presented-code__highlight">"subscribe"</span>: {
    <span class="presented-code__highlight alternative">"event": "rcv_recommendations"</span>,
    "actions": [
      {
        "render": { "view": "item_list",
            "data": "${event.response.items}" }
      }
    ]
  }
}
</div>


<div class="presented-code presented-code--with-highlights">

{
  "name": "RecommendationRenderService",
  "subscribe": {
    "event": "rcv_recommendations",
    <span class="presented-code__highlight">"actions": [
      {
        "render": { "view": "item_list",
            "data": "${event.response.items}" }
      }
    ]</span>
  }
}
</div>


<div class="presented-code presented-code--with-highlights">

{
  "name": "RecommendationRenderService",
  "subscribe": {
    "event": "rcv_recommendations",
    "actions": [
      {
        <span class="presented-code__highlight">"render"</span>: { <span class="presented-code__highlight alternative">"view": "item_list"</span>,
            "data": "${event.response.items}" }
      }
    ]
  }
}
</div>


<div class="presented-code presented-code--with-highlights">

{
  "name": "RecommendationRenderService",
  "subscribe": {
    "event": "rcv_recommendations",
    "actions": [
      {
        <span class="presented-code__highlight">"render"</span>: { "view": "item_list",
            <span class="presented-code__highlight alternative">"data": "${event.response.items}"</span> }
      }
    ]
  }
}
</div>


<div class="presented-code">
$ curl example.org/srv/recommendations-render
{
  "name": "RecommendationRenderService",
  "subscribe": {
    "event": "rcv_recommendations",
    "actions": [
      {
        "render": { "view": "item_list",
            "data": "${event.response.items}" }
      }
    ]
  }
}
</div>


<div class="presented-code">
$ curl example.org/srv/
{
  "services": [
    "/srv/item-catalog",
    "/srv/recommendations",
    "/srv/recommendations-render"
  ]
}
</div>


## Meta Programming


### Trade-offs

- Might get rejected
- Complexity of DSL implementation
- 5-10% still deployed "old-school"


### Choose your own trade-off

- performance
- development cycles
- cost-efficiency
- etc.


### DSL => Hybrid + Programming Language



## MicroServices Properties

Note:
Componentization, Bounded Contexts, Single Responsibility, Loose Coupling, Independent Deployability, --Independent-Scalability--, Decentralization, Evolutionary Design, Independent Replaceability and Independent Upgradeability.



## Pseudo-"MicroServices" Architecture


![start](./img/start.png)
<!-- <img src='http://g.gravizo.com/g?
  digraph G {
    Main -> Backend [label="on start & on internet"];
    Backend -> Main;
    Main -> DSLInterpreter;
    DSLInterpreter -> ItemCatalogService [label="builds"];
    DSLInterpreter -> RecommendationService [label="builds"];
    DSLInterpreter -> RecommendationRenderService [label="builds"];
  }
'/> -->


![start](./img/services.png)
<!-- <img src='http://g.gravizo.com/g?
  digraph G {
    MessageBus [shape="box"];
    User -> ItemCatalogService [label="likes an item"];
    ItemCatalogService -> MessageBus [label="publish: liked_item"];
    MessageBus -> RecommendationService [label="subscribe: liked_item"];
    RecommendationService -> RequestAction [label="/recommendations/42"];
    RequestAction -> Backend;
    Backend -> RequestAction;
    RequestAction -> MessageBus [label="publish: rcv_recommendations"];
    MessageBus -> RecommendationRenderService [label="subscribe: rcv_recommendations"];
    RecommendationRenderService -> RenderAction [label="item_list+data"]
    RenderAction -> UI;
  }
'/> -->



## The Best Alternative

(not technical and political)


### Just Iterate
## On Mobile Platforms
# Quickly


## As Industry
#### We Need to
# Advance Forward
#### and
# Reject Manual Review
## As Bad Practice



## Q&A



# Demo

- iOS/Swift App: https://github.com/waterlink/LikeyLikes
- Fake Static API: https://github.com/waterlink/LikeyLikesStatic
- Demo Video: https://git.io/vDJKG


## Thanks

Twitter: [twitter.com/waterlink000](https://twitter.com/waterlink000)

Github: [github.com/waterlink](https://github.com/waterlink)

Blog: [tddfellow.com](http://tddfellow.com)

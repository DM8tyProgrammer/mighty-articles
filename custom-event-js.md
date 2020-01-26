---
title: Publishing Custom Events Publisher with RxJs
subtitle: Introduction to Event-Driven Programming
---

Event-Driven programming paradigm¹ introduces an intuitive flow of control based upon events.

> Button is clicked => show dialog.

Web UI offers inbuilt Event Notification model, but _it is only useful when you are dealing with GUI / DOM² elements_. There is no support for non-DOM elements.

_This article showcases building up custom event publisher-subscriber model on top of Reactive Programming._

## Motivation

With time, web applications have grown up; also, Javascript started participating in backend applications; which in turn made the world to see Javascript to more than HTML DOM² manipulation gig.

## Reactive Programming

Reactive Programming is a trending paradigm these days. It is the processing of streams of data asynchronously. At core, reactive streams are written on top of the event-driven paradigm (?). You are going to build an event notification model on top of it by _modelling events as a stream of data_.

[RxJs](https://rxjs-dev.firebaseapp.com) is a reactive extension for Javascript. It offers `Subject` which can be used to emit events to multiple subscribers.

## In Action

### Lingo

Due to variation in technologies in case of Event-Driven, there exist several names for a single concept.

- One who emits event can be called Event Publisher or Event Emitter or Event Dispatcher.
- One who listens to events is called Event Subscriber or Event Listener or Event Handler.
- One who caused event is event source.

In general, **Publisher-Subscriber** or **PubSub** term is used collectively for event publisher and even subscriber.

---

Consider a hypothetical case (maybe real?). Student notifies her Parent on the exam result, and Parent reacts accordingly.

```js
import {subject} from rxjs
// Event Publisher
class Student {
  constructor() {
  this.subjects = {
     passed : new Subject();
     failed : new Subject();
   }
 }

 // Registration
 on(event, action) {
   // santize event..
   this.subjects[event].subscribe(v => action(v))
 }

}
```

```js
class StudentParent {

  Student kid;



  // registration of notification
  kid.on('passed', () => shout('Hurray !!'))
  kid.on('failed', () => sad());

}
```

You might have noticed:

- There are two `Subject` objects. You can also create a single `Subject` object named result with status failed or passed. Separating the event or combining the event is Design Concern which may depend upon many factors.
- Event Publisher is holding event subscription data.

## Fiddling

Any variation you make with the above code itself a pattern:

- Keeping event subscription data in the mediator class of Event-Emitter and Event-Listener is in-memory Event Bus pattern.
- If the event holds enough information that Event-Listener doesn't look back to Event-Emitter; it is Event Carried State Transfer pattern
- If you store all events in datastore, it is Event Source pattern (possible for backend side).
- If there is a network partition between Event Emitter and Event Publisher, then collectively, it is a reactive system. It is build using queuing technologies (possible for backend side).

Event may contain, reference or link back to source which may be useful to query back source. In UI, has field target to point back the source.

## Footnotes

1. A paradigm is a conceptual view of programming elements.
2. Document Object Model is the parsed tree object representation of HTML document.

---
title: Custom Events with RxJS
subtitle: Publisher-subscriber event model on top of Reactive Programming.
tags: rxjs, javascript
description: 'Building up Publisher-subscriber event model on top of RxJS.'
image: 'https://themightyprogrammer.dev/post/custom-event.jpg'
datePublished: '2020-01-29'
lastModified: '2020-01-29'
---

Event-Based Programming is natural to any GUI based interface. HTML DOM¹ offers inbuilt Event Notification model, but **_it is only useful when you are dealing with DOM Elements._** There is no support for non-DOM elements.

Web applications have grown up in complexity with time; also, Javascript started participating in backend applications; which in turn made the world to see Javascript as more than HTML DOM manipulation gig.

_This article showcases building up custom event publisher-subscriber model on top of Reactive Programming for non-DOM elements._

## Setting up Stage

### Event-Driven Programming

Event-Driven programming paradigm² introduces an intuitive flow of control based upon events.

> Button is clicked => show dialog.

#### Vocabulary

- One who emits event can be called Event Publisher or Event Emitter or Event Dispatcher.
- One who listens to events is called Event Subscriber or Event Listener or Event Handler.

In general, **Publisher-Subscriber** or **PubSub** term is used collectively for event publisher and even subscriber.

### Reactive Programming

Reactive Programming is a trending paradigm² these days. It is the processing of streams of data asynchronously. At core, reactive streams are written on top of the event-driven paradigm (?). You are going to build an event notification model on top of it by modelling events as a stream of data.

[RxJS](https://rxjs-dev.firebaseapp.com/) is a <b>r</b>eactive programming e<b>x</b>tension for <b>J</b>ava<b>s</b>cript.

---

## In Action

### Problem Statement

Consider modelling Thermostat, when room temperature crosses a certain threshold (30°C), AC would turn up, and windows would close.

### Design

Consider class Thermostat which encapsulates the logic of monitoring and publishes events:

- `above`: when temperature crossed above 30°C _(threshold)_ from lower value.
- `below`: when temperature crossed below or equal to 30°C _(threshold)_ from higher value.

### Data Structure for holding event structure

RxJS offers `Subject` which can be used to emit events to multiple subscribers.

```js
class Thermostat {
  constructor(sensor) {
    // data structure for holding subscriptions
    this.subscriptions = {
      above: new Subject(),
      below: new Subject()
    }
  }
}
```

Notice, there are two `Subject` objects. You can also create a single `Subject` object named `ThresholdCrossed` with direction `above` or `below`. _Separating the event or combining the event is Design Concern which may depend upon many factors._

### Subscription API for Event

`on` API for register action based upon event.

```js
on(event, action) {
  // sanatize event left code
  this.subscriptions[event].subscribe(v => action(v));
}
```

### Publishing Events

Consider `monitor` method which detects the rise and falls in temperature and publishes event accordingly.

```js
 monitor(currentTemperature) {
  // when temperature cross above THRESHOLD
  if (
    currentTemperature > THRESHOLD
    && this.lastRecordedTemperature <= THRESHOLD
  ) {

    // publish event
    this.subscriptions["above"].next(currentTemperature);

  } else if (
    currentTemperature <= THRESHOLD
    && this.lastRecordedTemperature >= THRESHOLD
  ) {

    // publish event
    this.subscriptions["below"].next(currentTemperature);
  }

  this.lastRecordedTemperature = currentTemperature;
 }
```

### Subscribing for event 

When the temperature is below 30°C then ac should go off, and the window should open; the opposite should happen for above event 30°C.

```js
thermostat.on('below', (t) => {
  this.ac = false
  this.window = true
})

thermostat.on('above', (t) => {
  this.ac = true
  this.window = false
})
```

Wiring all pieces together.

<iframe
     src="https://codesandbox.io/embed/custom-event-rxjs-ho0n5?fontsize=14&theme=light&view=preview"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="custom-event-rxjs"
     allow="geolocation; microphone; camera; midi; vr; accelerometer; gyroscope; payment; ambient-light-sensor; encrypted-media; usb"
     sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"
   ></iframe>

## Fiddling

Any variation you make with the above code itself a pattern:

- Keeping event subscription data in the mediator class of Event-Emitter and Event-Listener is in-memory Event Bus pattern.
- If the event holds enough information that Event-Listener doesn't look back to Event-Emitter; it is Event Carried State Transfer pattern
- If you store all events in datastore, it is Event Sourcing pattern _(possible for backend side)_.
- If there is a network partition between Event Publisher and Event Subscriber, then collectively, it is a Reactive System. It is build using queuing technologies _(possible for backend side)_.

An event may contain, reference or link back to its source. In HTML DOM, each event has a field named target to point back to its source.

## Footnotes

- <b>D</b>ocument <b>O</b>bject <b>M</b>odel is the parsed tree object representation of HTML document.
- A paradigm is a conceptual view of programming elements.

## More

- About Event-Driven  
  https://www.youtube.com/watch?v=STKCRSUsyP0&vl=en

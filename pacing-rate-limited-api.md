---
title: 'Design Problem: Pacing Rate Limited API'
subtitle: 
description: 'Learn about making a Pacer that paces the requests hitting metered API at the desired rate using Queues and timeout.'
keywords: pacing rate limited api, pacing, rate limited api
datePublished: ‘2022-08-30’
lastModified: ‘2024-05-01’
image: https://miro.medium.com/v2/resize:fit:1400/format:webp/1*JfSDFRJtl5LVPKh2hkKpsA.png
---

Today, most applications are API-driven. Whether creating a weather forecast, a financial ticker, a sports score alert, or translating a local language, you’ll need to connect 3rd party APIs to access the required data.

APIs are typically metered, and consumption is limited through Rate Limiting. A rate-limited API defines an API rate. It is usually expressed in terms of the number of requests made in a given timeframe; for example, the API rate of stock price quotes API is 100 per second, implying that you can only make 100 calls in a second.

A pacer mechanism must handle the gap between the application demand rate and API acceptance rate.

![Pacing Rate Limited Api](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*JfSDFRJtl5LVPKh2hkKpsA.png)

## Setting up the scenario

Assume we want to retrieve the weather forecast for all `19000` (approximately) postal codes (PINs) in India. A single request can only result in a single postal code’s prediction, and the maximum API call rate is `50 per second`.

OBVIOUSLY, We can’t hit `19000` requests at once to retrieve data. We need to build a mechanism for requesting at pace.

_This post will demonstrate creating a Pacer (in Typescript) that takes all requests for pin codes and hits API at the appropriate rate. Both the browser and node sides would be compatible with the code._

## Understanding the pacing mechanism

If we have to execute `19000 requests` (1 request in one call) in a single call, we keep the requests in memory and take 50 of them, execute them in the next second, take the next 50, and so on. _Essentially, It is throttling requests without rejecting them on the maximum limit._

## Implementation: Designing Interface

Pacing logic might well be contained within a class. It is best to keep API call logic and pacing logic separated. Pacing can then be applied in different contexts.

### Pacer Interface

`class Pacer` accepts requests and processes them at a given pace. A Request is a self-executable code for fulfilling it, and Pacer decides when to run that code.

```typescript
interface Request<T> {
	(): Promise<T>;
}

class Pacer<T> {
	private ratePerSecond: number;
	constructor(ratePerSecond: number) {
		this.ratePerSecond = ratePerSecond;
	}

	pace(request: Request<T>): Promise<T> {
		// implementation pending
		return request();
	}
}
```

### Using Pacer API

To use `Pacer`, we must create an instance of it with the required rate. After then, `pace` method can be used to add requests.

```typescript
let pacer = new Pacer<Axios.Response>(50);

let resultPromises = [];
for(let pinCode of pinCodes) {
  resultPromises.push(pacer.pace(() => $axios.get(`https://api.weather.org/weather/${pinCode}`));
}

let responses = await Promise.all(results);
```

## Implementation: Coding Logic

A functionally correct basic implementation of pace method can be:

```typescript
pace(request: Request<T>): Promise<T> {
   return request();
}
```

When using the above implementation, pace method would execute the request immediately after the reception, similar to code without pacing. We thus need to take this further.

### Micro-batching

The requests must be batched and executed once per second for appropriate implementation. This technique is referred to as micro-batching: _Smaller batch, Frequent Execution_. We need a Queue and an Executor to complete requests in a controlled manner.

### Queue

A request will be queued _as soon as it is received_. We also need to return a Promise of the request result. But we can't resolve the promise without executing the request.

```typescript
pace(request: Request<T>): Promise<T> {
  this.q.push(request);
  // what to return ??
}
```

### Proxy Promise
A _Proxy Promise_, which will capture the result of the request when executed, will be returned. We need to store its resolve and reject references along with the request.

```typescript
pace(request: Request<T>): Promise<T> {
  
  let requestResolve, requestReject;
  let result = new Promise<T>((resolve, reject) => {
    requestResolve = resolve;
    requestReject = reject; 
  })
  this.q.push({request, requestResolve, requestReject});

   // execution code.. implementation pending;
  return result;
}
```


### Execution
A solid design choice is keeping request execution in a separate function that executes multiple queued requests. Now, we need to work on triggering it once every second.


```typescript
private executeRequests() {
  let els = this.q.splice(0, Math.min(this.ratePerSecond, this.q.length));
  for (let el of els) {
    el.request()
        .then(el.requestResolve)
        .catch(el.requestReject)
  }
}
```


`setTimeout()` is a reasonable construct to use than setInterval(), as with setTimeout we can control the next execution.

#### Scheduling Logic
When a request is accepted, it may be processed immediately or after a few milliseconds of delay if no execution is scheduled.
On the execution side, after triggering requests, we check the queue size for the scheduling next execution with 1s delay.

![Scheduling Logic](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*CtCO1Rzs0vz_zzz1CZJOOQ.png)



```typescript
private scheduleRequests() {
  if (this.exectutorId == null) {
    // MIN_WAIT_TIME = 0 means immediate or MIN_WAIT_TIME = 50 after 50 millis seconds
    this.exectutorId = setTimeout(() => this.executeAndScheduleNext(), Pacer.MIN_WAIT_TIME);
  }
}

private executeAndScheduleNext() {
  this.executeRequests();

  // clear schedule
  clearTimeout(this.exectutorId);
  this.exectutorId = null;

  // next schedule
  if (this.q.length > 0) {
    this.exectutorId = setTimeout(() => this.executeAndScheduleNext(), 1000);
  }
}
```

The implementation works if requests are completed within one second; it is hardly the case in the real world. To make it more effective, we must track the number of requests being processed and accordingly adjust the pace.

The full code is accessible at:
[https://github.com/DM8tyProgrammer/api-pacer](https://github.com/DM8tyProgrammer/api-pacer)


## What’s next?
It is a practical implementation. Another option is to include a retry or failure mechanism. I have not yet considered implementing this feature; I would appreciate your feedback on improving it.
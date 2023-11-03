---
sidebar_position: 0
---

# Combination Operators

## startWith
Emits this value first, before the values emitted by the observable.

```ts
import { startWith } from "rxjs/operators";
```

```ts title="Code"
const numbers$ = of(1,2,3);

numbers$.pipe(
    startWith('a', 'b', 'c')
)
.subscribe(console.log)
```

```ts title="Output"
a
b
c
1
2
3
```

```html title="Practical Example"
<div>
    <h2 id="countdown"></h2>
    <div id="message"></div>
    <button id="abort">Abort Mission</button>
</div>
```
```ts title="Practical Example"
// element refs
const countdown = document.getElementById('countdown');
const message = document.getElementById('message');
const abortButton = document.getElementById('abort');

// streams
const counter$ = interval(1000);
const abortClick$ = fromEvent(abortButton, 'click');

const COUNTDOWN_FROM = 10;

counter$.pipe(
    mapTo(-1),
    scan((accumulator, current) => {
      return accumulator + current;
    }, COUNTDOWN_FROM),
    takeWhile(value => value >= 0),
    takeUntil(abortClick$),
    startWith(COUNTDOWN_FROM)
)
.subscribe(value => {
  countdown.innerHTML = value;
  if (!value) {
    message.innerHTML = 'Liftoff!'
  }
})
```

## endWith
Emits this value last, after the values emitted by the observable.

```ts
import { endWith } from "rxjs/operators";
```

```ts title="Code"
const numbers$ = of(1,2,3);

numbers$.pipe(
    endsWith('x', 'y', 'z')
)
.subscribe(console.log)
```

```ts title="Output"
1
2
3
x
y
z
```

## concat
Concat allows you to create an observable from multiple other observables.
On subscription, the concat will subscribe to the observables in order.

```ts
import {concat, interval} from "rxjs";
```

```ts title="Code"
const interval$ = interval(1000)

concat(
    interval$.pipe(take(3)),
    interval$.pipe(take(2))
).subscribe(console.log)
```

```ts title="Output"
0
1
2
0
1
```

## merge
Lets you subscribe to multiple observables and emits values as they occur.
Creates an observable by merging together other observables.
Merge should be used when the observables relate to each other and have a common solution.

```ts
import {merge} from "rxjs";
```

```ts title="Code"
const keyup$ = fromEvent(document, 'keyup');
const click$ = fromEvent(document, 'click');


merge(
    keyup$,
    click$
).subscribe(console.log)
```

```ts title="Practical Example"
// element refs
const countdown = document.getElementById('countdown');
const message = document.getElementById('message');
const pauseButton = document.getElementById('abort');
const startButton = document.getElementById('start');

// streams
const counter$ = interval(1000);
const pauseClick$ = fromEvent(pauseButton, 'click');
const startClick$ = fromEvent(startButton, 'click');

const COUNTDOWN_FROM = 10;

merge(
    startClick$.pipe(mapTo(true)),
    pauseClick$.pipe(mapTo(false))
).pipe(
    // switchmap to counter
    switchMap(shouldStart => shouldStart ? counter$ : of(null)),
    mapTo(-1),
    scan((accumulator, current) => {
      return accumulator + current;
    }, COUNTDOWN_FROM),
    takeWhile(value => value >= 0),
    startWith(COUNTDOWN_FROM)
)
.subscribe(value => {
  countdown.innerHTML = value;
  if (!value) {
    message.innerHTML = 'Liftoff!'
  }
})
```

## combineLatest
CombineLatest creates an observable from a number of other variables.
On subscription, it subscribes to all inner observables, after all observables
have emitted at least one value it will emit the last value emitted from each
observable as an array each time any of the observables emits a value.

```ts
import {combineLatest} from "rxjs";
```

```ts title="Code"
const keyup$ = fromEvent(document, 'keyup');
const click$ = fromEvent(document, 'click');


combineLatest(
    [keyup$,
      click$]
).subscribe(console.log)

```

```ts title="Practical Example"

```

## withLatestFrom

## forkJoin
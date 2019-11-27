# RxJS Recipes

[Dominic Elm & Kwinten Pisman - RxJS Recipes - Uphill Conf 2019](https://www.youtube.com/watch?v=W8T3eqUEOSI)

## Pattern #1

### Repeater

Work that needs to be executed n times (n > 1)

```
const result$ = trigger$.pipe(
  <flatteningOperator>(_ => work$)
);
```

## Pattern #2

### Enricher

Lazily enrich a stream with data when the trigger fires

```
const result$ = trigger$.pipe(
  withLatestFrom(enricherSource$),
  map(([trigger, data]) => <data>)
);
```

## Pattern #3

### Group Aggregator

Whenever the result is an `Observable<boolean>`

```
const FT$ = falseTrigger$.pipe(mapTo(false));
const TT$ = trueTrigger$.pipe(mapTo(true));

const result$ = merge(FT$, TT$);
```

## Pattern #4

### State Manager

State that needs to be reactively managed

```
const result$ = trigger$.pipe(
  scan((prevState, updates) => {
    return { ...prevState, updates };
  })
);
```

## Pattern #5

### Work Decider

Work that needs to be stopped and restarted when some trigger fires

```
const result$ = trigger$.pipe(
  <flatteningOperator>(condition => {
    if (condition) workA$;
    else workB$;
  })
);
```

## Pattern #6

### Error Decider

Work that needs to be stopped and restarted when some trigger fires

```
const result$ = trigger$.pipe(
  retryWhen(error$ => {
    return error$.pipe(switchMap(_ => {
      if (condition) workA$;
      else workB$;
    }))
  })
);
```

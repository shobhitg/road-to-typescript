# Deep Readonly

DeepReadonly is a usecase that can be used to maintain immutability of objects.

Typescript Example ([Playground](https://agentcooper.github.io/typescript-play/#code/JYOwLgpgTgZghgYwgAgCIQgBwEoTgEwHsQAbATwEEoo4yAeAFQD5kIAPSEfAZ2VwOLkqNeuiz8ipesxYBvAL4AoMGUwoAcsQBiAVxAIwwYgAUohNVBXq4AWwjdGLALzJZyANoBpZKGQBrCDJCGGQGAF0ALlCvMNYOCC5eXX1DYmQAfmQQCAA3aGQo73l3AKCQ8IBuZVUUMRw8SXIAeQAjACsIA0dkF1lFZAHkKAbBMg9jHxBkTRBkgyMQU3NoK1t7R0i0DHqBKUZ3YzCmKvkqlTUt8RG95h7+wYY4zh5kOBAyd1jMuolR4Vp9iAdDYWtAjgV7gNHuxnrxCO1OmAMpcdo0yK0Ol1bhFIaEqopQJBYIgUABlSCYVz3EgJADmYAAFlEgSDoCdFATwNB4EhQlBgJS+gMbIR8BAogByBjYCjqUkASQYEuQAB9kBKAMIUbASqoDbgU7hRclYT7s6oXBj8zC8FxWgVmjnnFDymw2HRgOAtGn2m09FG-G7W7jHDk0pHAN0er0+4NRV3uz3eiC+20eIXIEViyXS2UKpUAGnuBqwRo8uIzg2QNJA9KZyAAjAAGXFKAZhRTyDsEqNJ2MC7juJthAB0WZQLk12t1AwA9LPkABRaiEKBRXLQMiM0C0ny8Ya7cg9xMxlPBoejks2i8jmt1-0AJgAzBVkPOlyu16w8lAtwyd3uQzXOQQA))

```typescript
interface DeepReadonlyArray<T> extends ReadonlyArray<DeepReadonly<T>> {}
type NonFunctionPropertyNames<T> = { [K in keyof T]: T[K] extends Function ? never : K }[keyof T];
type DeepReadonlyObject<T> = {
    readonly [P in NonFunctionPropertyNames<T>]: DeepReadonly<T[P]>;
};
type DeepReadonly<T> =
    T extends any[] ? DeepReadonlyArray<T[number]> :
    T extends object ? DeepReadonlyObject<T> :
    T;

interface Step {
  length: number;
}

interface Trip {
  mode: 'TRANSIT' | 'CAR';
  steps: Step[];
}

type Trips = Trip[];

type ImmutableTrips = DeepReadonly<Trips>;

let immutableTrips: ImmutableTrips = [{
  mode: 'TRANSIT',
  steps: [
    {
      length: 10
    }
  ]
}]

immutableTrips[0].mode = 'CAR';  // Error: everything is readonly
immutableTrips[0].steps[0].length = 23; // Error: everything is readonly
```
# $ObjMap&lt;T, F&gt;

Flow's [`$ObjMap<T, F>`](https://flow.org/en/docs/types/utilities/#toc-objmap) takes an object type `T` and a function type `F` and returns the type of the object that results from applying `F` to each key of the object.

Replicating `$ObjMap<O, F>` directly in TypeScript is not yet possible. There is an open issue ([#1213](https://github.com/Microsoft/TypeScript/issues/1213)) in the TypeScript repo which, once resolved, will make it possible to directly implement `$ObjMap<O, F>`.

Until higher kinded types are implemented in TypeScript, we have to write the resulting type from `$ObjMap` directly for each case.

Flow example ([playground](https://flow.org/try/#0PTAEAEDMBsHsHcBQiAuBPADgU1AUQB4oBOAhgMYoBKWKArkQHYAqmOAvKADwBqAfABT8AlKDa9Q3EWInJItBhQCWsBqCLzOAeQBcoAN4BtANZY0ugM7FFDAOYBdXQDF5SlQF8BsXZqG6AJJoARgBWALIkGFoANHiEpBTUdIws2OJ6iKBqNPSqQcFYFAB0Jmjm-LBChURYACa0ZFiC5GQxRlLieQUohSTm5oo2DPzNMXqgxg6gsBPCoG5Co-MA3IhuyGQqllOi+hmgJLqz0sS0WFF7gYftoADkkLCwN6sriPzqQxU9uoEP0FgkDCES1AIFAmiMr3e5Uql1AliI1hsQJBYHBkPk0MKsJ+sD+AORoIAcrBsDFAqBFOZ9nCrLZEFDPmRgUSSWdQGRQPBeqAGLAUBTVCgABY4WAImzWEjQKYhLpAA))

```javascript
// @flow

type ExtractReturnType = <V>(() => V) => V

function run<O: {[key: string]: Function}>(o: O): $ObjMap<O, ExtractReturnType> {
  return Object.keys(o).reduce((acc, k) => Object.assign(acc, { [k]: o[k]() }), {});
}

const o = {
  a: () => true,
  b: () => 'foo'
};

(run(o).a: boolean); // Ok
(run(o).b: string); // Ok
(run(o).b: boolean); // Nope, b is a string
run(o).c; // Nope, c was not in the original object
```

TypeScript example ([playground](https://agentcooper.github.io/typescript-play/#code/GYVwdgxgLglg9mABAJ3AHgPKIKYA8rZgAmAzogN4DaA1tgJ4BciJUyMYA5gLpMAUAlIgC8APkQBDMHUQBfEbzhMM-JuUSUA0onaJadOMEQYeiAErYoIZGAAqdAA7ZMmrmJkUAUIhQWrSDABGAFbY0AB0eiQK-GHI2EQgENi8vOIQEAA0uoKiRsGhUGHiJCQwHGCp6VlqNCZwtQKy-NXuxRJS-ADcHjIeHhAILIhwwp7e4nw5Yqwg2BleiAGTwmIA5MBwcKs93f2DUBJMAZsANtiSo6gVcDHie2BDS8ys7ByX4NFhAfdDwOIwJyOp3OSCEKA+Ny+PwOfwBACZ3tcYhAgA))

```typescript
function run<O extends {[key: string]: () => any }>(o: O): { [K in keyof O]: ReturnType<O[K]> } {
  return Object.keys(o).reduce((acc, k) => Object.assign(acc, { [k]: o[k]() }), {} as any);
}

const o = {
  a: () => true,
  b: () => 'foo'
};

const a: boolean = run(o).a
const b: string = run(o).b
const fail: boolean = run(o).b
const fail2 = run(o).c
```

Flow example 2 ([playground](https://flow.org/try/#0PTAEAEDMBsHsHcBQiAmBTAxtAhgJzaJAK4B2GALgJawmgAOusdAzgDwCCANKAPIBcoAN6gA2gGs0ATwHNyuSiQDmAXQHtQAXwB8ACgawAtpWZpmAngEoBABUZGTrACQ8ARgCsAstjqse3cpJ0aLCQoI7Y8NiU5FpaANzIGDSy9HbGpqAAvEKg2DZpJgB0+Myw0ABuaDoALABMFpoJ+ix6BaYWheQAFmgkOrBZWkKIoKD9hXmgdRZxoCC8YiNjsBMC1QDMM3NgAKK4jLjcMAigYiQIzKDRAOSXdYgaM0A))

```javascript
// @flow

declare function props<A, O: { [key: string]: A }>(promises: O): Promise<$ObjMap<O, typeof $await>>;

const promises = { a: Promise.resolve(42) };
props(promises).then(o => {
  (o.a: 42); // Ok
  (o.a: 43); // Error, flow knows it's 42
});
```

TypeScript example 2 ([playground](https://agentcooper.github.io/typescript-play/#code/C4TwDgpgBACgTgewLYEsDOEBKE0FcA2wAPACoB8UAvFCVBAB7AQB2AJmrIqhgDIoDWEIimYAzCHCiYKAfilQAXDQBQy1hADG+AIZxoo3Mw3AUCZlDCIwaIgHk6jFuygBvKAG1BIJWmBwRAOYAukrazCBQAL5kABSWyOg4SrYAlErwCRhELspQHgDSUCJQXgiiULYhnJlYOATEtu75QWTK0QDcqhpmvhZciRzUbtrp-RgAdHpoCPgAbhAxACwATClRnfHWcWM4KePAABYsMQhUFDl53cy9I1DMuEgARhJUUAjj2u1QAPTfNOAQADKGn8YGARTEEg4AAN7k8JNCADR3BDg6EraFQUQISSHaCgSDjXJQK69UTaFD4Hx+QKvd6fNopdpAA))

```typescript
type PromiseResult<T> = T extends PromiseLike<infer R> ? R : T

declare function props<O extends { [key: string]: any }>(promises: O): Promise<{
  [K in keyof O]: PromiseResult<O[K]>
}>;

const promises = { a: Promise.resolve(42) };
props(promises).then(o => {
  const a: number = o.a; // TypeScript infers `number`, not `42` for the type.
  const fail: string = o.a;
});
```

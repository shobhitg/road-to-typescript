# $Diff&lt;A, B&gt;

## Description

Flow documentation: [$Diff&lt;A, B&gt;](https://flow.org/en/docs/types/utilities/#toc-diff)

As the name hints, `$Diff<A, B>` is the type representing the set difference of `A` and `B`, i.e. `A \ B`, where `A` and `B` are both object types.

Flow example ([playground](https://flow.org/try/#0PTAEAEDMBsHsHcBQAXAngBwKagAoCdZ0BnUAXlAG9QA7AQwFtMAuUI5PAS2oHMAaUWt2Y0ArvQBGmPKAC+AbhQZsAEUyRaI6MnyES5KoOHUxk6fMVZQAJUwBHERzyYAJjuJlQAEmUdIkADxuRPyq6praBMQAfAqIkCLUAMbIHLDUrJgRugAU6JFELDb2ji5BAJSUiKCgIKAAdA2IMoiIRJlB2VR0jCwA5JCwsL2yZQptWcSdNAzC-YO9-IYsACwATPzitABeLOrQbSNyNWCosCKgibTp6LREJJgAHuy0oHm6oMiDre35U0uga0Ox1AUgIeH43WwHBITmKTmcQA))

```javascript
// @flow
type Props = { name: string, age: number };
type DefaultProps = { age: number };
type RequiredProps = $Diff<Props, DefaultProps>;

function setProps(props: RequiredProps) {
  // ...
}

setProps({ name: 'foo' });
setProps({ name: 'foo', age: 42, baz: false }); // you can pass extra props too
setProps({ age: 42 }); // error, name is required
```

TypeScript equivalent of `$diff<A, B>` ([playground](https://agentcooper.github.io/typescript-play/#code/C4TwDgpgBACgTgezAZygXigbygOwIYC2EAXFMsHAJY4DmANFHjSbgK4EBGEcUAvgNwAoUJCgARCADM8rADbB4SVBmxMWOdlzgMIADwp5SHBAlkQ8OPkMEB6G1ABKEAI6tKcCABNFKMgAsEOU8oLixcQhZyKlo+QVt7AFkIYADggEZBPTAEOGAoEWgE6lZkACYAHgAVBgBVAD50WEoAYwBrKoYxSklJctaIEARJKGqofsHh+rqhAscXNw9vRBQ0xqKNMvKfZE6pGXlt6bi7KCSUhGDS4XBoLp6OqHrGyqg9YAgcT1QaqAB+XAgADduFBSJUhFkcnlZgBlVgccoAQQYACEGioANoAOSg1HE3V64yGUGRYwGxLRAF1SIjsZTeNdRE5XO4vNtSo04QjtrtpHIFMtkEdBEA))

```typescript
type Props = { name: string, age: number };
type DefaultProps = { age: number, extra: boolean };

// RequiredProps should be { name: string }

// Method 1
export type Minus2<T, U> = Pick<T, Diff<keyof T, keyof U>>;
type RequiredProps1 = Minus2<Props, DefaultProps>;

// Method 2
type Diff<T, U> = T extends U ? never : T;
export type Sub<A, B> = {[N in Diff<keyof A, keyof B>]: A[N]}
type RequiredProps2 = Sub<Props, DefaultProps>;
```

In Flow, `$Diff<A, B>` will error if the object you are removing properties from does not have the property being removed, i.e. if `B` has a key that doesn’t exist in `A`:

Flow error ([playground](https://flow.org/try/#0PTAEAEDMBsHsHcBQAXAngBwKagAoCdZ0BnUAXlAG9QA7AQwFtMAuUI5PAS2oHMAaUWt2Y0ArvQBGmPKAC+AbhQZsAEUyRaI6MnyES5KoOHUxkvP1jIAFlJZtOPWXNAhQAdQ7RooKQWkATEWxkWFArDhIAAwtrPAjQdAIsPDQaC1BJLm5QLlxEogA6RSxQACVMAEcRDjxMPx1iMlAAEmUOSEgAHnqiflV1TW08gD4FREgRagBjZA5YalZMQd0ACgTdFjLK6trugEpKRFB4vPy6RgUjl3zrxBkgA))

```javascript
// @flow
type Props = { name: string, age: number };
type DefaultProps = { age: number, other: string }; // Will error due to this `other` property not being in Props.
type RequiredProps = $Diff<Props, DefaultProps>;

function setProps(props: RequiredProps) {
  props.name;
  // ...
}
```

Flow has this issue, this doesn't happen in typescript.

But if Typescript had to create an exact equivalent that gives errors just like Flow's `$Diff<A, B>`, then it would be as follows:

* TODO



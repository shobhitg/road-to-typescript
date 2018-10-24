# $Shape&lt;T&gt;

[`$Shape<T>`](https://flow.org/en/docs/types/utilities/#toc-shape) copies the shape of the supplied type but marks all properties as optional.

TypeScript's equivalent to `$Shape<T>` is [`Partial<T>`](https://www.typescriptlang.org/docs/handbook/advanced-types.html#mapped-types)

Flow example ([playground](https://flow.org/try/#0PTAEAEDMBsHsHcBQAXAngBwKagAqYE4DOsAdqALygDeiooAhgOaYBcoJArgLYBGBANLXb0urUIWT4AliUaCAvigzY8RUgBFMyelOiEKoACQBlABb0sAHlXESAPgDciRAGNSE0FjUkAjGxukBlRMYgBMABzyDnQgoACi+Piw+GxcUoSEMoygAAYkIpg5ru7IngS2of7lgZRU+aJsAOT0jVExYAlJKaBpGVm5IUVuJB5etgDMVd6a2rr6tSFsEW2gsQDyANLFI6VjpAAsU7YzOnpB9WLNrdGrYJvbo9UkAKxHGlqn89SLoBH8wg1QFcVusNkA))

```javascript
// @flow
type Person = {
  age: number,
  name: string,
}
type PersonDetails = $Shape<Person>;

const person1: Person = {age: 28};  // Error: missing `name`
const person2: Person = {name: 'a'};  // Error: missing `age`
const person3: PersonDetails = {age: 28};  // OK
const person4: PersonDetails = {name: 'a'};  // OK
const person5: PersonDetails = {age: 28, name: 'a'};  // OK
```

TypeScript example ([playground](https://agentcooper.github.io/typescript-play/#code/C4TwDgpgBAChBOBnA9gOygXigbwFBSgEMBzCALilQFcBbAIwQBp9LCbypFh4BLVY5gF9coSLAQpUAEQjBCPADaJMsQvGA9CCgDxwkaAHwBuXLgDGaLlEj7UARgp7JK7CQ4AmAByCjBAPR+UACi8PDI8BQ0PIiIfMRQAAaobBAJ5pbA1hJo7o7Z6FjYyewUAOSEpT7+gSFhEVBRMXGJbmkWqFY2kgDMebYycorKhW4UXlVQAVAA8gDS6R2ZXWgALH2SA-JKLsUc5ZW+k4FzC535AKzraJtDLqNQXoysJVD7E1NzQA))

```typescript
type Person = {
  age: number,
  name: string,
}
type PersonDetails = Partial<Person>;

const person1: Person = {age: 28};  // Error: missing `name`
const person2: Person = {name: 'a'};  // Error: missing `age`
const person3: PersonDetails = {age: 28};  // OK
const person4: PersonDetails = {name: 'a'};  // OK
const person5: PersonDetails = {age: 28, name: 'a'};  // OK
```

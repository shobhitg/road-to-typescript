# $Values&lt;T&gt;

## Description

[$Values&lt;T&gt;](https://flow.org/en/docs/types/utilities/#toc-values) represents the union type of all the value types of the enumerable properties in an Object Type`T`.

Flow example \([playground](https://flow.org/try/#0PTAEAEDMBsHsHcBQAXAngBwKagAoCdZ0BnUAXlAG9FRQA7AQwFtMAuUI5PAS1oHMAaaqHq9WdAK6MARpjyCAvgG5EiEKAAqAC2yRY0OPB69QyeLBMZMJenmyYAjuK4A3etEy1kLFJdwF0AGpu4lZk7JxGoAA+EtKyymhYfoQAJEHQISTkacFWADz4hEQAfMqIAMawtBx0TGKF6DkZoeQA5ABSVa2KNGoA8gDSFVU1IvX+TZlhACwATD2g-UMgldXIoJC0bA2TLaAAFACUZMWUSr1gAKJ4BHgAhBvitOXIXFWgXCS0sOvoNuuwSAmbSgJ5vWgWLBAA)\):

```javascript
// @flow
type Props = {
  name: string,
  age: number,
};

// The following two types are equivalent:
type PropValues = string | number;
type Prop$Values = $Values<Props>;

const name: Prop$Values = 'Jon';  // OK
const age: Prop$Values = 42;  // OK
//const fn: Prop$Values = () => {};  // Error! function is not part of the union type
```

Corresponding Typescript example \([playground](https://www.typescriptlang.org/play/index.html#src=type%20Values%3CT%3E%20%3D%20T%5Bkeyof%20T%5D%20%2F%2F%20%24Values%3CT%3E%0D%0A%0D%0Atype%20Props%20%3D%20%7B%0D%0A%20%20name%3A%20string%2C%0D%0A%20%20age%3A%20number%2C%0D%0A%7D%3B%0D%0A%0D%0A%2F%2F%20The%20following%20two%20types%20are%20equivalent%3A%0D%0Atype%20PropValues%20%3D%20string%20%7C%20number%3B%0D%0Atype%20Prop%24Values%20%3D%20Values%3CProps%3E%3B%0D%0A%0D%0Aconst%20name1%3A%20Prop%24Values%20%3D%20'Jon'%3B%20%20%2F%2F%20OK%2C%20if%20in%20a%20module%20context%0D%0Aconst%20age%3A%20Prop%24Values%20%3D%2042%3B%20%20%2F%2F%20OK%0D%0Aconst%20fn%3A%20Prop%24Values%20%3D%20%28%29%20%3D%3E%20%7B%7D%3B%20%2F%2F%20Error)\)

```typescript
type Values<T> = T[keyof T] // $Values<T>

type Props = {
  name: string,
  age: number,
};

// The following two types are equivalent:
type PropValues = string | number;
type Prop$Values = Values<Props>;

const name1: Prop$Values = 'Jon';  // OK
const age: Prop$Values = 42;  // OK
const fn: Prop$Values = () => {}; // Error
```


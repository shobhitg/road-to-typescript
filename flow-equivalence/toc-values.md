# $Values&lt;T&gt;

## Description

[$Values&lt;T&gt;](https://flow.org/en/docs/types/utilities/#toc-values) represents the union type of all the value types of the enumerable properties in an Object Type `T`.

Flow example ([playground](https://flow.org/try/#0PTAEAEDMBsHsHcBQAXAngBwKagAoCdZ0BnUAXlAG9FRQA7AQwFtMAuUI5PAS1oHMAaaqHq9WdAK6MARpjyCAvgG5EiEKAAqAC2yRY0OPB69QyeLBMZMJenmyYAjuK4A3etEy1kLFJdwF0AGpu4lZk7JxGoAA+EtKyymhYfoQAJEHQISTkacFWADz4hEQAfMqIAMawtBx0TGKF6DkZoeQA5ABSVa2KNGoA8gDSFVU1IvX+TZlhACwATD2g-UMgldXIoJC0bA2TLaAAFACUZMWUSr1gAKJ4BHgAhBvitOXIXFWgXCS0sOvoNuuwSAmbSgJ5vWgWLBAA)):

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

Corresponding Typescript example ([playground](https://agentcooper.github.io/typescript-play/#code/C4TwDgpgBAaghgGwK4QM4B4AqA+KBeKTAbQGsIQB7AM0IF0oB6BqAEnmTS2wChvRIoABQBOFMKnxQA3tyhQAdnAC2EAFxRUwYQEt5AcwA0sqHD1qFSJQCMIwowF8A3LyaEAFtCoUECCgHddPShgPwpg8DQTYWgIAEckbQA3RAh5YFU+CKFRMHYUCQJNHX0oAB8La1tnfmgRMTZEfMk8zjrxbGduAGMKeU0FZQgARnU2ho4CqAByACleqcc5VwB5AGlu3v7TczGWyYAWACZFxmY1jb7gKCp5UZzxpoIACgBKfFwpJ1OoAFFhUWEQA))

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


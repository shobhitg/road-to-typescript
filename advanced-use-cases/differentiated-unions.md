# Differentiated Unions

TypeScript's differentiated unions are equivalent to Flow's [disjoint unions](https://flow.org/en/docs/types/unions/#toc-disjoint-unions). They can be used to create types which better represent REST API responses.

Consider a situation where an API will return a JSON object with the `success` property set to `true` and some data if our request was successful or `success` set to `false` if the request failed and a helpful `message` explaining why the request failed.

This *could* be typed like the following:

```typescript
interface APIResponse {
  success: boolean,
  data?: string[],
  message?: string
}
```

However, with this interface, we will run into problems where we know more about the type than TypeScript, as seen in the following example. ([playground](https://agentcooper.github.io/typescript-play/#code/JYOwLgpgTgZghgYwgAgIIAUCSAlCBnABwHsQ8UBvAKGWTwFcEk88AuZAIyKIBsI4QANNWQATOGDgB+NnjBRQAcwDaAXSE0AtvjxwFEabTmLKAX0qUREBNzhQUCErOR3CjiGww58xUhHPAYZAAKFx8yADp6Rm0ASmQqGgdSMFFxOBkjEGUVZABeZ283cLEJZAB6MuQAUSgoIigBZAc6bhEOFDoQSxhQCBFTZAhuMnjhJKctZl13Q3ksvILXX3DJnT1yypq6hqaiFrb2Dq6IHpA+0yA))

```typescript
interface APIResponse {
  success: boolean,
  data?: string[],
  message?: string
}

declare const response: APIResponse

if (response.success) {
  const data: string[] = response.data // Error, could be undefined
} else {
  const message: string = response.message // Error, could be undefined
}
```

We can use differentiated unions to resolve both errors. ([playground](https://agentcooper.github.io/typescript-play/#code/JYOwLgpgTgZghgYwgAgKJSgeygJQgZwAdMR8UBvAKGWXwFcEl98AuZeAGzIBprkBbAvjgBzCG3xgooEZQC+lSqEixEKAPIBrPERJlkVGvUZC2UuhF40AJnDBwJUmQG0AuvMVgAnoRQBBAAUASR1iUhQAXjQMbFC9FAAfZC048MVrCAQOOCgUBD0wZFzdcLZAkIIwskVgGGQACmKqiAA6YyZ8AEoDPnzSQtt7R2kQETdkKKb4lsG4ZAB6eeQAOUxkaCwoeXWuCl6CgSFRcVonUYmiyunBZmOFpdX1mK25IA))

```typescript
interface ErrorResponse {
  success: false,
  message: string
}

interface OkResponse {
  success: true,
  data: string[]
}

type APIResponse = ErrorResponse | OkResponse

declare const response: APIResponse

if (response.success) {
  const data: string[] = response.data // No error
} else {
  const message: string = response.message // No error
}
```

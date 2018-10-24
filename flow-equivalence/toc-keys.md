# $Keys&lt;T&gt;

## Description

In Flow, [$Keys&lt;T&gt;](https://flow.org/en/docs/types/utilities/#toc-keys) is often used as a substitute for a missing enum functionality in javascript.

{% code-tabs %}
{% code-tabs-item title="Flow" %}
```javascript
// @flow
const countries = {
  US: "United States",
  IT: "Italy",
  FR: "France"
};

type Country = $Keys<typeof countries>;

const italy: Country = 'IT';
const nope: Country = 'nope'; // 'nope' is not a Country
```
{% endcode-tabs-item %}

{% code-tabs-item title="TypeScript keyof" %}
```typescript
const countries = {
  US: "United States",
  IT: "Italy",
  FR: "France"
}

type Country = keyof typeof countries

const italy: Country = 'IT';
const nope: Country = 'nope'; // 'nope' is not a Country
```
{% endcode-tabs-item %}

{% code-tabs-item title="TypeScript enum" %}
```typescript
enum Countries {
  US = "United States",
  IT = "Italy",
  FR = "France"
}

type Country = keyof typeof Countries

const italy: Country = 'IT';
const nope: Country = 'nope'; // 'nope' is not a Country
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Typescript provides [enums](https://basarat.gitbooks.io/typescript/content/docs/enums.html) as a first class entity, and provides the `keyof` operator for use cases which don't exactly match enumerations.


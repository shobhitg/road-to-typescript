# $Keys&lt;T&gt;

## Description

In Flow, [$Keys&lt;T&gt;](https://flow.org/en/docs/types/utilities/#toc-keys) is used to as a substitute for a missing enum functionality in javascript.

{% code-tabs %}
{% code-tabs-item title="Flow code" %}
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
{% endcode-tabs %}

Typescript instead provides [enums](https://basarat.gitbooks.io/typescript/content/docs/enums.html) as a first class entity.




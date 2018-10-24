# Assert if a reducer case is missing by accident

Typescript example ([playground](https://agentcooper.github.io/typescript-play/#code/GYVwdgxgLglg9mABAQwM6oKYCcoDkMBu2AFHAEYBWAXImIdgJQ11FaIDeAUIolABZY4Ad1oYRAUSyCsxAESTpshgG5OAX06cYYKNmDIIGRAGUAjiGRYjXHgGttAExqzU5yxlmqeqGAC8MzCAAtmTYqhraulj6hogAShjQyGAA5gA21tyI9mBOiLJWSakZnllCMA78gSFhWXwYMCl8UNWhWOFaOnoGRgDCMFgQGRxZOXmyEANDHl6IWMgOMCCorbUanFAAngAORsZ8yLuIALwmblaIAD7xiVDJ6UbX-YMZqpygkLAIKFbICQ4gQwyFYmA67BgjbzlKAQPiIYioAB0YwhNh4iAgaCMLnOHioWXRcwwUBAWCQSJ8-kQACpEBS-BhZjxMZh8oU7sU8QT0VYSWS6Yj6o1mjSBeVKnwmYgAPTSjFY-KTF5cwmywlEvlIACyyH4iIACgBJUVI+aLZY02kAJilDgw+hAaRa3J4aEwOHwrARKiyGh4vNJ5I6QA))
```typesript
function assertNever(obj: never): never {
  throw new Error("Error");
}

interface Square {
  kind: "square";
  size: number;
}
interface Rectangle {
  kind: "rectangle";
  width: number;
  height: number;
}
interface Circle {
  kind: "circle";
  radius: number;
}

type Shape = Square | Rectangle | Circle;

function areaReducer(s: Shape) {
  switch (s.kind) {
    case "square":
      return s.size * s.size;
    case "rectangle":
      return s.height * s.width;
    // case "circle":
    //     return Math.PI * s.radius ** 2;
    default:
      assertNever(s);
  }
  return s;
}
```
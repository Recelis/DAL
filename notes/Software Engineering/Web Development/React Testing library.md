# React Testing Library

## Within

[docs](https://testing-library.com/docs/dom-testing-library/api-within/)

If you already have a DOM Element and you want to query only within that DOM element you can use the `within` function to do this.

```javascript
import { render, within } from "@testing-library/react";

const { getByText } = render(<MyComponent />);
const messages = getByText("messages");
const helloMessage = within(messages).getByText("hello");
```

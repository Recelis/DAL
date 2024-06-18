---
title: "Storybook"
date: 
---

# Stories
A story captures a rendered state of a UI component. Stories are typically:
components/Button/Button.stories.tsx

A story typically looks like:
```typescript
import type { Meta } from '@storybook/react';

import { Button } from './Button';

const meta: Meta<typeof Button> = {
    title: 'Path/To/MyComponent',
    component: Button,
};

export default meta;
```

The title is analyzed from Storybook 7.0. It is recommended to use **Component Story Format** when laying out stories.

## Args

### Story args

### Component args

### Global args

# Decorators

# Component Story Format
[docs](https://storybook.js.org/docs/api/csf)
This is an open standard based on ES6 modules and can be reused across different frameworks.

## Default export
There is a default export in the component that defines the metadata of your component.

```typescript
// Replace your-framework with the name of your framework
import type { Meta } from '@storybook/your-framework';

import { MyComponent } from './MyComponent';

const meta: Meta<typeof MyComponent> = {
  /* 👇 The title prop is optional.
   * See https://storybook.js.org/docs/configure/#configure-story-loading
   * to learn how to generate automatic titles
   */
  title: 'Path/To/MyComponent',
  component: MyComponent,
  decorators: [ ... ],
  parameters: { ... },
};

export default meta;
```

## Named story exports
Every named export in file is a story object.
```typescript
import type { Meta, StoryObj } from '@storybook/react';

import { MyComponent } from './MyComponent';

const meta: Meta<typeof MyComponent> = {
  component: MyComponent,
};

export default meta;
type Story = StoryObj<typeof MyComponent>;

export const Basic: Story = {};

export const WithProp: Story = {
  render: () => <MyComponent prop="value" />,
};
```

Each story name should start with a capital letter and are converted using Lodash's **startCase** function.

```typescript
some_custom_NAME	Some Custom NAME
```
By default, the story name is the variable name i.e. `export const WithProp: Story = xxx` but you can set the field manually in the Story.

```typescript
export const Simple: Story = {
  decorators: [],
  name: 'So simple!',
  parameters: {},
};
```

## Args story inputs
A story can take in args that are passed into the component. This avoids hardcoding the component.
```typescript
export const Text = {
  args: {
    label: 'Hello',
    onClick: action('clicked'),
  },
  render: ({ label, onClick }) => <Button label={label} onClick={onClick} />,
};
```

## Play function
You can add interactive tests to your stories with Storybook play functions.

```typescript
/*
 * See https://storybook.js.org/docs/writing-stories/play-function#working-with-the-canvas
 * to learn more about using the canvasElement to query the DOM
 */
export const FilledForm: Story = {
  play: async ({ canvasElement }) => {
    const canvas = within(canvasElement);

    // 👇 Simulate interactions with the component
    await userEvent.type(canvas.getByTestId('email'), 'email@provider.com');

    await userEvent.type(canvas.getByTestId('password'), 'a-random-password');

    // See https://storybook.js.org/docs/essentials/actions#automatically-matching-args to learn how to setup logging in the Actions panel
    await userEvent.click(canvas.getByRole('button'));

    // 👇 Assert DOM structure
    await expect(
      canvas.getByText(
        'Everything is perfect. Your account is ready and we should probably get you started!',
      ),
    ).toBeInTheDocument();
  },
};
```

## Custom render functions
You can render your component with differently using a custom render function. This can wrap your component up.
```typescript
export const Example: Story = {
  render: () => (
    <Layout>
      <header>
        <h1>Example</h1>
      </header>
      <article>
        <MyComponent />
      </article>
    </Layout>
  ),
};
```

## Non-story exports
If you have non-story exports in your Story file, Storybook will treat them as stories unless you use the `includeStories` or `excludeStories` property in the `meta` default export.
```typescript
const meta: Meta<typeof MyComponent> = {
  component: MyComponent,
  includeStories: ['SimpleStory', 'ComplexStory'], // 👈 Storybook loads these stories
  excludeStories: /.*Data$/, // 👈 Storybook ignores anything that contains Data
};

export const simpleData = { foo: 1, bar: 'baz' };
export const complexData = { foo: 1, foobar: { bar: 'baz', baz: someData } };
```

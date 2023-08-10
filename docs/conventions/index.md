---
layout: default
title: Coding Conventions
nav_order: 3
has_children: false
---

## TABLE OF CONTENTS
{: .no_toc .text-delta }

1. COMPONENT STYLING
{:toc}

----

### COMPONENT STYLING

All component styling should done using [styled-components].

#### ONE THEME ABOVE ALL

Utilizing the theme for the majority of component styling is the preferred approach.

NON-COMPLIANT

```tsx
const StyledButton = styled.button`
  color: #333333;
  font-size: 1rem;
  font-weight: 400;
  margin-left: 4px;
  border-radius: 50px;
`;
```

COMPLIANT

```tsx
const StyledButton = styled.button`
  color: ${({ theme }) => theme.font.color.primary};
  font-size: ${({ theme }) => theme.font.size.md};
  font-weight: ${({ theme }) => theme.font.weight.regular};
  margin-left: ${({ theme }) => theme.spacing(1)};
  border-radius:  ${({ theme }) => theme.border.rounded};
`;
```

#### UNITS OF MEASUREMENT

Avoid using `px` or `rem` values directly within the styled components. The necessary values are generally already defined in the theme, so it's recommended to make use of the theme for these purposes.

Example:

```tsx
const StyledButton = styled.button`
  padding: ${({ theme }) => theme.spacing(2)};
`;
```

#### COLORS

Refrain from introducing additional colors; instead, utilize the existing palette from the theme. Should there be a situation where the palette does not align, kindly leave a comment so that it can be rectified.

Example:

```tsx
const StyledContainer = styled.div`
  color: ${({ theme }) => theme.font.color.secondary};
`;
```

#### FONT-SIZE

Please use theme for font-size, font-weight

```tsx
const StyledContainer = styled.div`
  font-size: ${({ theme }) => theme.font.size.md};
  font-weight: ${({ theme }) => theme.font.weight.regular};
`;
```

### STYLED COMPONENTS

All styled components should have names starting with `Styled` and should be placed in the same file as the component they are styling.

Example:

```tsx
import styled from 'styled-components';

const StyledButton = styled.button`
  background-color: red;
`;

const Button = () => <StyledButton>Click me!</StyledButton>;
```

### NO INTERFACES

The project prioritizes the usage of `type` definitions over `interfaces`.

NON-COMPLIANT

```ts
interface User {
  name: string;
  age: number;
}
```

COMPLIANT

```ts
type User = {
  name: string;
  age: number;
};
```

### THE `useEffect` HOOK

Limit the utilization of this hook to reduce the frequency of component re-renders. Employ it only when necessary and accompany its usage with a comment clarifying the rationale behind its need.

Furthermore, adhere to a single useEffect hook within each component to maintain code clarity and organization.


### NO `FC`

As per project convention, please refrain from using `FC`. Instead, opt for defining functions with the format `FunctionName`.

Secondly, avoid utilizing arrow function syntax for defining components. Please adhere to the format of exporting components using the `export function FunctionName() {}` syntax.

NON-COMPLIANT

```tsx
const EmailField: React.FC<{
  value: string;
}> = ({ value }) => <TextInput value={value} disabled fullWidth />;
```

COMPLIANT

```tsx
type OwnProps = {
  value: string;
};

function EmailField({ value }: OwnProps) {
  return <TextInput value={value} disabled fullWidth />;
}
```

### LOGS (`console.log`)

This should be straightforward: refrain from using `console.log` statements, please.

### VARIABLE NAMES

Variable names ought to precisely depict the purpose or function of the variable.

NON-COMPLIANT

```tsx
const [value, setValue] = useState('');
```

COMPLIANT

```tsx
const [email, setEmail] = useState('');
```

#### SOME WORDS TO AVOID

- dummy

### OPTIONAL PROPS

Avoid supplying the default value for an optional prop, as it generally doesn't contribute significantly.

EXAMPLE
Assume, we have the EmailField component defined below

```tsx
type OwnProps = {
  value: string;
  disabled?: boolean;
};

function EmailField({ value, disabled = false }: OwnProps) {
  return <TextInput value={value} disabled fullWidth />;
}
```

NON-COMPLIANT USAGE

```tsx
function Form() {
  return <EmailField value="username@email.com" disabled={false} />;
}
```

COMPLIANT USAGE

```tsx
function Form() {
  return <EmailField value="username@email.com" />;
}
```

### PROP DRILLING

Minimize the practice of excessively passing down state variables and their setters.

### HOOK DEPENDENCIES

Avoid utilizing a dependency solely to trigger a re-run of a `useEffect` if that dependency is not actually employed within the `useEffect` logic.

Generally, it's recommended to refrain from using `useEffect` as a substitute for imperative calls that could be executed at specific locations where the changes are initiated.

### EVENT HANDLERS

Event handler names should start with `handle` instead of `on`

NON-COMPLIANT

```tsx
const onEmailChange = (val: string) => {
  // ...
}
```

COMPLIANT

```tsx
const handleEmailChange = (val: string) => {
  // ...
}
```

### IMPORTS

When importing, opt for the designated aliases rather than specifying complete or relative paths.

```
{
  alias: {
    "~": path.resolve(__dirname, "src"),
    "@": path.resolve(__dirname, "src/modules"),
    "@testing": path.resolve(__dirname, "src/testing"),
  },
}
```

NON-COMPLIANT

```tsx
import { CatalogDecorator } from '../../../../../testing/decorators/CatalogDecorator';
import { ComponentDecorator } from '../../../../../testing/decorators/ComponentDecorator';
```

COMPLIANT

```tsx
import { CatalogDecorator } from '~/testing/decorators/CatalogDecorator';
import { ComponentDecorator } from '~/testing/decorators/ComponentDecorator';
```

### FIXED CONSTANT SETS

For situations where a value anticipates a fixed set of constants, it's advisable to define enums.

NON COMPLIANT

```ts
let color: "red" | "green" | "blue" = "red";
```

COMPLIANT

```ts
enum Color {
  Red = "red",
  Green = "green",
  Blue = "blue",
}
let color = Color.Red;
```

### BREAKING CHANGES

Prioritize thorough manual testing before proceeding to guarantee that modifications haven't caused disruptions elsewhere, given that tests have not yet been integrated.

----

[styled-components]: https://emotion.sh/docs/styled

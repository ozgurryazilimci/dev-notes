# shadcn/ui

## Introduction

**shadcn/ui** is an open-source, accessible, and fully customizable component library for React.  
It provides a headless design system that works with your Tailwind setup. ([ui.shadcn.com](https://ui.shadcn.com/docs))

---

## Installation

### 1. Initialize your project

```bash
npx shadcn@latest init
```

This sets up the necessary configuration for shadcn/ui in your project.

### 2. Install dependencies with npm

```bash
npm install
```

Make sure `react`, `react-dom`, and `tailwindcss` are installed as well.

---

## Adding Components

To add a core component:

```bash
npx shadcn@latest add button
npx shadcn@latest add input
npx shadcn@latest add spinner
npx shadcn@latest add label
```

After adding, import them in your components:

```tsx
import {Button} from "@/components/ui/button";
import {Input} from "@/components/ui/input";
import {Spinner} from "@/components/ui/spinner";
import {Label} from "@/components/ui/label";
```

---

## Using External Components

You can also add components from external registries:

```bash
npx shadcn@latest add https://nativeui.io/registry/button
```

- Downloads the external component into your local `components/ui` folder.
- You can now use it like any internal shadcn/ui component:

```tsx
import {Button} from "@/components/ui/external/button";

export function ExternalButtonDemo() {
    return <Button>External Button</Button>;
}
```

---

## Component Examples

### Button

```tsx
<Button>Click me</Button>
<Button variant="secondary">Secondary</Button>
<Button disabled>Disabled</Button>
```

### Input

```tsx
<Input type="text" placeholder="Type here..."/>
<Input type="email" placeholder="Email"/>
```

### Spinner

```tsx
<Spinner className="w-6 h-6 text-blue-500"/>
```

### Label

```tsx
<Label htmlFor="email">Email</Label>
<Input id="email" placeholder="Enter your email"/>
```

### Combining Form Components

```tsx
<form>
    <Label htmlFor="name">Name</Label>
    <Input id="name" placeholder="Your name"/>

    <Label htmlFor="email">Email</Label>
    <Input id="email" type="email" placeholder="Your email"/>

    <Button type="submit">Submit</Button>
</form>
```

### Spinner & Kbd

**Spinner:** Loading indicator.

```tsx
import {Spinner} from "@/components/ui/spinner";

<Spinner/>
```

**Kbd:** Display keyboard keys.

```tsx
import {Kbd, KbdGroup} from "@/components/ui/kbd";

<Kbd>Ctrl</Kbd>
<KbdGroup>
    <Kbd>Ctrl</Kbd>
    <Kbd>B</Kbd>
</KbdGroup>
```

### Button Group

Groups related buttons, split buttons, or prefixes/suffixes.

```tsx
import {ButtonGroup} from "@/components/ui/button-group";

<ButtonGroup>
    <Button>Button 1</Button>
    <Button>Button 2</Button>
</ButtonGroup>
```

Nested example:

```tsx
<ButtonGroup>
    <ButtonGroup>
        <Button>Button 1</Button>
        <Button>Button 2</Button>
    </ButtonGroup>
    <ButtonGroup>
        <Button>Button 3</Button>
        <Button>Button 4</Button>
    </ButtonGroup>
</ButtonGroup>
```

With text and input:

```tsx
<ButtonGroup>
    <ButtonGroupText>Prefix</ButtonGroupText>
    <Input placeholder="Type here..."/>
    <Button>Button</Button>
</ButtonGroup>
```

### Input Group

Combine inputs with icons, buttons, labels.

```tsx
import {
    InputGroup,
    InputGroupAddon,
    InputGroupInput,
} from "@/components/ui/input-group";

<InputGroup>
    <InputGroupInput placeholder="Search..."/>
    <InputGroupAddon>
        <SearchIcon/>
    </InputGroupAddon>
</InputGroup>
```

### Field

Unified component for complex forms.

```tsx
import {
    Field,
    FieldDescription,
    FieldError,
    FieldLabel,
} from "@/components/ui/field";

<Field>
    <FieldLabel htmlFor="username">Username</FieldLabel>
    <Input id="username" placeholder="Max Leiter"/>
    <FieldDescription>Choose a unique username for your account.</FieldDescription>
</Field>
```

Supports all form controls (input, textarea, select, checkbox, radio, switch, slider).

Grouping fields:

```tsx
<FieldSet>
    <FieldLegend/>
    <FieldGroup>
        <Field/>
        <Field/>
    </FieldGroup>
</FieldSet>
```

### Item

Flexible container for lists, cards, avatars, etc.

```tsx
import {
    Item,
    ItemContent,
    ItemDescription,
    ItemMedia,
    ItemTitle,
} from "@/components/ui/item";

<Item>
    <ItemMedia variant="icon">
        <HomeIcon/>
    </ItemMedia>
    <ItemContent>
        <ItemTitle>Dashboard</ItemTitle>
        <ItemDescription>Overview of your account and activity.</ItemDescription>
    </ItemContent>
</Item>
```

`ItemGroup` can wrap multiple items; `asChild` prop allows links.

### Empty

For empty states.

```tsx
import {
    Empty,
    EmptyContent,
    EmptyDescription,
    EmptyMedia,
    EmptyTitle,
} from "@/components/ui/empty";

<Empty>
    <EmptyMedia variant="icon">
        <InboxIcon/>
    </EmptyMedia>
    <EmptyTitle>No messages</EmptyTitle>
    <EmptyDescription>You don't have any messages yet.</EmptyDescription>
    <EmptyContent>
        <Button>Send a message</Button>
    </EmptyContent>
</Empty>
```

Works with avatars and input groups as well.

---

## Theming

### CSS Variables

Enable CSS variables for theming:

```json
{
  "tailwind": {
    "cssVariables": true
  }
}
```

### Utility Classes

Disable CSS variables and use utility classes instead:

```json
{
  "tailwind": {
    "cssVariables": false
  }
}
```

---

## CLI Commands

- `init` → Setup project for shadcn/ui
- `add <component>` → Add a component
- `view` → View available components

---

## AI Elements

[**AI Elements**](https://github.com/vercel/ai-elements) by Vercel provides React components for building AI-powered
UIs, such as chat and dynamic content components.  
It is a complementary tool: while shadcn/ui provides general UI components and styling, AI Elements adds AI-specific
interactive components that can integrate alongside shadcn/ui components in a React project.

**Example: Chat Component**

```tsx
import {Chat} from "@vercel/ai-elements/react";

export function AIChatDemo() {
    return (
        <div className="max-w-md mx-auto">
            <h2 className="text-lg font-bold mb-4">AI Chat Demo</h2>
            <Chat
                apiKey={process.env.NEXT_PUBLIC_AI_API_KEY}
                systemMessage="You are a helpful assistant."
                placeholder="Type a message..."
            />
        </div>
    );
}
```

This shows how AI Elements can provide interactive AI-powered UI, which can be combined with shadcn/ui components like
`Button` or `Spinner` for a complete user experience.

---

## FAQ

**Q: Can I install components via npm?**  
A: Yes, you can install the core shadcn/ui package with `npm install`. The CLI handles adding components.

**Q: Can I add external components?**  
A: Yes, using a URL or registry, e.g.:

```bash
npx shadcn@latest add https://nativeui.io/registry/button
```

**Q: How do I customize colors or sizes?**  
A: Use Tailwind classes or enable `cssVariables` in `components.json`.

---

### References

- [shadcn on X](https://x.com/shadcn)
- [shadcn/ui Changelog](https://ui.shadcn.com/docs/changelog)
- [shadcn/ui Docs](https://ui.shadcn.com/docs)
- [Vercel AI Elements GitHub](https://github.com/vercel/ai-elements)
- [AI SDK Elements Overview](https://ai-sdk.dev/elements/overview)

For more examples and up-to-date docs, visit [shadcn/ui official site](https://ui.shadcn.com/docs).

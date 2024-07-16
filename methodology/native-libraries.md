## Leveraging Platform-Specific Code and Native Libraries in shared codebase

Building complex applications in React Native often requires taking advantage of platform-specific features and native libraries. React Native provides several tools to help developers achieve this, including Platform.select, Platform.OS, and Metro's file extensions for platform-specific code. Additionally, employing design patterns like the Strategy Pattern can help manage platform-specific implementations elegantly. Let's dive into these techniques and how they can be applied.

consider the case of applying themes to the app,
- In Next.js we use the [next-themes](https://github.com/pacocoursey/next-themes) library.
- In Expo we use the `useColorScheme` hook from React Native.

We make use of the strategy pattern in building UI components that make use of platform specific code:

## [Stategy pattern](https://refactoring.guru/design-patterns/strategy)
<i>
The Strategy pattern suggests that you take a class that does something specific in a lot of different ways and extract all of these algorithms into separate classes called strategies.

The original class, called context, must have a field for storing a reference to one of the strategies. The context delegates the work to a linked strategy object instead of executing it on its own.

The context isn’t responsible for selecting an appropriate algorithm for the job. Instead, the client passes the desired strategy to the context. In fact, the context doesn’t know much about strategies. It works with all strategies through the same generic interface, which only exposes a single method for triggering the algorithm encapsulated within the selected strategy.

This way the context becomes independent of concrete strategies, so you can add new algorithms or modify existing ones without changing the code of the context or other strategies.
</i>

In my own example:
Consider you want to make two instances of a `Duck` class, the `quacking Duck` and the `flying Duck`. We say `quacking` and `flying` are behaviours of the duck, and these behaviours can be coupled in their own functions (strategies) and passed to the `Duck` class

Lets go back to the case of building the `ToggleTheme` component.
```tsx
// packages/components/toggle-theme/base

export default function ToggleThemeBase ({toggleTheme}: {toggleTheme: () => void}) {
    return <Button onPress={toggleTheme}>
        <Text>Toggle Theme</Text>
    </Button>
}
```
Then, we can make use of Metro's file extensions for bundling platform specific code:
```tsx
// packages/components/toggle-theme.tsx

import ToggleThemeBase from "./base"

export default function ToggleTheme () {
    const { toggleTheme } = useColorScheme()
    return <ToggleThemeBase toggleTheme={toggleTheme}>
}
```
```tsx
// packages/components/toggle-theme.web.tsx

import ToggleThemeBase from "./base"

export default function ToggleTheme () {
    const { toggleTheme } = useContext(ThemeContext)
    return <ToggleThemeBase toggleTheme={toggleTheme}>
}
```

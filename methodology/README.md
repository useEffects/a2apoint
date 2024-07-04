# Building universal apps with react

In the ever-evolving world of app development, the goal of creating universal applications that function seamlessly across web and mobile platforms is highly coveted. Since React Native was open-sourced, many software developers and companies have leveraged it to utilize the core React functionalities [they originally created for their websites](https://discord.com/blog/how-discord-achieves-native-ios-performance-with-react-native). This was possible because of the fact that **react is a library** and not a framework.

The React Native team advocates for a single codebase solution for Android and iOS. However, what about web development? A glance at the documentation reveals scant information on using React Native for platforms such as web, Windows, Linux, or macOS, unlike its counterpart, Flutter, which supports compilation to [all major platforms](https://flutter.dev/multi-platform). Support for web was brought in by the veteran ex-twitter software developer [Nicolas Gallagher](https://nicolasgallagher.com/). Today react-native-web has involved to a compatibility layer between React DOM and React Native. You will be surprised twitter uses few elements from [react native for their web](https://blog.x.com/engineering/en_us/topics/infrastructure/2019/buildingfasterwithcomponents) but a native implementation for each of their android and ios apps.

## State of building universal applications today
- React Native for Web is currently used in production Web apps by companies including [Meta](https://www.meta.com/), [X](https://x.com/), and [Flipkart](https://x.com/naqvitalha/status/969577892991549440). Software engineers from Meta, Expo, and elsewhere continue to contribute design and patches to the react-native-web project.
- Expo has official adaptors to port [react native codebase to Next.js](https://docs.expo.dev/guides/using-nextjs/)
- [Solito](https://solito.dev) is the missing piece for using React Native with Next.js to build powerful cross-platform apps.

<hr/>

Recently, I've formed a team with [Pavithra VS](https://pavithra.tech) to build an exciting project: [A2A Point](https://a2apoint.com). You can find the full project report [here](). In this post, I will share our team's design choices in building the web app and mobile apps from a single codebase, adhering as much as possible to the DIY principle. Importantly, we've ensured that this approach does not compromise the unique benefits offered by each tech stack, preserving their respective advantages.

This blog would cover three key aspects in this project.
- [UI](./ui.md)
- [Data fetching](./data-fetching.md)
- [Targeting native libraries](./native.md)
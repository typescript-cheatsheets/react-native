# react-native-typescript-cheatsheet

## Getting Started

The best way to start is with Expo:

```bash
npm install -g expo-cli
expo init AwesomeProject
# you can pick from the typescript templates in the Managed or Bare workflows. If in doubt, use Managed
```

This should install the correct TypeScript dev dependencies to get you started:

```js
  "devDependencies": {
    "@types/react": "~16.9.0",
    "@types/react-native": "~0.60.23",
    "@babel/core": "^7.0.0",
    "babel-preset-expo": "~8.0.0",
    "typescript": "~3.6.3"
  },
```


## Contributors

This project aims to accumulate TypeScript advice for React Native users, in the same nature as https://github.com/typescript-cheatsheets/react-typescript-cheatsheet/. This is a new project and [**we are actively seeking maintainers**](https://github.com/typescript-cheatsheets/react-native-typescript-cheatsheet/issues/1).

The goal is to **open source knowledge**:

- Write what you wish you had known
- What you will want to refer to in future
- Identify what you don't know and actively ask for community to fill in the gaps
- Be corrected on your misconceptions

An example list of sections:

- Setting up a RN + TS project from scratch
- Recommended Build tools + Prettifying/Linting setup
- Most commonly used RN APIs with their TS types
- Tips and tricks with RN Core's typings
- Tips and tricks with RN Community typings

# react-native-typescript-cheatsheet

This project aims to accumulate TypeScript advice for React Native users, in the same nature as https://github.com/typescript-cheatsheets/react-typescript-cheatsheet/. It serves as a community maintained supplement to [the official RN + TS docs](https://facebook.github.io/react-native/docs/typescript).

This is a new project and [**we are actively seeking maintainers**](https://github.com/typescript-cheatsheets/react-native-typescript-cheatsheet/issues/1).

## Getting Started

The best way to start is with Expo:

```bash
npm install -g expo-cli
# or
yarn global add expo-cli

expo init AwesomeProject
# you can pick from the typescript templates in the Managed or Bare workflows.
# If in doubt, use Managed
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

## TypeScriptified Tutorial

This translates [the RN docs](https://facebook.github.io/react-native/docs/getting-started) into TypeScript:

---

### Props

https://facebook.github.io/react-native/docs/props

```ts
import React, { Component } from "react";
import { Text, View } from "react-native";

class Greeting extends Component<{ name: string }> {
  render() {
    return (
      <View style={{ alignItems: "center" }}>
        <Text>Hello {this.props.name}!</Text>
      </View>
    );
  }
}

// // Function Component version
// function Greeting(props: {name: string}) {
//   return (
//     <View style={{ alignItems: 'center' }}>
//       <Text>Hello {props.name}!</Text>
//     </View>
//   );
// }

export default class LotsOfGreetings extends Component {
  render() {
    return (
      <View style={{ alignItems: "center", top: 50 }}>
        <Greeting name="Rexxar" />
        <Greeting name="Jaina" />
        <Greeting name="Valeera" />
      </View>
    );
  }
}
```

---

### State

https://facebook.github.io/react-native/docs/state

```ts
import React, { Component } from "react";
import { Text, View } from "react-native";

type BlinkProps = {
  text: string;
};
type BlinkState = {
  isShowingText: boolean;
};
class Blink extends Component<BlinkProps, BlinkState> {
  componentDidMount() {
    // Toggle the state every second
    setInterval(
      () =>
        this.setState(previousState => ({
          isShowingText: !previousState.isShowingText
        })),
      1000
    );
  }

  //state object
  state = { isShowingText: true };

  render() {
    if (!this.state.isShowingText) {
      return null;
    }

    return <Text>{this.props.text}</Text>;
  }
}

// // hooks equivalent
// function Blink(props: BlinkProps) {
//   const [isShowingText, setIsShowingText] = React.useState(true) // state's type is inferred to be boolean
//   // with other types it is helpful to explicitly specify the state's type
//   // const [isShowingText, setIsShowingText] = React.useState<BlinkState>({ isShowingText: true})
//   React.useEffect(() => {
//     let interval = setInterval(() => (
//       setIsShowingText(previousState => !previousState)
//     ), 1000);
//     return () => clearInterval(interval) // class component forgot to cleanup the interval
//   })
//   if (isShowingText) return null
//   return (
//     <Text>{props.text}</Text>
//   );
// }

export default class BlinkApp extends Component {
  render() {
    return (
      <View>
        <Blink text="I love to blink" />
        <Blink text="Yes blinking is so great" />
        <Blink text="Why did they ever take this out of HTML" />
        <Blink text="Look at me look at me look at me" />
      </View>
    );
  }
}
```

---

### Style, Height and Width, Flexbox

https://facebook.github.io/react-native/docs/style

https://facebook.github.io/react-native/docs/height-and-width

https://facebook.github.io/react-native/docs/flexbox

Nothing TS Specific.

---

### Handling Text Input

https://facebook.github.io/react-native/docs/handling-text-input

```ts
import React, { Component } from "react";
import { Text, TextInput, View } from "react-native";

export default class PizzaTranslator extends Component<{}, { text: string }> {
  constructor(props) {
    super(props);
    this.state = { text: "" };
  }

  render() {
    return (
      <View style={{ padding: 10 }}>
        <TextInput
          style={{ height: 40 }}
          placeholder="Type here to translate!"
          onChangeText={text => this.setState({ text })}
          value={this.state.text}
        />
        <Text style={{ padding: 10, fontSize: 42 }}>
          {this.state.text
            .split(" ")
            .map(word => word && "🍕")
            .join(" ")}
        </Text>
      </View>
    );
  }
}
```

---

### Handling Touches

https://facebook.github.io/react-native/docs/handling-touches

---

### Using a ScrollView

https://facebook.github.io/react-native/docs/using-a-scrollview

---

### Using List views

https://facebook.github.io/react-native/docs/using-a-listview

---

### Networking

https://facebook.github.io/react-native/docs/network

```ts
import React from "react";
import { FlatList, ActivityIndicator, Text, View } from "react-native";

type DataItem = { title: string; releaseYear: string; id: string };
type State = {
  isLoading: boolean;
  dataSource?: DataItem[];
};
export default class FetchExample extends React.Component<{}, State> {
  constructor(props) {
    super(props);
    this.state = { isLoading: true };
  }

  componentDidMount() {
    return fetch("https://facebook.github.io/react-native/movies.json")
      .then(response => response.json())
      .then((responseJson: { movies: any }) => {
        this.setState(
          {
            isLoading: false,
            dataSource: responseJson.movies
          },
          function() {}
        );
      })
      .catch(error => {
        console.error(error);
      });
  }

  render() {
    if (this.state.isLoading) {
      return (
        <View style={{ flex: 1, padding: 20 }}>
          <ActivityIndicator />
        </View>
      );
    }

    return (
      <View style={{ flex: 1, paddingTop: 20 }}>
        <FlatList
          data={this.state.dataSource}
          renderItem={({ item }) => (
            <Text>
              {item.title}, {item.releaseYear}
            </Text>
          )}
          keyExtractor={({ id }, index) => id}
        />
      </View>
    );
  }
}
```
#### Networking in Functional Components
```ts
import React, { useState, useEffect } from "react";
import { FlatList, ActivityIndicator, Text, View } from "react-native";

type DataItem = { title: string; releaseYear: string; id: string };

type State = {
  isLoading: boolean;
  dataSource?: DataItem[];
};

const FetchExample = ({ }: State) => {
  const [isLoading, setIsLoading] = useState(true)
  const [dataSource, setDataSource] = useState()


  useEffect(() => {
    fetch("https://facebook.github.io/react-native/movies.json")
      .then(response => response.json())
      .then((responseJson: { movies: any }) => {
        setIsLoading(false)
        setDataSource(responseJson.movies)
      })
      .catch(error => {
        console.error(error);
      });

  }, [])


  if (isLoading)
    return (
      <View style={{ flex: 1, padding: 20 }}>
        <ActivityIndicator />
      </View>
    );


  return (
    <View style={{ backgroundColor: 'red' }}>
      <FlatList
        data={dataSource}
        renderItem={({ item }) => (
          <Text>
            {item.title}, {item.releaseYear}
          </Text>
        )}
        keyExtractor={({ id }, index) => id}
      />
    </View>
  );

}
export default FetchExample
```

## Contributing

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

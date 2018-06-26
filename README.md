# React Native Styling

This tutorial shall mostly take away the fear of styling a React Native App. React Native will translate the JavaScript 
camelCase style properties of your components into platform specific native styles/layouts (iOS and Android).

Thoughts:

- You will mostly use Flexbox (`flex`) to do the layout
- Use `Platform` Component to do platform specific Styling
- Use `Dimensions` Component to do Responsive Styling
- Also think of using/buying an Out-of-the-Box Template like React Native Sketch Elements: https://react-native.shop/elements

*Notes:*

- This is NOT an in-depth tutorial, just an overview.
- We do NOT talk about Global Styling (with Custom Components), Animations, and Image Handling.
- BUT it should give a good first overview of the main topics of React Native Styling :-)

## Table of Contents

1. [Getting Started](#chapter1)
2. [Styling Basics](#chapter2)
    1. [Which components can be styled?](#chapter2a)
    2. [StyleSheet.create() vs. {}](#chapter2b)
    3. [Layout with Flexbox `flex`](#chapter2c)
    4. [Styling with Units & Relative Units](#chapter2d)
    5. [Platform specific Styling with `Platform` Component](#chapter2e)
    6. [Responsive Styling with `Dimensions` Component](#chapter2f)
    7. [Dynamic Responsive Styling with `Dimensions` Component's Event Listener `change`](#chapter2g)
3. [Links](#chapter3)


## <a id="chapter1"></a>1. Getting Started

*Notes:*

- This is just a README.md to show code examples for your own React Native Styling Project.

## <a id="chapter2"></a>2. Let's dive into the Basics

## <a id="chapter2a"></a>i. Which components can be styled?

You can only add styles to these 4 components (or components that are composed from these 4 components):

- Image
- ScrollView
- Text
- View

Example styling:

1. First let's import StyleSheet component:

```
import { StyleSheet } from "react-native";
```

2. Then create a styles object

    ```
    const styles = StyleSheet.create({
        container: {
            flex: 1,
            backgroundColor: "blue"
        }
    });
    ```

3. Finally attach it to style property of your component:

    ```
    <View style={styles.container}>
        {props.children}
    </View>
    ```


For more information:

- https://github.com/vhpoet/react-native-styling-cheat-sheet


## <a id="chapter2b"></a>ii. StyleSheet.create() vs. {}

We quickly compare this ...

```
const styles = StyleSheet.create({
    container: {
        flex: 1,
        backgroundColor: "white"
    }
});
```

... against using a "normal" JavaScript object:

```
const styles = {
    container: {
        flex: 1,
        backgroundColor: "white"
    }
};
```
 
Use StyleSheet.create() whenever possible because it

- validates the styles
- sends the styles to native code more efficiently


## <a id="chapter2c"></a>iii. Layout with Flexbox `flex`

The three basic properties of Flexbox are:

```
const styles = StyleSheet.create({
    container: {
        flex: 1,
        justifyContent: "center",
        alignItems: "center"
    }
});
```

This will give the container a maximum possible size on the screen and centers its elements vertically and horizontally.

More detailed:

- `flex: 1`: Take ALL available space or more precisely: `flex: 1` is component's priority to get a piece of the available space 
on the screen. E.g. if you have 2 sibling containers with `flex: 1`, both containers get 50% of the available space.

- `justifyContent: "center"`: `justifyContent` means, by default, vertical positioning, i.e. positioning in main direction, 
which is column by default: `flexDirection: "column"`, so from top to bottom = vertically.
So `justifyContent: "center"` tells React Native to center the elements in the container vertically.

- `alignItems: "center"`: `alignItems` means, by default, horizontal positioning, i.e. positioning on cross-axis 
to main direction above (cross-axis means 90 degrees from the axis (main direction) of justifyContent above).
So `alignItems: "center"` tells React Native to center the elements in the container horizontally.

For more information:

- https://github.com/vhpoet/react-native-styling-cheat-sheet


## <a id="chapter2d"></a>iv. Styling with Units & Relative Units

To give a component a specific size (e.g. width = 200px), do the following:

```
const styles = StyleSheet.create({
    container: {
        flex: 1,
        justifyContent: "center",
        alignItems: "center"
    },
    input: {
        width: 200
    }
});
```

Attach it easily:

```
<View style={styles.container}>
    <TextInput placeholder"Your Username" style={styles.input} />
</View>
```

To be more flexible if you want to re-use the component in other parts of the app, you might want to give it a Relative Unit (e.g. width = 75%):

```
const styles = StyleSheet.create({
    container: {
        flex: 1,
        justifyContent: "center",
        alignItems: "center"
    },
    input: {
        width: "75%"
    }
});
```


## <a id="chapter2e"></a>v. Platform specific Styling with `Platform` Component

First let's import it:

```
import { Platform } from "react-native";
```

Check if user has an Android device ...

```
if (Platform.OS === "android") {

    // do styling for Android

}
```

or an iOS device:

```
if (Platform.OS === "ios") {

    // do styling for iOS

}
```

## <a id="chapter2f"></a>vi. Responsive Styling with `Dimensions` Component

1. First let's import Dimensions component:

```
import { Dimensions } from "react-native";
```

2. Then, in order to, say, change a responsive container's Flexbox's main direction 
from vertical `flexDirection:: "column"` to horizontal `flexDirection:: "row"` 
when user rotates his/her device from portrait to landscape, do this:

```
const styles = StyleSheet.create({
    responsiveContainer: {
        flex: 1,
        flexDirection: Dimensions.get("window").height > 500 ? "column" : "row"
    },
    input: {
        width: "75%"
    }
});
```


## <a id="chapter2g"></a>vii. Dynamic Responsive Styling with `Dimensions` Component's Event Listener `change`

The key is to use `Dimensions.addEventListener("change", dims => {})` function to listen to dimension changes (rotation of the device).

It is placed inside the constructor of the component. When the rotation event occurs, the `this.setState()` is called 
causing a re-rendering of the component with the adjusted style properties for the current device's orientation:


```
class OurResponsiveComponent extends Component {
  state = {
    respStyles: {
      pwContainerDirection: "column",
      pwContainerJustifyContent: "flex-start",
      pwWrapperWidth: "100%"
    }
  };

  constructor(props) {
    super(props);
    Dimensions.addEventListener("change", dims => {
      this.setState({
        respStyles: {
          pwContainerDirection:
            Dimensions.get("window").height > 500 ? "column" : "row",
          pwContainerJustifyContent:
            Dimensions.get("window").height > 500
              ? "flex-start"
              : "space-between",
          pwWrapperWidth: Dimensions.get("window").height > 500 ? "100%" : "45%"
        }
      });
    });
  }
  
  render() {
      return (
      ...
```

## <a id="chapter3"></a>3. Links

### Have a look !

React Native Styling:

- Maximilian Schwarzmüller, Udemy Course: "React Native - The Practical Guide": https://www.udemy.com/react-native-the-practical-guide/
- Cheat Sheet: https://github.com/vhpoet/react-native-styling-cheat-sheet

### Credits to the authors of above links ! Thank you very much !

### And credits to the reader: Thanks for your visit !
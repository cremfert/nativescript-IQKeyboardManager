# NativeScript IQKeyboardManager Plugin

NativeScript wrapper for the popular [IQKeyboardManager](https://cocoapods.org/pods/IQKeyboardManager) iOS framework, which provides an elegant solution for preventing the iOS keyboard from covering `UITextView` controls.

![Example of using the IQKeyBoardManager NativeScript plugin on an iOS device](screenshot.gif)

## Installation

```
$ tns plugin add nativescript-iqkeyboardmanager
```

## Usage

That's it! IQKeyboardManager takes care of all initialization when your app starts up by default.

## Advanced usage

### Grouping related textfields (previous / next buttons)
If your UI layout has sibling text fields, then IQKeyboardManager is able to automatically
add previous / next buttons to the accessorybar which the user can use to jump back and forth.
See those < and > buttons in the video above.

In case those fields were not direct siblings, until version 1.3.0 of this plugin, you had no way
to force the previous / next buttons to appear. However, now you can:

#### NativeScript /w XML usage
Note that for example sake superfluous `<StackLayout>`s were added:

```xml
<Page xmlns="http://schemas.nativescript.org/tns.xsd" xmlns:IQKbMgr="nativescript-iqkeyboardmanager">
  <StackLayout>
    <IQKbMgr:PreviousNextView><!-- add this 'wrapper' to enable those previous / next buttons -->
      <StackLayout>
        <StackLayout>
          <TextField hint="Email"/>
        </StackLayout>
        <StackLayout>
          <TextField hint="Password"/>
        </StackLayout>
      </StackLayout>
    </IQKbMgr:PreviousNextView>
  </Stacklayout>
</Page>
```

#### NativeScript /w Angular usage
In the `.modules.ts` file where you want to use this feature (or the `app.module.ts`),
register the `PreviousNextView` element:

```typescript
import { registerElement } from "nativescript-angular";
registerElement("PreviousNextView", () => require("nativescript-iqkeyboardmanager"). PreviousNextView);
```

Then in the view, use that element like this (again, we went nuts with the `<StackLayout>`s:

```html
<StackLayout>
  <PreviousNextView><!-- add this 'wrapper' to enable those previous / next buttons -->
    <StackLayout>
      <StackLayout>
        <TextField hint="Email"></TextField>
      </StackLayout>
      <StackLayout>
        <TextField hint="Password"></TextField>
      </StackLayout>
    </StackLayout>
  </PreviousNextView>
</Stacklayout>
```

#### NativeScript /w Vue usage
Vue usage is very similar to Angular usage, the only difference is in how the element is registered. Open your app's entry file, and add this:

```javascript
Vue.registerElement("PreviousNextView", () => require("nativescript-iqkeyboardmanager"). PreviousNextView)
```

### Tweaking the appearance and behavior

Start by adding the following two paths into your app’s `references.d.ts` file. (See this repo’s demo app for a specific example.)

```
/// <reference path="./node_modules/tns-platform-declarations/ios/ios.d.ts" />
/// <reference path="./node_modules/nativescript-iqkeyboardmanager/index.d.ts" />
```

> **NOTE**: You might also need to `npm install --save-dev tns-platform-declarations` to bring in NativeScript’s TypeScript definitions for native iOS development.

Next, initialize an instance of `IQKeyboardManager` with the following line of code.

```typescript
const iqKeyboard = IQKeyboardManager.sharedManager();
```

You now have the full IQKeyboardManager APIs available for you to use. For example you could use the following code to switch to a dark keyboard.

```typescript
const iqKeyboard = IQKeyboardManager.sharedManager();
iqKeyboard.overrideKeyboardAppearance = true;
iqKeyboard.keyboardAppearance = UIKeyboardAppearance.Dark;
```

For more examples of what's possible, run the demo app (shown in the gif below) and check out the [app's `main-view-model.ts` file](demo/app/main-view-model.ts).

<img src="https://github.com/tjvantoll/nativescript-IQKeyboardManager/raw/master/demo.gif" width="320px"/> 

## Documentation

For more details on how IQKeyboardManager works, including more detailed API documentation, refer to [the library's CocoaPod page](https://cocoapods.org/pods/IQKeyboardManager).

## Maintainers

For maintainer’s of this plugin’s source code: when the [IQKeyboardManager Podfile](platforms/ios/Podfile) updates, you should generate new typings for for this plugin to reflect those changes.

To do so, execute these commands.

```bash
cd demo
TNS_DEBUG_METADATA_PATH="$(pwd)/metadata" tns build ios
TNS_TYPESCRIPT_DECLARATIONS_PATH="$(pwd)/typings" tns build ios
```

Next, locate IQKeyboardManager’s generated typings file in the `demo/typings` folder and override the `IQKeyboardManager.d.ts` file in this repo’s root.

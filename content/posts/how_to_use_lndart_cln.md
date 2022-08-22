---
title: "Develop custom plugins for Core Lightning"
author: "swaptr"
description: "This blog post elaborates on how to use lndart.cln to develop your own custom plugins for Core Lightning in the Dart programming environment."
date: "2022-08-21"
tags: [lightning-network]
---
![lndart](https://raw.githubusercontent.com/swaptr/site/master/static/images/logo.png "lndart.cln")

Hello again, today we will look into developing custom plugins for Core Lightning using the Dart programming language. Core Lightning ships with a solid and high-performant core written in the C programming language, and extended functionality is provided through plugins. [Some](https://github.com/ElementsProject/lightning/tree/master/plugins) plugins critical to the functioning of Core Lightning are shipped along, whereas there are many [community curated](https://github.com/lightningd/plugins) plugins available as well.

So let's get into it and start developing our next great idea.

[lndart.cln](https://github.com/dart-lightning/lndart.cln) houses a package [cln_plugin](https://github.com/dart-lightning/lndart.cln/tree/main/packages/cln_plugin) which is used to develop custom plugins. A [template](https://github.com/dart-lightning/lndart.cln_plugin) that implements the library is available at you disposal for easy setup of your project. All efforts are made to keep the template up to date, but in case we are lagging behind, you can continue to keep using the latest library by keeping your `pubspec.yaml` up to date.
```yaml
dependencies:
  cln_plugin_api: ^0.0.1-beta.2
```

The library offers you two ways to develop a plugin. The repository has branches which can help you in setting the initial configuration of your plugin for the same.
1. Object-Oriented plugins using [template/class](https://github.com/dart-lightning/lndart.cln_plugin/tree/template/class)
2. Functional plugins using [template/function](https://github.com/dart-lightning/lndart.cln_plugin/tree/template/function)

Once your template is set up, you can start working on implementing your plugin. The `/lib/src/` directory for the most part is what you will be working on and will contain the core implementation of your plugin.

We can get started by extending the `Plugin` class in our code. Then the simplest thing you can do is instantiate an object of it inside the `main()` of your plugin as shown below.
```dart
class MyPlugin extends Plugin {}

void main() {
  var plugin = MyPlugin();
  plugin.start();
}
```

Code that goes inside this subclass will form the business logic of your plugin. The `start()` will be called when core lightning marks your plugin as ready and the plugin will communicates with lightningd through it's stdin and stdout.

The `lndart.cln` API exposes several methods which can be used to build robust plugins conforming to the core lightning and JSON-RPCv2.0 standards. A plugin can interact with Core Lightning in several ways and we have methods to configure this interaction:

- RPC command line options using the `registerOption()`.
- RPC methods using the `registerRPCMethod()`.
- RPC Hook calls using the `registerHook()`.
- Subscriptions using `registerSubscriptions()`.

Lets understand how we can use these methods in detail.

### 1. registerOption()

Core Lightning allows plugins to register their own command line options which are exposed through the lightning daemon i.e lightningd.
```bash
lightningd --my_cli_option
```
A typical registerOption() looks something like this and as you can see most of the fields are self explanatory.
```dart
registerOption(
        name: "my_option",
        type: "string",
        def: "hello this string represents the default my_option value.",
        description: "This is an example of how the option looks like");
```
When we start lightningd by enabling this plugin, you'll notice we can now use the `--my_option=` option to assign a new value to it, i.e. `--plugin="This is a new value for this option"`.

### 2. registerRPCMethod()

This is probably the most versatile method in your arsenal. The registerRPCMethod() can be used to invoke a custom dart callback at any time. Yes, you heard it right. The applications are endless and it can be tailored to suit your need. You can return custom responses by interacting with core lightning. The is a minimal overhead to set this up.
```dart
registerRPCMethod(
    name: "my_custom_method",
    usage: "",
    description: "This string is the description of my custom RPC method.",
    callback: (plugin, request) => myCustomMethod(plugin, request));
```
We use the `callback` field to assign a callback to our RPC method. A simple callback would look like this.
```dart
Future<Map<String, Object>> myCustomMethod(Plugin plugin, Map<String, Object> request) {
    log(level:'debug', message:"This is a successful method call.");
    return Future.value({
        "my_custom_option": getOpt(key: "my_option") ?? "Option not registered!",
    });
}
```
In this case we use the `getOpt()` to return the value of the option we registered earlier or a default placeholder string "Option not registered!" if it was not found for some reason.

### 3. registerHook()
Hooks allow a plugin to define custom behavior for lightningd without modifying the Core Lightning source code itself. The below code snippet registers a hook that is triggered by core lightning when any rpc command is executed.

```dart
registerHook(name: "rpc_command", callback: onCustomRPCCommand);
```
This call registers a callback `onCustomRPCCommand` to log into Core Lightning but can be customised with new behavior.
```dart
Future<Map<String, Object>> onCustomRPCCommand(
    Plugin plugin, Map<String, Object> request) {
    log(level: "info", message: "This is a hook callback!");
    return Future(() => {"result": "continue"});
}
```

### 4. registerSubscriptions()

Subscriptions allow plugins to respond using custom callbacks to Core Lightning notifications. We can assign a callback function with custom behavior to any of the internal notifications in a very simple and push-based mechanism.
```dart
registerSubscriptions(
    event: 'connect',
    callback: customNotificationMethod);
```
The above call would make the `customNotificationMethod()` to be registered as a callback to whenever a new connection to a peer is established.
```dart
Future<Map<String, Object>> customNotificationMethod(
    Plugin plugin, Map<String, Object> request) {
  log(level: 'debug', message: 'Notification received!');
  return Future.value({});
}
```

Plugins can be written in any programming language and can be compiled into an executable and run at the startup of lightningd using the `--plugin=` option to register the plugins that should be started.

In dart we can compile our plugin into an executable in this way.
```bash
dart compile exe path_to_the_main_file
```

This executable can then be run during the startup of lightningd using the `--plugin=` option.

This is perhaps everything you need to know to start developing a plugin for Core Lightning in Dart.
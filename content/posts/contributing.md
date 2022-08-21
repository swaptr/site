---
title: "Contributing to lndart.cln"
author: "swaptr"
description: "This blog post elaborates on how to contribute to the lndart.cln repository."
date: "2022-08-14"
tags: [lightning-network]
---

[lndart.cln](https://github.com/dart-lightning/lndart.cln) is a Dart framework for Core Lightning. This package is really handy if you wish to interact with your lightning node and develop some cool plugins for it. This post offers detailed guide on how you can start contributing to the lndart.cln package.

The only requirement is to have a working Dart SDK. You can follow instructions from [here](https://dart.dev/get-dart) to get up and running.

To get started, one should first fork and clone the repository [lndart.cln](https://github.com/dart-lightning/lndart.cln). The instructions on doing that can be found [here](https://docs.github.com/en/get-started/quickstart/fork-a-repo).  

Let's first understand the structure of this repository. 

The repository is structured as a monorepo which means it houses several related tools together, four in this case:
```bash
packages
|
└─── cln_common
|
└─── cln_plugin
|
└─── lnlambda
|
└─── rpc
```

- The [lndart.cln_common](https://github.com/dart-lightning/lndart.cln/tree/main/packages/cln_common) package houses all the utilities and interfaces for other subsidiary packages to use.  
- [lndart.cln_plugin](https://github.com/dart-lightning/lndart.cln/tree/main/packages/cln_plugin) is a Dart library that provides ergonomic support to developing custom plugins for Core Lightning. It provides comprehensive and robust support to developing plugins using functional and object-oriented methods.  
- [lnlambda](https://github.com/dart-lightning/lndart.cln/tree/main/packages/lnlambda) is a minimal interface to run lnlambda function with Dart.   
- The [lndart.rpc](https://github.com/dart-lightning/lndart.cln/tree/main/packages/rpc) package is a cool tool to interact with the Core Lightning RPC interface.

If you are looking to contribute to the repository, it is highly recommended that you go through the [Hacking Guide](https://docs.page/dart-lightning/lndart.clightning/dev/MAINTAINERS).

This is a well documented package with essential examples guiding you along the way in case you get stuck. The documentation can be found [here](https://docs.page/dart-lightning/lndart.clightning).

You can use any IDE or text editor of your choice to hack the codebase or develop plugins. A good recommendation would be [IntelliJ IDEA](https://www.jetbrains.com/idea/) with the [Dart plugin](https://plugins.jetbrains.com/plugin/6351-dart) installed.

It is expected that any patches submitted are well formatted. To ensure this you should run `make fmt` inside the package directory. You can also use the dart formatter for this by running `dart format file-name`.

All code must be written with the expectation of a successful release. As with **lndart.cln**, we have the possibility to release the packages either individually or simultaneously. To make our lives easier, we can generate the changelog before a release with a minimal overhead using [changelog.dart](https://github.com/vincenzopalazzo/changelog.dart).  
The instructions on how to make a release can be summarized as following:
1. Bump the version number in the package and the `changelog.json` according to the package versioning [guide](https://dart.dev/tools/pub/versioning). 
2. Generating the changelog for the package(s) is essential for every release. To generate the changelog you can refer to [here](https://docs.page/dart-lightning/lndart.clightning/dev/MAINTAINERS#how-to-make-the-release).
3. Make a GitHub release using the following tags:
   - **Minor release**: `{package_name}-v{version_number}`
   - **Major release**: `v{version_number}`
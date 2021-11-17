DartPad allows users to import a few different kinds of code libraries:

* Dart SDK libraries like `dart:convert`
* Flutter packages like `package:flutter/widgets.dart`
* A small group of pre-approved and tested packages from the pub ecosystem, like `package:provider` or `package:http`

It does this by analyzing and compiling each user's code inside a stubbed-out project that includes each available library as a dependency. You can see where this is set up in [`/lib/src/project.dart`](https://github.com/dart-lang/dart-services/blob/8587fdb2b47676234c40b43105295598d619a215/lib/src/project.dart#L77) in the dart-lang/dart-services repo. DartPad uses a stateless backend, which means that each individual instance of dart-services needs to be set up with exactly the same list of packages loaded up and ready to go, since a user's analysis and compilation requests could go to any one of them at any time, even during a single session.

This approach keeps things simple and reliable (and is what made it possible for the DartPad team to implement package support at all), but it also introduces some restrictions. For example, each supported package/version needs to work nicely with the others. If two packages require conflicting versions of the same dependency, for instance, it's a blocker. In addition, because DartPad is a very unique execution environment that neither Flutter nor most packages were truly designed for, each package must be manually tested in order to guarantee that it works.

This is all a fancy way to say that adding support for new packages requires a lot of work, and in some cases simply won't be possible. The `google_mobile_ads` plugin, for example, is destined never to appear in DartPad. However, updating the allowed list of packages with new libraries and occasionally removing an old one is definitely part of the plan going forward.

Here is the process in a nutshell:

* Anyone who would like DartPad to support a new package should check to see if [an issue](https://github.com/dart-lang/dart-pad/issues?q=is%3Aissue+label%3Asuggested-package+) for that package already exists.
  - If it does, upvote it with a "thumb's up" reaction.
  - If it doesn't, create a new issue with the name of the package and a brief description of why it would be good to support.
* Once every quarter or so, the team will take a look at the existing package list and all the issues, and then:
  - Remove support for any packages that are no longer needed/maintained.
  - Look at suggestions with a significant number of upvotes, and either:
    - Add support, or
    - (if support isn't possible) close the issue and explain why.




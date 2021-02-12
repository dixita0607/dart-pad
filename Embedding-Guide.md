**So, you want to embed DartPad in a web page?**
No worries, it's simple!
This page tells you how to do that,
using a gist or a code block to provide
the code that the embedded DartPad displays and runs.

> **Note:**
> Support for code blocks is new, so this page mostly covers gists.
> The last section has the additional details you'll need
> to use code blocks instead of gists.

You can use embedded DartPads for simple, runnable demos/examples
or for more complex, tested exercises.
To see a good example of using both examples and exercises,
visit the [async-await codelab](https://dart.dev/codelabs/async-await).

## Overview

Here's an example of code that embeds a simple example in a web page:

```html
<iframe src="https://dartpad.dev/embed-inline.html?id=5d70bc1889d055c7a18d35d77874af88"></iframe>
```

That code embeds a DartPad containing the source code at http://gist.github.com/5d70bc1889d055c7a18d35d77874af88.

To customize the look of the embedded DartPad, you can add
[query parameters](#query-parameters) like `split` and `theme`:

```html
<iframe src="https://dartpad.dev/embed-inline.html?id=5d70bc1889d055c7a18d35d77874af88&split=80&theme=dark"></iframe>
```

Specify the width and the height as style parameters of the iframe:

```
<iframe style="width:400px;height:400px;" src="https://dartpad.dev/embed-inline.html?id=5d70bc1889d055c7a18d35d77874af88&split=80&theme=dark"></iframe>
```

Which [embed URL](#embedding-choices) you should use depends on how you want DartPad
to display and run your code. For example, a basic Dart example might use `embed-inline.html`,
but Flutter examples must use `embed-flutter.html`.

An embedded DartPad looks for the following files:

- **main.dart**: The code to display — for example, a working or nonworking method.
- **test.dart** _(optional)_: A `main()` function that tests the above code. Also contains
any classes, functions, constants, etc. to be used by main.dart but not displayed.
- **solution.dart** _(optional)_: The ideal final state of main.dart, once the user
has made all the changes you've asked them to make. The solution
code is hidden until the user asks to see it.
- **hint.txt** _(optional)_: A text hint that the user can request to see,
to help them complete the exercise. Examples: "Try X or Y," "Have you
considered Z."


## Embedding choices

DartPad offers 4 different embedding choices for shared code:

1. Dart and console: [embed-dart.html](#embed-darthtml)
2. Dart and console (minimal): [embed-inline.html](#embed-inlinehtml)
3. Flutter, console, and HTML output: [embed-flutter.html](#embed-flutterhtml)
4. Dart, console, and HTML output (with options to modify HTML/CSS): [embed-html.html](#embed-htmlhtml)

### embed-dart.html

A simple interface for Dart code that includes an editor and console on the
right-hand side.

![Dart screenshot](https://user-images.githubusercontent.com/969662/61907271-01599200-aee2-11e9-9c68-4b874f8937e1.png)

### embed-inline.html

Another simple interface for Dart code with a console beneath the editor.

![Inline screenshot](https://user-images.githubusercontent.com/969662/61926324-fecc5c00-af24-11e9-9971-9de427699dcb.png)

### embed-flutter.html

A layout for editing and running Flutter code. When this layout is used, code is
compiled with DDC rather than dart2js, and the Flutter for web packages are
available for import.

![Flutter screenshot](https://user-images.githubusercontent.com/969662/61907277-03235580-aee2-11e9-8940-082322197791.png)

### embed-html.html

A layout for editing `dart:html` projects. Editors for HTML, CSS, and Dart are
included, and HTML output is displayed to the right.

![HTML screenshot](https://user-images.githubusercontent.com/969662/61907279-04ed1900-aee2-11e9-933e-d8552663b0ed.png)


## Query parameters

DartPad pages use query parameters in the URL to retrieve & show certain information.
This means that users can configure how to show their code by quickly changing the URL.

Separate multiple query parameters using ampersand (`&`).

DartPad looks for the following parameters in its query string:

- **split**: Percentage of the iframe width to use for the editor (the rest may
be used by the console or Flutter/HTML output).
- **theme**: Set this to 'dark' to use the dark theme (seen in the first
screenshot above).
- **null_safety**: Set this to true to enable null safety mode.
- **run**: Set this to 'true' to auto-run the sample once loaded.
- **id**: ID of a GitHub gist to load into the editor
- **sample_id**: ID of an API doc sample to load into the editor (see https://api.flutter.dev/snippets/index.json for a list)
- **sample_channel**: If this parameter is set to "master", DartPad will load API Doc samples from the master doc server (master-api.flutter.dev). Any other values (or no value) will cause DartPad to load from the stable doc server (api.flutter.dev).

The following parameters are used together when loading a sample directly from a GitHub repo:
- **gh_owner**: Owner of the GitHub account.
- **gh_repo**: Name of the repo within the above account.
- **gh_path**: Path to a `dartpad_metadata.yaml` file within the repo.
- **gh_ref**: (optional) Branch to use when loading the file. Defaults to `master`.

## Styling the editor

The embedded editor styles its contents according to the
desired theme (either light or dark). No border is added to the iframe contents
by DartPad, nor does it attempt to size itself. Developers adding DartPad
iframes to their pages should use CSS within their own pages to style these
properties.

## Testing the user's code

In addition to running code and displaying the output, the new embed UI can
present exercises to be completed by the user and then test their responses. It
does this by combining the user's code with additional "test" code provided by
the exercise author and executing the result as if it were a single file.

An example exercise might look like this:

**User's code**
```dart
String stringify(int x, int y) {
  return '$x $y';
}
```

**Test code**
```dart
void main() {
  try {
    final str = stringify(2, 3); 

    if (str == '2 3') {
      _result(true);
    } else if (str == '23') {
      _result(false, ['Test failed. It looks like you forgot the space!']);
    } else if (str == null) {
      _result(false, ['Test failed. Did you forget to return a value?']);
    } else {
      _result(false, ['That\'s not quite right. Keep trying!']);
    }
  } catch (e) {
    _result(false, ['Tried calling stringify(2, 3), but received an exception: ${e.runtimeType}']);
  }
}
```

When combined and executed, the `main` function in the test code runs,
calls into the user's code, and validates the result. The `_result` function is
provided by DartPad in the scope in which the test code executes and can be used
to report the result of a test. It takes a single boolean indicating success or
failure, and a list of strings to be displayed to the user with the result.

> **Tip:**
> To work on the test code locally (in an IDE, for example),
> create a Dart file that contains everything in main.dart (or solution.dart) and test.dart,
> and then add a [mock _result() function](https://gist.github.com/legalcodes/9fbfea4739aa064455e836dbc84cc62f).

## Converting code blocks to DartPad

DartPad can "inject" itself into a web page by replacing code blocks.

### Step 1: Include the script
Include `https://dartpad.dev/inject_embed.dart.js` into your page with the `defer` attribute:

```html
<script type="text/javascript" src="https://dartpad.dev/inject_embed.dart.js" defer></script>
```

Alternatively, if you are using Jekyll, use the `js:` field at the top of the
article:

```
title: "Codelab: using DartPad"
js: 
  - defer: true
    url: https://dartpad.dev/experimental/inject_embed.dart.js
```

### Step 2: Add a code snippet

> **Important:** You should always [sanitize code snippets](https://stackoverflow.com/questions/1273145/what-do-i-need-to-escape-inside-the-html-pre-tag) to escape HTML characters. 
If you are using a site generator or a markdown library, these tags will be sanitized automatically.

In Markdown:

````
```run-dartpad:theme-light:mode-flutter:run-true
main() => print("Hello, World!");
```
````

In HTML, use `<pre>` and `<code>` tags:

```
<pre>
    <code class="language-run-dartpad:theme-light:mode-flutter:ga_id-example1">
        main() =&gt; print(&quot;Hello, World!&quot;);
    </code>
</pre>
```

### Step 3 (optional): Add optional files

To provide virtual files for test, solution, or hint code, add the following before and after each virtual file:

```
{$ begin filename.dart $}
...
{$ end filename.dart $}
```

For example, to specify the main, test, solution, and hint content, you can have this:

````
```run-dartpad:theme-light:mode-flutter:run-true
{$ begin main.dart $}
main() => print("Hello, World!");
{$ end main.dart $}
{$ begin solution.dart $}
...solution goes here
{$ end solution.dart $}
{$ begin test.dart $}
...test code goes here
{$ end test.dart $}
{$ begin hint.txt $}
...hint text goes here
{$ end hint.txt $}
```
````

### Options

The Markdown [info string][] must be `run-dartpad` followed by options separated
by `:`. The following options are supported:

Theme options:
- `theme-light` (default)
- `theme-dark`

Mode options:
- `mode-dart` (default)
- `mode-flutter`
- `mode-html`
- `mode-inline`

Auto run:
- `run-true`
- `run-false` (default)

Null safety:
- `null_safety-true`
- `null_safety-false` (default)

Split:
- `split-70`

Google analytics ID, used to identify separate samples in an article or codelab:
- `ga_id-myCustomID123`

### Example

An example is provided in `web/example/inject.html` and can be viewed in the
[embeddings demo][].

### Motivation
DartPad typically uses GitHub Gists to display code snippets. For example, to
add DartPad to a page, you can add an `iframe` with the URL to DartPad:

```
<iframe src="https://dartpad.dev/embed-flutter.html?id=[GIST_ID]"></iframe>
```

However, storing code in GitHub Gists is not always desirable:

- Gist changes happen in a different repository with a different commit history.
- Gists only have one owner, and can't take advantage of collaboration features
of a repo
- In an article or codelab, gists are opaque to the writer and more difficult to
edit than inline snippets

## Setting the ga_id parameter

To give a meaningful name to snippets, you can assign a `ga_id` parameter:

```
<iframe src="embed-flutter.html?theme=dark&run=false&split=false&ga_id=example1"></iframe>
```

Alternatively, using the inject script will send a virtual pageview with this query parameter to GA:

```
    <pre>
        <code class="language-run-dartpad:theme-dark:mode-flutter:ga_id-example1">foo</code>
    </pre>
```

[info string]: https://spec.commonmark.org/0.29/#info-string
[embeddings demo]: https://dartpad.dev/example/inject.html

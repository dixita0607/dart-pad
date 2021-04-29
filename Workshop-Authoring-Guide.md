DartPad can load and display step-by-step instructional content. [See an example
here][flutter-example].

# File format

A workshop contains a `meta.yaml` file and a directory for each step:

```
my_workshop/
    meta.yaml
    step_01/
            instructions.md
            snippet.dart
            solution.dart
    step_02/
            instructions.md
            snippet.dart
            solution.dart
```

The `meta.yaml` file should be configured

```yaml
name: Example workshop
type: dart # This should be either "dart" or "flutter".
steps:
  - name: Step 1
    directory: step_01
    has_solution: true
  - name: Step 2
    directory: step_02
    has_solution: false
```


The `solution.dart` file is optional. If the step doesn't have a `solution.dart`
file, the `has_solution` parameter must be set to `false`.


# Hosting

Codelabs can be hosted on GitHub or using a web server. It is recommended to use
the web server option for in-person events.


## GitHub


> **Warning**: rate limiting Codelab files are fetched using the GitHub API,
which is rate limited. Running with more than 1-2 users in the same location
will quickly hit these limits. If you are running an in-person workshop, use the
web server hosting option instead.


**Hosting**

Commit and push the files to a GitHub repository (subdirectories are allowed.)

**Viewing**

Create a URL starting with `dartpad.dev/workshops.html` with the following query
parameters:

* **gh_owner**: The name of the GitHub user or organization
* **gh_repo:** The name of the GitHub repository
* **(optional) gh_ref**: The branch or commit to load from  (defaults to 'main')
* **(optional) gh_path**: The relative path to the directory containing
  `meta.yaml` (defaults to the root of the repository)

**Example:**

```
http://dartpad.dev/workshops.html?gh_owner=flutter&gh_repo=codelabs&gh_ref=main&gh_path=dartpad_codelabs/src/example_flutter
```

## Web server

**Hosting**

1. Host the files using a web hosting provider such as [Firebase
   Hosting](https://firebase.google.com/docs/hosting), Cloud Storage, or S3.
2. Configure your server's
   [CORS](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) headers to
   allow requests from `dartpad.dev`.

**Viewing**

Create a URL starting with  `dartpad.dev/workshops.html` with a  `webserver`
query parameter containing the URL where the files are hosted.

**Example**

For files hosted at `https://my-firebase-app.web.app/path/to/my_workshop`

```
http://localhost:8000/workshops.html?webserver=https://my-firebase-app.web.app/path/to/my_workshop
```

## Displaying images

We recommend hosting images alongside the workshop files. First, create an
`images` directory with the images you would like to display:

```
my_workshop/
    images/
        dash.png
    step_01/
    step_02/
    meta.yaml
```

Then use a `NetworkImage` widget with the URL that will host the workshop:

```
Image.network('https://my-firebase-app.web.app/example_dart/images/dash.png')
```

[flutter-example]: https://dartpad.dev/workshops.html?webserver=https://dartpad-codelabs-experimental1.web.app/example_flutter
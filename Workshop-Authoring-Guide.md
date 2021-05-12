DartPad can load and display step-by-step instructional content. [See an example
here][flutter-example].

# Setup 
## Directory structure

Create a new directory containing a `meta.yaml` file and a directory for each
step:

```
my_workshop/
    meta.yaml
    step_01/
            instructions.md
            snippet.dart
            solution.dart # optional
    step_02/
            instructions.md
            snippet.dart
            solution.dart # optional
```

Each step directory must contain an `instructions.md` file and a `snippet.dart`
files. The `solution.dart` file is optional.

## Metadata file format 

`meta.yaml` has the following structure:

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

Set the `has_solution` parameter to true if the step has a `solution.dart` file.

# Hosting 
## Hosting using a web server

**Hosting**

1. Host the files using a web hosting provider such as [Firebase
   Hosting](https://firebase.google.com/docs/hosting), Cloud Storage, or S3.
2. Configure your server's
   [CORS](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) headers
   to allow requests from `dartpad.dev`.

# Opening

## Opening from a web server
Create a URL starting with  `dartpad.dev/workshops.html?webserver=` followed by
the URL where the files are hosted. For example, if the workshop files are
hosted at `https://my-firebase-app.web.app/path/to/my_workshop`, then the URL
is:

```
http://dartpad.dev/workshops.html?webserver=https://my-firebase-app.web.app/path/to/my_workshop
```

## Opening from GitHub

> **Warning: Rate limiting:** Codelab files are fetched using the GitHub API,
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

# FAQ

## Displaying images

Relative paths are not supported for images. We recommend hosting your images
alongside the workshop files.

For example, create an `images` directory with the images you would like to
display:

```
my_workshop/
    images/
        dash.png
    step_01/
    step_02/
    meta.yaml
```

Then use a `NetworkImage` widget with the full URL where workshop files are
hosted:

```
Image.network('https://my-workshop.web.app/example_dart/images/dash.png')
```

Or use Markdown to display an image in the instructions

```
![alt text](https://my-workshop.app/example_dart/images/dash.png)
```

[flutter-example]: https://dartpad.dev/workshops.html?webserver=https://dartpad-codelabs-experimental1.web.app/example_flutter
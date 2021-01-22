## The DartPad UI release process

- We follow a normal pull request, review, merge to master github workflow.
- New changes get batched up in master. Our CI bot runs on each PR; we maintain the master branch in an always-shipping state.
- When we're ready to release, we merge all of master into the `prod` branch (`git checkout prod`; `git merge master`). We only release production version from this branch.
- We verify that the `web/app.yaml` has the`secure: always` line enabled. The `tool/grind.dart` `deploy` task also does verification of the branch name and `web/app.yaml` settings.
- We then deploy from this branch (`grind deploy`, then a manual push to appengine; see below).
- Upstream changes to prod; `git push origin prod` or `git push upstream prod` depending on your local clone. We need to make sure to upstream changes to the prod branch so that multiple developers can release.

This release workflow lets us continue to make forward development progress, while at the same time having a stable (and patchable) shipping prod version.

## Patch releases

- Between releases, if we have a critical fix, we apply the fix to the master branch. We then do a `git cherry-pick` of the commit(s) into the prod branch, and re-deploy from there.
- `git push origin prod` or `git push upstream prod` depending on your local clone

## Pushing to App Engine

- After following the above release process steps, `git checkout prod`
- `grind deploy`; this will create a deployable version of the prod website in `build`
- Deploy the app from the gcloud appengine CLI, use the no-promote flag to promote from the cloud console:
  - `gcloud app deploy build/app.yaml --project=dart-pad --no-promote`
- From the App Engine cloud console - [console.cloud.google.com](https://console.cloud.google.com/) - make that deployed version of the app the default

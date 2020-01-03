# Deploy Demo

A simple example showing multi-stage deployments using GitHub Actions.

## Just an example

Note that the testing and deployment logic present in `script/test` and
`script/deploy` are merely illustrative and need to be modified to do anything
meaningful.

## Development Flow

Create a topic branch:

```shell
$ git checkout -b change-abc
```

Make your changes...

Then, commit and push:

```shell
$ git commit -m "Changed ABC"
$ git push --set-upstream origin change-abc
```

On GitHub, create a pull request.

:gear: The [CI workflow](.github/workflows/ci.yml) will run. (Tip: In your repo, set **Require status checks to pass before merging** for `ci` on your `master` branch by visiting **Settings** :arrow_right: **Branches** :arrow_right: **Add rule** to prevent non-passing PRs from being merged.)

When CI passes (and your code is ready, reviewed, etc), merge the PR.

:gear: The [Deploy to Staging workflow](.github/workflows/deploy-staging.yml) will run. Note that this workflow will also run the tests to ensure they pass post-merge before deploying.

Once deployed to staging, after any and all manual testing is complete, promote staging to production by merging your changes from `master` into the `production` branch. One way to do this is by a specific deployment branch:

```shell
# While on the 'master' branch; just pushing this to an alias
$ git push origin master:change-abc-deploy
```

Then submitting a pull request to merge `change-abc-deploy` into `production`.

:gear: The [CI workflow](.github/workflows/ci.yml) will run. (Tip: In your repo, set **Require status checks to pass before merging** for `ci` on your `production` branch by visiting **Settings** :arrow_right: **Branches** :arrow_right: **Add rule** to prevent non-passing PRs from being merged.)

When 100% ready, merge the pull request.

:gear: The [Deploy to Production workflow](.github/workflows/deploy-production.yml) will run. Note that this workflow will also run the tests to ensure they pass post-merge before deploying.

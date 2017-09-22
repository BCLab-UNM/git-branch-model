# BCLab Git Branching Model

The workflow below assumes your repo contains an existing `master` branch. This branching model is based on Vincent Driessen's "[A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/)," which presents an excellent visual and programmatic guide to git branching with style.

In the following examples `feature-foo` or `hotfix-foo` should be replaced with a short phase describing the feature or hotfix.

### Feature branches

Feature branches are used to implement new enhancements for upcoming releases. A feature branch should be *ephemeral*, i.e. it should only last as long as the feature itself is in development. Once the feature is completed, it must be merged back into the `master` branch and/or discarded.

Consider an example in which we to implement a new feature called `feature-foo`:

Begin by switching to a new branch `feature-foo`, branching off of `master`:

```
$ git checkout -b feature-foo master
```

You should use `feature-foo` to implement and commit all changes required for your new feature.

- Make many small commits so that the history of development for your feature branch is clear and so that it is easy to pinpoint and edit or cherry-pick specific commits if necessary.
- Avoid merging your feature branch with other feature branches being developed in parallel. This *MIGHT* cause a lot of problems down the line when doing a pull request to merge your feature back into the master branch.

When your feature is complete, push it to the remote repo to prepare for a pull request.

```
$ git push -u origin feature-foo
```

Next, you will want to [create a pull request](https://help.github.com/articles/creating-a-pull-request/), so that the repository administrator can review and merge your feature. You will want to create the following pull request:

* base: `master`, compare: `feature-foo`

Finally, after your pull request is accepted, clean up your local repositories by deleting your local feature branch:

```
$ git branch -d foo
```

The repository administrator is responsible for deleting the remote copy of the feature branch.

All pull requests must be reviewed before they can be merged into the `master` branch. Reviewers may ask questions or make suggestions for edits and improvements before your feature can be merged. If your feature branch pull request is not accepted, make the necessary adjustments or fixes as indicated by the repository administrator and redo the pull request.

### Hotfix branches

Hotfix branches are used to implement critical bug fixes in the *production version* of your code, i.e. code that is currently being used for experimental analysis. As such, they should always originate from the `master` branch.

Now, consider an example in which we discover a critical bug in the current production release of your code:
Begin by switching to a new branch `hotfix-foo`, branching off of `master`:

```
$ git checkout -b hotfix-foo master
```

Implement your bug fix in `hotfix-foo`, commit your changes, then push your hotfix branch back into the remote repository to prepare it for a pull request:

- Make many small commits so that the history of development for your hotfix branch is clear and so that it is easy to pinpoint and edit or cherry-pick specific commits if necessary.

```
$ git push -u origin hotfix-foo
```

Next, you will want to [create a pull request](https://help.github.com/articles/creating-a-pull-request/), so that the repository administrator can review and merge your hotfix:

* base: `master`, compare: `hotfix-foo`

Finally, after your pull request is accepted, clean up your local repositories by deleting your hotfix branch:

```
$ git branch -d hotfix-foo
```

The repository administrator is responsible for deleting the remote copy of the hotfix branch and updating the version tag for the `master` branch.

All pull requests must be reviewed before they can be merged into the `master` branch. Reviewers may ask questions or make suggestions for edits and improvements before your feature can be merged. If your feature branch pull request is not accepted, make the necessary adjustments or fixes as indicated by the repository administrator and redo the pull request.

# BCLab Git Branching Model

The workflow below assumes your repo contains existing `master` and `development` branches. This branching model is based on Vincent Driessen's "[A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/)," which presents an excellent visual and programmatic guide to git branching with style.

**NOTE: Several steps for our Git Branching Model has changed. Crossed out steps indicate the old way we used to do things, and new steps have been added in their place.**

In the following examples 'foo' should be replaced with a short phase describing the feature or hotfix.

### Feature branches

Feature branches are used to implement new enhancements for upcoming releases. A feature branch should be *ephemeral*, i.e. it should only last as long as the feature itself is in development. Once the feature is completed, it must be merged back into the `development` branch and/or discarded.

Consider an example in which we to implement a new feature called `foo`:

Begin by switching to a new branch `foo`, branching off of `development`:
```
$ git checkout -b feature-foo development
```
You should use `feature-foo` to implement and commit all changes required for your new feature.

~~~When your feature is complete, merge your feature branch back into `development`:~~~

~~~$ git checkout development~~~

~~~$ git merge --no-ff foo~~~

When your feature is complete, push it to the remote repo to prepare for a pull request.
```
$ git push -u origin foo
```
~~~Then, push your local changes to the remote repository:~~~

~~~$ git push origin development~~~

Next, you will want to [create a pull request](https://help.github.com/articles/creating-a-pull-request/), so that the repository administrator can review and merge your feature. You will want to create the following pull request:
* base: `development`, compare: `foo`

~~~Finally, delete your feature branch:~~~

~~~$ git branch -d foo~~~

Finally, after your pull request is accepted, clean up your local repositories by deleting your feature branch:
```
$ git branch -d foo
```
The repository administrator is responsible for deleting the remote copy of the feature branch.

If your feature branch is not accepted, make the necessary adjustments or fixes as indicated by the repository administrator and redo the pull request.

### Hotfix branches

Hotfix branches are used to implement critical bug fixes in the *production version* of your code, i.e. code that is currently being used for experimental analysis. As such, they should always originate from the `master` branch, who's version tag must be incremented after the hotfix branch is merged.

First, identify the current tag for the master:
```
$ git describe --tags
```
Now, consider an example in which we discover a critical bug in the current production release of your code:

Begin by switching to a new branch `hotfix-foo`, branching off of `master`:
```
$ git checkout -b hotfix-foo master
```
~~~Implement your bug fix in `hotfix-0.5.4`, commit your changes, then merge your hotfix branch back into `master`:~~~

~~~$ git checkout master~~~

~~~$ git merge --no-ff hotfix-0.5.4~~~

Implement your bug fix in `hotfix-foo`, commit your changes, then push your hotfix branch back into the remote repository to prepare it for a pull request:
```
$ git push -u origin hotfix-foo
```
~~~Don't forget to increment the patch value of your version tag by one and push all changes to the remote:~~~

~~~$ git tag v0.5.4~~~

~~~$ git push origin master --tag~~~

~~~Finally, merge the hotfix into `development` and push to the remote:~~~

~~~$ git checkout development~~~

~~~$ git merge --no-ff hotfix-0.5.4~~~

~~~$ git push origin development~~~

Next, you will want to [create a pull request](https://help.github.com/articles/creating-a-pull-request/), so that the repository administrator can review and merge your hotfix. You will want to create two pull requests:
1. base: `master`, compare: `hotfix-foo`
2. base: `development`, compare: `hotfix-foo`

~~~Then delete your hotfix branch:~~~

~~~$ git branch -d hotfix-0.5.4~~~

Finally, after your pull request is accepted, clean up your local repositories by deleting your hotfix branch:
```
$ git branch -d hotfix-foo
```
The repository administrator is responsible for deleting the remote copy of the hotfix branch and updating the version tag for the `master` branch.

If your feature branch is not accepted, make the necessary adjustments or fixes as indicated by the repository administrator and redo the pull requests.

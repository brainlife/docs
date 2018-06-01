
Although Brainlife does not require you to use any specific git branching schema and you should observe any standrad practice from your own group, here are some guidelines that we suggest.

## 1. git pull often

Before you start editing your app on your local machine each day, be sure to pull from origin. 

```
git pull
```

You should git pull as often as you can to reduce possible merge issues down the road. When you are done testing your changes, git push to origin master.

* By the way, see [git rebase](https://www.atlassian.com/git/tutorials/merging-vs-rebasing) if you are not familiar with rebasing. It helps our commit log to be clean.

## 2. Always work on master branch

When you are working on your app, you should always make changes on master. You can create as many local branches as you'd like (for each features you are working on) but when you are done, merge it to your own local master, then push the local master to the origin master. Pull from origin master, and push to origin master. Use local branches for you to organize what you are working on. Don't push local branches to origin.

## 3. Create branch for new versions

When we are done with making all changes, tested it, and ready to release it, we should create a new branch from master. You can easily create a new branch using github UI. 

![branch](/img/versioning.branch.gif)

Once you create a branch, you should update the BL app to point users to use that new branch.

![branch](/img/versioning.app.gif)

You could use tags instead of branches, but tags does not allow you make modifications like you can with branches. Tags are great to point to any particular commit point, but you can do that with just a plain commit ID also. We recommend creating new branches for each release.

## 4. Bug fix on master, then on branch.

If you find a bug after you create a branch, you first need to fix the bug on the master, test it, then apply the same fix on all branches that are affected. I recommend using command like [cherry-pick](https://git-scm.com/docs/git-cherry-pick) to apply specific changes on other branches. Again, we should not add any new features on branches (only bug fixes) for reproducibility. 

## 5. Semver

If you don't know what semantic versioning is, please read [https://semver.org/](https://semver.org/).

For branch names, we should use major and minor version (like "2.3"), but don't include the patch number, as patch numbers are incremeneted for each bug fixes and you don't need to create a new branch for each bug fixes. If you make non-backward compatible changes, you should consider registering a brand new Brainlife app with different major version number so that user can continue to submit your app with previous versions.

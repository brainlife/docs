# git submodules

Submodules allows you to share code across multiple github repo. If you maintain multiple similar Apps, this is a 
great way to refactor common code between different apps.

## Adding submodules

You can add submodule by running a command like on the root directory of your repo.

```
git submodule add https://github.com:brainlife/vistasoft vistasoft
```

!!! warning
    Please do not use ssh-key based URL for your submodule repo (like `git@github.com/something/something.git`). Brainlife will not be able to clone your App if you use git@ url. You can check/update it by editing `.gitmodules` file.






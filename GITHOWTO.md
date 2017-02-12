# Git/Github Workflow

This Document will describe the prefered workflow for contributing to the /frontend repository.

After following the instructions on the README.md and setting up your own personal Github account.  You will have cloned a copy of your fork of the central spring2885/frontend repository.  (ie you ran git clone https:...yourUserName/frontend and NOT git clone https:...spring2885/frontend).  Now you have a copy of your fork on your local machine and you can beguin editing/changing/adding to the App.  

However, as you are doing this others are also changing the App and your fork and your branch on your local machine are becoming further and further appart.  To prevent this you should frequently push your code.

### Do Once!

From /frontend run: 
```sh
$ git remote add upstream https://github.com/spring2885/frontend.git
```
Notice that we just told your local git repository about there being a remote master copy of the project.

### Do Often!

The best way to not have as many merge conflicts is to stay as close to the main branch as possible.  The more your fork and your branch diverge the more problems you will have to deal with--you may even overwrite someone else's work or worse loose your own in a merge.  So...

To pull down the latest copy of the App from the Master Branch, you will need to protect the work you have done.

from /frontend run: 
```sh
$ git status
```

This will tell you which branch (on your local machine) you are on as well as all of the files you have touched.  

Now run:
```sh
$ git add <fileName(s)>
```

or to add all of the files:

```sh
$ git add *
```

Now all of your files are in a state where they are ready to be commited.

Run:

```sh
$ git commit
```

This will open a Vi input for you to write a brief description of the changes you have made.  Ideally, include the issue #.  Don't forget hit 'i' to beguin typing and esc + ':wq' or esc + 'ZZ' to write and quit.

Now your changes are protected.  You can pull all of the changes that have been made to the Master Branch.

Run:

```sh
$ git pull upstream master
```

This will pull down the Master Branch.

Now you may have merge conflicts.  This happens most fequently when two people touch the same file.  Git will tell you which files have confilics and you will have to open them in the editor of your choice.  Once you open them you will see something like:

```sh
>>>>>>>>>HEAD
<your code>
=======
<your code>
<someone else's code>
==397re90r0098478
```

You will need to fix/remove the code that is between the first and last markers in the file.  This could be as simple as deleting a line or as complicated as working in new functionality.

After you have cleaned up all of the conflicted files run grunt and make sure that the App is working properly.  (HINT: use your console in the Browser to make sure there ar no errors!)

Once you have debugged any new errors you can now add and commit the files that had the merge conflicts.

Now run:

```sh
$ git push
```

This will push you local copy of the App to your Fork on your personal Github.

Now you can proceed with going to the main repo for the spring-2885 App on Github.  Find the new pull request button.  Press it.  Now on the left hand side of the screen you will see a like that says 'compare across forks'.  Click that. Leave the base fork as: spring2885/frontend and click the head fork and drop down to your user copy.  Add any more detail to the pull request comments and then hit the 'create pull request' button.

//TO GET FANCY:

If the pull is closing an issue, you can use any of these key words + #<issue number> and Github will automatically close that issue.

Key Words: 

  - close
  - closes
  - closed
  - fix
  - fixes
  - fixed
  - resolve
  - resolves
  - resolved

Example Usage:
```sh
This closes #34, closes #23, and closes #77
```


Now you can keep the App and the project up-to-date and deal with a lot less merge conflicts.  Happy Coding.


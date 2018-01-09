
---
# Table of Contents
* [Intro](#intro)
* [Git-files Download](#git-files-download)
* [Git Tips](#git-tips)
	* [How to commit stuff](#how-to-commit-stuff)
	* [How to add author info](#how-to-add-author-info)
	* [How to skip SSL certificate validation check](#how-to-skip-ssl-certificate-validation-check)
	* [How to enable the colors in shell](#how-to-enable-the-colors-in-shell)
	* [How to setup a credential-helper for git](#how-to-setup-a-credential-helper-for-git)
	* [How to prevent the Wall of Pink issue - Part 1 - Disable the default EOL conversion](#how-to-prevent-the-wall-of-pink-issue---part-1---disable-the-default-eol-conversion)
	* [How to prevent the Wall of Pink issue - Part 2 - Specify your own EOL conversion](#how-to-prevent-the-wall-of-pink-issue---part-2---specify-your-own-eol-conversion)
	* [How to refresh after changing gitignore and gitattributes](#how-to-refresh-after-changing-gitignore-and-gitattributes)
	* [How to rename a branch](#how-to-rename-a-branch)
	* [How to delete a branch](#how-to-delete-a-branch)
	* [How to create a diff between branches or commits](#how-to-create-a-diff-between-branches-or-commits)
	* [How to create a diff between branches or commits (2nd way - auto commit)](#how-to-create-a-diff-between-branches-or-commits-2nd-way---auto-commit)
	* [How to archive a git repository](#how-to-archive-a-git-repository)
	* [How to merge several git repositories](#how-to-merge-several-git-repositories)
	* [How to show tracked ignored files](#how-to-show-tracked-ignored-files)
	* [How to get the count of the commits made](#how-to-get-the-count-of-the-commits-made)
	* [How to delete a file from the whole history](#how-to-delete-a-file-from-the-whole-history)
	* [How to show and delete all the gitignored files](#how-to-show-and-delete-all-the-gitignored-files)
	* [Other Git Tips](#other-git-tips)

---
## Intro
This repository will contain all the fundamental tips for git.

---
## Git-files Download
* Git Clients (GUI)
	* [GitEye](http://www.collab.net/downloads/giteye)
	* [SourceTree](https://www.sourcetreeapp.com/)
	* [TortoiseGit](https://tortoisegit.org/download/)
	* [GitHub Desktop](https://desktop.github.com/)
	* [Others](http://git-scm.com/downloads/guis)
* Git Clients (Command-Line)
	* [msysgit](http://git-scm.com/downloads)
* Git Books
	* [Git Reference](http://git-scm.com/docs)
	* [Pro Git v2](http://git-scm.com/book/en/v2)
	* [Others](http://git-scm.com/doc/ext)

---
## Git Tips

---
##### How to commit stuff

Check which files are staged or untracked with `git status`:

```sh
$ git status
On branch master
Changes not staged for commit:
		modified:   TODO.txt
Untracked files:
		NEWSTUFF.txt
```

We can say that `TODO.txt` has been modified, and `NEWSTUFF.txt` is a new file yet to be added to the repository's index.

We have two ways to commit the modified `TODO.txt`:

1. We can add it, and then commit it:

	```sh
	$ git add TODO.txt
	$ git commit -m "your message"
	```

2. We can simply commit it with the `-a` option, which commits all the **unstaged modified** changes without previously using `git add <file>`:

	```sh
	$ git commit -am "your message"
	```

In either cases, `NEWSTUFF.txt` won't be added because it's an **untracked** file.

Below, the same example written above that includes `NEWSTUFF.txt` too:

1. We can add both, and then commit them:

	```sh
	$ git add TODO.txt NEWSTUFF.txt
	$ git commit -m "your message"
	```

2. We can simply commit the unstaged modified files with the `-a` option, and add the untracked one separately before the commit:

	```sh
	$ git add NEWSTUFF.txt
	$ git commit -am "your message"
	```

Beside `git add` we also have other instructions. Here a list:

```sh
# how to add files/folders
$ git add <filename1> <filename2> <filename3>

# how to delete files/folders
$ git rm <filename1> <filename2> <filename3>

# how to move files/folders
$ git mv <old_path_name> <new_path_name>

# how to discard changes of unstaged modified files/folders
$ git checkout -- <filename1> <filename2>

# how to discard changes of staged modified files/folders
$ git reset HEAD <filename1> <filename2>
$ git checkout -- <filename1> <filename2>

```

---
##### How to add author info

```sh
$ git config --global user.email you@example.com
$ git config --global user.name "Your Name"
```

---
##### How to skip SSL certificate validation check

```sh
$ git config --global http.sslverify false
```

---
##### How to enable the colors in shell

To enable the colors if they are not present:

```sh
$ git config --global color.ui true
```

On BSD, the colors are not displayed and you see ESC[r] instead. You solve it with:

```sh
$ git config --global core.pager 'less -R'
```

---
##### How to setup a credential-helper for git

* Temporary (stored in ram/cache)
	* On BSD/Linux

		* By default (timeout specified for 15 minutes)

			```sh
			$ git config --global credential.helper cache
			```

		* Custom (timeout specified for an hour)

			```sh
			$ git config --global credential.helper "cache --timeout=3600"
			```

	* On OSX

		```sh
		$ git config --global credential.helper osxkeychain
		```

	* On Windows

		```sh
		$ git config --global credential.helper wincred
		```

* Permanent (stored in file)
	* On BSD/Linux/OSX/Windows

		```sh
		$ git config --global credential.helper store
		```

		_Note: It's saved as plain text. Its path is usually `%userprofile%\.git-credentials` on Windows, and `~/.git-credentials` on BSD/Linux._

* [Other ways](http://stackoverflow.com/questions/5343068)

---
##### How to prevent the Wall of Pink issue - Part 1 - Disable the default EOL conversion

The "Wall of Pink" is the dreadful commit where it removes and re-adds all the lines of a file. It usually happens due to a diff misconception of handling cr/lf EOL characters.

Variables like `core.autocrlf` and `core.eol` are pointless "today", because, on mixed repositories, we usually have (e.g.) _.bat_ files with **crlf**, and _.sh_ files with **lf**.

If we would set the same EOL for all the text files, we could invalidate many of them. So it's better disable such feature, and, if you really need, use the `.gitattributes` file to handle them.

```sh
$ git config --global core.autocrlf false
```

_Note: Different git clients than msysgit handle `core.autocrlf`, `core.eol`, and `.gitattributes` differently causing ambiguous walls of pink._

_Note2: More info here [topic1](http://stackoverflow.com/questions/3206843), [topic2](http://stackoverflow.com/questions/2333424)_

---
##### How to prevent the Wall of Pink issue - Part 2 - Specify your own EOL conversion

As previously said, we can use the `.gitattibutes` file to handle the EOL character to use, some diff options, whether a file should be considered as text or binary, and so on.

If you've followed the "Part 1", you should have set `core.autocrlf` as **false**. We're going to use [`.gitattributes`](http://git-scm.com/docs/gitattributes) to specify, in this case, which EOL directive use.

We need to create such file in the repository's root folder, and not in sub-folders. An example of the `.gitattribute` is:

```ini
# default behavior
* text=auto

# git files
.gitattributes eol=lf
.gitignore eol=lf

# shell scripts
*.bat eol=crlf
*.sh eol=lf

*.rar binary
```

As you can imagine, .sh, .gitattributes, and .gitignore files's EOL will be converted to `lf`, and .bat files's EOL to `crlf`. The .rar files will be considered as `binary`.

The `text=auto` directive will enable EOL normalization on text files (it's a replacement of `core.autocrlf`), and use the `core.eol` (or eol directive) parameter to convert the EOL.

As summary, the meaning of these keywords:

* `text` turns on eol normalization
* `text=auto` turns on auto eol normalization based on `core.eol` or `eol` directive
* `-text` turns off eol normalization
* `eol=crlf` sets the eol normalization character to `crlf`
* `eol=lf` sets the eol normalization character to `lf`
* `diff` turns on textual diff patch generation
* `diff=<name>` turns on textual diff patch generation using the specified <name> driver
* `-diff` turns off textual diff patch generation (binary diff will be applied on the relative file)
* `binary` is a short-cut for `-text -diff`

_...to be completed..._

---
##### How to refresh after changing gitignore and gitattributes
Be sure you have committed everything before doing this.

You have to clear the repository Index and re-add all the files again.

Doing this, all the files will be processed again with the new rules of `.gitignore` and `.gitattributes`:

```sh
# clear the index
git rm -r --cached .

# re-add all the files
git add .

# commit the changes
git commit -m "+gitignore and gitattributes fix"
```

---
##### How to rename a branch

* To rename the current local branch:

	```sh
	$ git branch -m <new_branch_name>
	```

* To rename a specified local branch:

	```sh
	$ git branch -m <old_branch_name> <new_branch_name>
	```

After renamed the local branch, we have to "rename" the remote one too:

```sh
# pushing the local renamed branch to the remote repository
$ git push origin <new_branch_name>:refs/heads/<new_branch_name>

# delete the old branch from the remote repository
$ git push origin :<old_branch_name>

# fix upstream if required
$ git branch --set-upstream-to=upstream/<new_branch_name> <new_branch_name>

# fix old remote tracking branches
$ git fetch origin --prune
```

---
##### How to delete a branch

* To delete a local branch:

	```sh
	$ git branch -D <branch_name>
	```

* To delete a remote branch:

	```sh
	$ git push origin :<branch_name>
	```

---
##### How to create a diff between branches or commits

* To generate a textual diff between commits:

	```sh
	$ git diff b0a7f70..8c2aef3 > b0a7f70_vs_8c2aef3.diff
	```

* To generate a textual diff between commits:

	```sh
	$ git diff b0a7f70..8c2aef3 > b0a7f70_vs_8c2aef3.diff
	```

* To generate a textual diff between a branch "master" and "retsam" considering hierarchy (using `...` instead of `..`) and EOL skipping (using `--ignore-space-at-eol`):

	```sh
	$ git diff master...retsam --ignore-space-at-eol > master_vs_retsam.diff
	```

* To apply the generated textual diff somewhere:

	```sh
	# system built-in diff patcher
	$ patch -p1 < master_vs_retsam.diff

	# otherwise, git built-in diff patcher
	$ git apply -p1 < master_vs_retsam.diff
	```

* To generate a diff including binary data between `master` and `master~2` (two commits behind master):

	```sh
	$ git diff master~2..master --binary < masterb2_vs_master.diff
	```

* To apply the generated diff (binary data included) somewhere:

	```sh
	$ git apply -p1 < masterb2_vs_master.diff
	```

---
##### How to create a diff between branches or commits (2nd way - auto commit)

* To generate a diff between a branch "master" and "retsam" considering hierarchy (using `...` instead of `..`) and EOL skipping (using `--ignore-space-at-eol`):

	```sh
	$ git format-patch master...retsam --ignore-space-at-eol -k --stdout > master_vs_retsam.patch
	```

* To apply the generated diff somewhere:

	```sh
	$ git am -3 -k < master_vs_retsam.patch
	```

---
##### How to archive a git repository

You can decide to archive either an entire repository (whether or not including the history) or just one or more branches/tags.

* Including the history

	Usually, this is made using the [`git bundle`](https://git-scm.com/blog/2010/03/10/bundles.html) feature. People could also zip the whole repository (including the `.git` folder), but that's another story.

	We should now decide if we must create a _bundle_ of the whole repository or of just a branch.

	* Bundle of the whole repo

		To make a bundle of the whole repo, we have to do:

		```sh
		$ git bundle create <repository_name>.bundle --all
		```

		To import a complete bundle, we have two solutions:

		1. Clone the bundle to a new repository

			```sh
			$ git clone <repository_name>.bundle <repository_name>
			```

			In this case, there will be no local branches created so far beside HEAD, but only remote ones. You can print them by doing `git branch -r`.

			You have to checkout the branch you want to get a local copy of it: (e.g. getting master)

			```sh
			$ cd <repository_name>
			$ git checkout -b master origin/master
			```

		2. Pull the bundle refs, creating so, for each branch, a local one

			```sh
			# create the folder and enter it
			$ mkdir <repository_name>
			$ cd <repository_name>

			# create .git
			$ git init

			# fetch the data form the .bundle
			$ git pull <repository_name>.bundle *:*
			```

			_Note: It will print a warning regarding HEAD, but everything will be fine._

	* Bundle of a bunch of branches

		To make a bundle of a specific branch:

		```sh
		$ git bundle create <filename>.bundle master
		```

		To clone the specific branch:

		```sh
		$ git clone <filename>.bundle --branch master
		```

		To import the specific branch:

		```sh
		# checkout the specific branch or it will overwrite the current one even though you setup a different local target!
		# and pull from the bundle the branch we need
		$ git checkout -b master
		$ git pull <filename>.bundle master
		```

	_Note: In all the cases, you should re-set the upstream url_

* Not including the history

	In this case, [`git archive`](https://git-scm.com/docs/git-archive) is the most used. [Other Info](http://stackoverflow.com/questions/160608/do-a-git-export-like-svn-export)

	The only inconvenience is that it exports just a branch at a time.

	```sh
	# create a .tar
	$ git archive <branch_name> --format=tar -o ./<archivename>.tar

	# create a .tgz
	$ git archive <branch_name> --format=tgz > ./<archivename>.tgz

	# create a .tgz with the best compression from pipe
	$ git archive <branch_name> --format=tar | gzip -9 > ./<archivename>.tgz

	# create a .zip with default compression
	$ git archive <branch_name> --format=zip -o ./<archivename>.zip

	# create a .zip with the maximum compression
	$ git archive <branch_name> --format=zip -9 -o ./<archivename>.zip
	```

_Note: Another way is via [`git fast-export`](https://git-scm.com/docs/git-fast-export) | [`git fast-import`](https://git-scm.com/docs/git-fast-import)_

---
##### How to merge several git repositories
You have to upgrade git to the 2.x version and then follow these commands:

```sh
git remote add project-a <path/to/project-a>
git fetch project-a
git merge --allow-unrelated-histories project-a/master
git remote remove project-a
```

_Note: [Source discussion](http://stackoverflow.com/questions/1425892/#10548919)_

---
##### How to show tracked ignored files

```sh
# simply:
$ git ls-files -i --exclude-standard

# making an alias of it and calling it
$ git config --global alias.showtrackedignored "ls-files -i --exclude-standard"
$ git showtrackedignored
```

_Note: [Source discussion](http://stackoverflow.com/questions/9320218/#9370094)_

---
##### How to get the count of the commits made
Simply:

```sh
# show the count of all the commits of every branch
$ git rev-list --count --all
543

# show the count of the commits up to a specific branch/hash
$ git rev-list --count master
543

# show the count separated by committers
$ git shortlog -s
	11	Administrator
	32	Charlie Root
	571	martysama0134
```

---
##### How to delete a file from the whole history
If you have several commits, it will take almost 1 second per commit to remove the relative file and rewrite the history.

```sh
# check which files are the most bigger in your git repository
$ git verify-pack -v .git/objects/pack/*.idx | sort -k 3 -n | tail -5
c7dc0e78fa8cce3e0b306aff4ad27f1d40eafcc6 blob   199636 4185 164305
c8255b0cc3afc4017a6ac181d7beed60ce6bae6c blob   204126 15879 355126
1d81968daaefa9bdb047b225976fac04e8c8f715 blob   208219 43106 2308251
781944f1e587b1a7a4e2f0d5087158727cedb27b blob   349680 100481 4267720
b71a1218246b246286980bbbcd7d8abc311c1fae blob   377306 32752 598865

$ git rev-list --objects --all | grep 781944f1e587b1a7a4e2f0d5087158727cedb27b
781944f1e587b1a7a4e2f0d5087158727cedb27b Srcs/Tools/Mysql2Proto/Mysql2Proto/libmysql.lib

# check the starting commit containing that file
$ git log --oneline --branches -- Srcs/Tools/Mysql2Proto/Mysql2Proto/libmysql.lib
50687d0 +Mysql2Proto

# remove the relative file from the whole history from its starting commit
$ git filter-branch --index-filter 'git rm --ignore-unmatch --cached Srcs/Tools/Mysql2Proto/Mysql2Proto/libmysql.lib' -- 50687d0^..

# push all to the remote repository forcefully (some sites don't let you force it if you don't unprotect them before in their website's branch settings)
$ git push --force

# clean the local refs as well (if it takes too much when using git gc, git clone the repo again)
$ rm -Rf .git/refs/original
$ rm -Rf .git/logs/
$ git gc
$ git prune --expire now

# to see the current repository's size
$ git count-objects -v
```

In case of deleting a folder instead of a file, it's all the same except the `git rm -r` option in the `git filter-branch` command, just like this:

```sh
$ git filter-branch --index-filter 'git rm -r --ignore-unmatch --cached Srcs/Tools/Mysql2Proto/' -- 50687d0^..
```

_Note: [Source discussion](https://git-scm.com/book/en/v2/Git-Internals-Maintenance-and-Data-Recovery#_removing_objects)_

---
##### How to show and delete all the gitignored files
To show all the gitignored files (and directories with -d specified):

```sh
$ git clean -xnd
```

To delete all of them:

```sh
$ git clean -xdf
```

---
##### Other Git Tips
[Customizing Git - Git Configuration](https://git-scm.com/book/it/v2/Customizing-Git-Git-Configuration)

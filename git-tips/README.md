
---
# Table of Contents
* [Intro](#intro)
* [Git-files Download](#git-files-download)
* [Git Tips](#git-tips)
    * [How to add author info](#how-to-add-author-info)
    * [How to skip SSL certificate validation check](#how-to-skip-ssl-certificate-validation-check)
    * [How to setup a credential-helper for git](#how-to-setup-a-credential-helper-for-git)

---
## Intro
This repository will contain all the fundamental tips for git.

---
## Git-files Download
* Git Clients (GUI)
    * [GitEye](http://www.collab.net/downloads/giteye)
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
##### How to add author info

```sh
$ git config --global user.email martysama0134@example.com
$ git config --global user.name martysama0134
```

---
##### How to skip SSL certificate validation check

```sh
$ git config --global http.sslVerify false
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

The "Wall of Pink" is the dreadful commit where it removes and re-add all the lines of a file. It usually happens due to a diff misconcept of handling cr/lf EOL characters.

Since variables like `core.autocrlf` and `core.eol` are pointless "today", because, on mixed repositories, we usually have (e.g.) _.bat_ files with **crlf**, and _.sh_ files with **lf**.

If we would set the same EOL for all the text files, we could invalidate many of them. So it's better disable such feature, and, if you really need, use the `.gitattributes` file to handle them.

```sh
$ git config --global core.autocrlf false
```

_Note: Different git clients than msysgit handle `core.autocrlf`, `core.eol`, and `.gitattributes` differently causing ambiguous walls of pink._

_Note2: More info here [topic1](http://stackoverflow.com/questions/3206843), [topic2](http://stackoverflow.com/questions/2333424)_

---
##### How to prevent the Wall of Pink issue - Part 2 - Specify your own EOL conversion

As previously said, we can use the `.gitattibutes` file to handle the EOL character to use, some diff options, whether a file should be considered as text or binary, and so on.

If you've followed the "Part 1", you should have set `core.autocrlf` as **false**. We're to use `.gitattributes` to specify, in this case, which EOL directive use.

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

As you can imagine, .sh, .gitattributes, and .gitignore files EOL will be converted to LF, and .bat files to CRLF. The .rar files will be considered as binary.

The `text=auto` directive will auto-detect text files (it's a replacement of `core.autocrlf`), and use the `core.eol` parameter to convert the EOL.

---
##### How to rename a branch

* To rename the current branch:

  ```sh
  $ git branch -m <new_branch_name>
  ```
* To rename a specified branch:

  ```sh
  $ git branch -m <old_branch_name> <new_branch_name>
  ```
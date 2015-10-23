
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

##### How to add author info

```sh
$ git config --global user.email Example@google.com
$ git config --global user.name Example
```

##### How to skip SSL certificate validation check

```sh
$ git config --global http.sslVerify false
```

##### How to setup a credential-helper for git

* Temporary (cache)
    * On OSX

      ```sh
      $ git config --global credential.helper osxkeychain
      ```

    * On Linux/BSD

      By default (timeout specified for 15 minutes)
      ```sh
      $ git config --global credential.helper cache
      ```

      Custom (timeout specified for an hour)
      ```sh
      $ git config --global credential.helper "cache --timeout=3600"
      ```

    * On Windows

      ```sh
      $ git config --global credential.helper wincred
      ```

* Permanent (file)
  ```sh
  $ git config --global credential.helper wincred
  ```

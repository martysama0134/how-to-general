# How to automatically open a list of folders

Create a `.bat` file called like `defdirs.bat` containing:

```sh
@echo off
timeout 1
start "" "%userprofile%\Music"
timeout 1
start "" "C:\Python27"
```

_Note: a `timeout` is required if you want to open them in a specific order and not randomly_

_Note2: in this case, it will open 2 folders specified in the 2nd argument of the `start` instruction_

...And then run the newly created `defdirs.bat`!

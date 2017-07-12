# How to mount a virtual drive letter from a folder
For example of a drive letter, I'll use `D:`!
For example of a folder, I'll use `C:\D\`!

If you don't have the possibility to create a `D:` partition, you can easily solve it by mounting any kind of folder as a virtual `D:` partition!

For example, you could use a folder like `C:\D\` as `D:`!

All you need to do is to create these two `.bat` files and add them **inside** `C:\D\`!

* `mount_d.bat`

	```sh
	@cd %~dp0
	@subst d: .
	@pause
	```

	Run this file if you want to mount the current folder as `D:`

* `unmount_d.bat`

	```sh
	@subst d: /D
	@pause
	```

	Run this file if you want to unmount the virtualized `D:`

_Note: if you want to see the virtual mounted `D:` folder in the admin-runned programs (e.g. PS/3dsMax), run the `mount_d.bat` as admin as well! (twice: one normal, one as admin)_

_Note2: if you have another component like a CD/DVD burner as `D:`, you can simply switch it very easily in few seconds! (Google it)[https://www.google.it/search?q=How+to+Change+a+Drive+Letter]_

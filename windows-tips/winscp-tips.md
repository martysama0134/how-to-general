WinSCP has some settings you can change to improve its usage:

1. All the transferred files will be sent compressed

	Advanced... -> SSH -> [v] SSH Compression [ClickMe](https://i.imgur.com/7fpnEmC.png)

	e.g. 1mb of .txt is received/sent as if it were 100kb

	e.g. 60mb of binary is received/sent as if it were 5-6mb

2. Enable the Hidden files (all the files starting with . in the name, such as .git, .gitignore, .ssh)

	Preferences (Ctrl+Alt+P) -> Panels -> [v] Show hidden files (Ctrl+Alt+H) [ClickMe](https://i.imgur.com/55JHx7h.png)

3. How to automatically open archives

	,, -> Editors -> Add... [ClickMe](https://i.imgur.com/tAI3MAc.png)

	(o) Associated application

	`*.tgz; *.gz; *.bz2; *.zip; *.rar`

	Ok

	Up Up Up

4. How to open everything else with notepad ++

	,, -> Editors -> Add... [ClickMe](https://i.imgur.com/DxRFLLQ.png)

	External editor -> Browser... -> `"C:\Program Files (x86)\Notepad++\notepad++.exe" !.!`

	Ok

	Up Up

5. To disable auto crlf and timestamp update (they were very annoying) [ClickMe](https://i.imgur.com/1zeQPbH.png)

	,, -> Transfer -> Default -> Edit...

	Transfer mode -> (o) Binary (archives, doc, ...)

	Common options -> [_] Preserve timestamp
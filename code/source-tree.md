# SourceTree

**[Download SourceTree at Atlassian][1]**

## SourceTree user config as a git repo

Over time I started to have problems keeping the bookmakes in sync between two pc's, so had this idea that it would be cool if the configuration by itself was under git control.   

Luckily [SourceTree][1] stores the configurations as files. Usual path on Windows 8

	%USERPROFILE%\AppData\Local\Atlassian\SourceTree

There is some file in there that should not be under source control so I made the following ```.gitignore```

	cache/
	LastFailedPatch.txt
	*.log
	gitflow_local/
	opentabs.xml
	passwd
	userhosts
	sourcetree.license

There are a few problems doing this, one is the [bookmarks.xml](%USERPROFILE%\AppData\Local\Atlassian\SourceTree\bookmarks.xml) stores information about expanded notes so will update often. Also this file is stored when you exit SourceTree so will not see the changes until next time the program is started.

*Would be cool if it was a build in feature...*

## Custom Actions

### Open Solution

Custom Action

	Menu caption:  Open Solution
	Script to run: <path>\OpenSolution.bat
	Parameters:    $REPO

OpenSolution.bat

	@echo off
	
	for %%a in (%1\*.sln) do (
	set slnfile=%%a
	goto FOUNDIT
	)
	
	echo Did not find any solution files
	goto :EOF
	
	:FOUNDIT
	start %slnfile%
	
	:EOF

### Edit with Notepad++

Custom Action

	Menu caption:  Edit with Notepad++
	Script to run: C:\Program Files (x86)\Notepad++\notepad++.exe
	Parameters:    $FILE

### Build Recipe

Custom Action

	Menu caption:  Build Recipe
	Script to run: <path>\BuildRecipe.bat
	Parameters:    $REPO

	Menu caption:  Build Recipe and show log
	Script to run: <path>\BuildRecipe.bat
	Parameters:    $REPO ShowLog


BuildRecipe.bat

	@Title Visual Studio 2012 Command Prompt
	@call "%VS110COMNTOOLS%\vsvars32.bat"
	@prompt "$P"$_$G
	@echo off
	
	set texteditor="%ProgramFiles(x86)%\Notepad++\notepad++.exe"
	if not exist {%texteditor%} (
	set texteditor="%SystemRoot%\system32\notepad.exe"
	)
	for %%a in (%1\*.recipe) do (
	set recipe=%%a
	goto FOUNDIT
	)
	
	echo Did not find any recipe files
	goto :DONE
	
	:FOUNDIT
	MSBuild %recipe% /t:Clean;Prepare;Build;Pack /v:m /l:FileLogger,Microsoft.Build.Engine;logfile=build.log
	IF ERRORLEVEL 1 GOTO ERROR
	IF '%2'=='ShowLog' (%texteditor% build.log)
	goto DONE
	
	:ERROR
	echo -= ERROR =-
	%texteditor% build.log
	exit /b 1
	
	:DONE

### Visual Studio Command Prompt

Custom Action

	Menu caption:  Visual Studio 2012 Command Prompt
	Script to run: <path>\VSPrompt.bat
	Parameters:    $REPO

VSPrompt.bat

	start "Visual Studio 2012 Command Prompt" %comspec% /k "<path>\PromptVS110.bat" %1

PromptVS110.bat

	@Title Visual Studio 2012 Command Prompt - %~n1
	@call "%VS110COMNTOOLS%\vsvars32.bat"
	@cd /d %1
	@prompt "$P"$_$G

[1]: http://www.sourcetreeapp.com/   
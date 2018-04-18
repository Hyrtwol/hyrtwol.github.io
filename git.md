# Git Stuff

## Links

* [SourceTree](code/source-tree.md)
* [Git Tutorial][1]
* [Git Workflows][2]
* [Git Web Access](http://gitweb.codeplex.com/)
* [git-dot-aspx](https://github.com/linquize/git-dot-aspx)
* [BASH Programming](http://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO.html#toc9)
* [SSH Keys](https://wiki.archlinux.org/index.php/SSH_Keys)

[1]: http://www.vogella.com/articles/Git/article.html
[2]: https://www.atlassian.com/git/workflows

## Commands

### Reset
	git reset --hard

### Clean
	git clean -xdf

### Create bare git repo

	mkdir test_repo.git
	cd test_repo.git
	git --bare init

### Clone including submodules

	git clone --recursive <url>

### Clone from subversion

	git svn clone <url>

### Force a push

	git push origin master --force

### Commit with a comment

	git commit -m 'changes to hello file'

### Delete all tags in the local repository

	git tag -d `git tag | grep -E '.'`

### Delete all tags on remote

	git ls-remote --tags origin | awk '/^(.*)(\s+)(.*[a-zA-Z0-9])$/ {print ":" $2}' | xargs git push origin

## BASH

Script for creating a bare git repo

	#!/bin/sh
	if [ -z "$1" ]; then 
		echo usage: $0 git-repo-name
		exit
	fi
	REPONAME=$1
	echo Creating bare git repo $REPONAME.git
	mkdir $REPONAME.git
	cd $REPONAME.git
	git --bare init

You can execute bash scripts remotely via ssh:

	ssh <host> 'bash -s' < <name-of-the-script>

This only works well by using ssh-keys for authentication.

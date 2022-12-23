main: upload

upload: conversion
	rsync -Pr . people.debian.org:~/public_html/ --exclude=.git
	$(MAKE) delete

conversion:
	pandoc -f markdown -t html optimization.md > optimization.html

delete:
	$(RM) optimization.html

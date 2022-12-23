main: upload

upload: conversion
	rsync -Pr . people.debian.org:~/public_html/ --exclude=.git
	$(MAKE) delete

conversion:
	-$(RM) -rf blob
	pandoc -f markdown -t html optimization.md > optimization.html
	mkdir -p blob/main/blog
	cp -v ../blog/*.md blob/main/blog/
	for I in $$(find blob/main/blog -name '*.md'); do \
		pandoc -f markdown -t html $$I > $$I.html; done
	rename 's/\.md\.html$$/.html/' blob/main/blog/*.md.html

delete:
	$(RM) optimization.html

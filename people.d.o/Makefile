main: upload

upload: resume conversion index
	rsync -Pr . people.debian.org:~/public_html/ --exclude=.git
	$(MAKE) delete

.PHONY: index
index:
	echo 1. the index.html is rendered version of README.md by github
	echo 2. note, replace cdluminate/cdluminate with ~lumin from the rendered result
	echo 3. and replace *.md with *.html

blob-clean:
	-$(RM) -rf blob

conversion: blob-clean
	pandoc -f markdown -t html optimization.md > optimization.html
	mkdir -p blob/main/blog
	cp -v ../blog/*.md blob/main/blog/
	for I in $$(find blob/main/blog -name '*.md'); do \
		pandoc -f markdown -t html $$I > $$I.html; done
	rename 's/\.md\.html$$/.html/' blob/main/blog/*.md.html

resume: blob-clean
	$(MAKE) -C ../assets quiet
	mkdir -p blob/main/assets
	cp -v ../assets/cv.pdf blob/main/assets/

delete:
	$(RM) optimization.html

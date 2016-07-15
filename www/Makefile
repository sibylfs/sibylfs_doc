GITBOOK?=gitbook
SHELL:=bash

# generate html in _book; use a web browser on _book/index.html to
# view (sometimes this doesn't work so well; then use `gitbook serve`;
# this has the advantage that pages are automatically refreshed when
# the markdown is edited)
all:
	$(MAKE) clean # clean out old version
	$(GITBOOK) build

clean:
	rm -rf _book
	rm -f book.pdf

pdf:
	$(GITBOOK) pdf

-include Makefile.local

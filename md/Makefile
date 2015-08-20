MDS=meta.md intro.md bugs_found.md testing.md thomas_permissions.md spec_uses.md references.md 

fs_doc.html: FORCE
	pandoc --self-contained --css=fs.css --toc $(MDS) >$@

FORCE:

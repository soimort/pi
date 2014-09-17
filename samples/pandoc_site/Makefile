
SRCPATH		= .
OUTDIR		= _site
EXT		= md
PANDOC		= pandoc
PANDOC_OPTIONS	= -s --mathml
TEMPLATE	= _templates/main.html
IGNORE_EXTS	= Makefile Guardfile

ENV		= env SRCPATH='$(SRCPATH)' OUTDIR='$(OUTDIR)' EXT='$(EXT)' PANDOC='$(PANDOC)' PANDOC_OPTIONS='$(PANDOC_OPTIONS)' TEMPLATE='$(TEMPLATE)' IGNORE_EXTS='$(IGNORE_EXTS)' bash -c

.PHONY: default clean copy html all server watch
default: all

clean:
	@rm -rf _site/

# Usage:
#   make FILE=css/theme.css copy
# Limitations: only relative file names are allowed
copy:
	@$(ENV) 'if [ -z "$$FILE" ];then echo Please specify a FILE.;exit;fi;f=./$$FILE;\
	mkdir -p $$SRCPATH/$$OUTDIR/$${f%/*};cp $$f $$SRCPATH/$$OUTDIR/$$f'

# Usage:
#   make FILE=README.md html
# Limitations: only relative file names are allowed
html:
	@$(ENV) 'if [ -z "$$FILE" ];then echo Please specify a FILE.;exit;fi;f=./$$FILE;\
	mkdir -p $$SRCPATH/$$OUTDIR/$${f%/*.$$EXT};$$PANDOC $$PANDOC_OPTIONS --template $$TEMPLATE $$f -o $$SRCPATH/$$OUTDIR/$${f%.$$EXT}.html'

all:
	@$(ENV)	'if [ -z "$$SRCPATH" ];then SRCPATH=.;fi;if [ -z "$$OUTDIR" ];then OUTDIR=_site;fi;if [ -z "$$EXT" ];then EXT=md;fi;if [ -z "$$PANDOC" ];then PANDOC=pandoc;fi;a=();while IFS= read -d $$'\''\0'\'' -r f ;do a=("$${a[@]}" "$$f");done < <(find $$SRCPATH -print0);for f in $${a[@]};do if [ -f "$$f" -a -n "$${f##./_*}" ];then if [ -z "$${f%%*.$$EXT}" ];then mkdir -p $$SRCPATH/$$OUTDIR/$${f%/*.$$EXT};$$PANDOC $$PANDOC_OPTIONS --template $$TEMPLATE $$f -o $$SRCPATH/$$OUTDIR/$${f%.$$EXT}.html;else mkdir -p $$SRCPATH/$$OUTDIR/$${f%/*};cp $$f $$SRCPATH/$$OUTDIR/$$f;fi;fi;done;for f in $${IGNORE_EXTS[@]};do find $$SRCPATH/$$OUTDIR -name \*$$f -delete;done'

server: all
	@ruby -run -e httpd $(SRCPATH)/$(OUTDIR) -p 8000

watch: all
	@guard start -i &
	@ruby -run -e httpd $(SRCPATH)/$(OUTDIR) -p 8000 2>/dev/null

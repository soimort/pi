
SRCPATH		= .
OUTDIR		= _site
EXT		= md
ENGINE		=
ENGINE_OPTIONS	=
HEAD		= _templates/main.head
INCLUDE_BEFORE	= _templates/main.include-before
INCLUDE_AFTER	= _templates/main.include-after
IGNORE_EXTS	= Makefile Guardfile

ENV		= env SRCPATH='$(SRCPATH)' OUTDIR='$(OUTDIR)' EXT='$(EXT)' ENGINE='$(ENGINE)' ENGINE_OPTIONS='$(ENGINE_OPTIONS)' HEAD='$(HEAD)' INCLUDE_BEFORE='$(INCLUDE_BEFORE)' INCLUDE_AFTER='$(INCLUDE_AFTER)' IGNORE_EXTS='$(IGNORE_EXTS)' bash -c

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
	mkdir -p $$SRCPATH/$$OUTDIR/$${f%/*.$$EXT};d=$$SRCPATH/$$OUTDIR/$${f%.$$EXT}.html;echo -e "<!DOCTYPE html>\n<html>\n<head>" >$$d;cat $$HEAD $${f%.$$EXT}.head 2>/dev/null >>$$d;echo -e "</head>\n<body>" >>$$d;cat $$INCLUDE_BEFORE $${f%.$$EXT}.include-before 2>/dev/null >>$$d;$$ENGINE $$ENGINE_OPTIONS $$f >>$$d;cat $${f%.$$EXT}.include-after $$INCLUDE_AFTER 2>/dev/null >>$$d;echo -e "</body>\n</html>" >>$$d'

all:
	@$(ENV)	'if [ -z "$$SRCPATH" ];then SRCPATH=.;fi;if [ -z "$$OUTDIR" ];then OUTDIR=_site;fi;if [ -z "$$EXT" ];then EXT=md;fi;if [ -z "$$ENGINE" ];then ENGINE=stmd;fi;a=();while IFS= read -d $$'\''\0'\'' -r f ;do a=("$${a[@]}" "$$f");done < <(find $$SRCPATH -print0);for f in $${a[@]};do if [ -f "$$f" -a -n "$${f##./_*}" ];then if [ -z "$${f%%*.$$EXT}" ];then mkdir -p $$SRCPATH/$$OUTDIR/$${f%/*.$$EXT};d=$$SRCPATH/$$OUTDIR/$${f%.$$EXT}.html;echo -e "<!DOCTYPE html>\n<html>\n<head>" >$$d;cat $$HEAD $${f%.$$EXT}.head 2>/dev/null >>$$d;echo -e "</head>\n<body>" >>$$d;cat $$INCLUDE_BEFORE $${f%.$$EXT}.include-before 2>/dev/null >>$$d;$$ENGINE $$ENGINE_OPTIONS $$f >>$$d;cat $${f%.$$EXT}.include-after $$INCLUDE_AFTER 2>/dev/null >>$$d;echo -e "</body>\n</html>" >>$$d;else mkdir -p $$SRCPATH/$$OUTDIR/$${f%/*};cp $$f $$SRCPATH/$$OUTDIR/$$f;fi;fi;done;IGNORE_EXTS=("$${IGNORE_EXTS[@]}" .head .include-before .include-after);for f in $${IGNORE_EXTS[@]};do find $$SRCPATH/$$OUTDIR -name \*$$f -delete;done'

server: all
	@ruby -run -e httpd $(SRCPATH)/$(OUTDIR) -p 8000

watch: all
	@guard start -i &
	@ruby -run -e httpd $(SRCPATH)/$(OUTDIR) -p 8000 2>/dev/null

all:
	rst2html.py cc.rst > cc.html

sync: all git

git:
	git pull origin master
	git commit -a -m newstuff
	git push origin master

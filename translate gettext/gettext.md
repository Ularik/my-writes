1. pip install python-gettext       # install
2. mkdir myapp                           # create folder
3. in folder : 
	1. locale:
		1. en:
			1. LC_MESSAGES:
				1. myapp.po
		2. ru:
			1. LC_MESSAGES:
				2. myapp.po
	2. main.py
	3. msgfmt.py
	4. pygettext.py
4. print code in main.py:
		import gettext
		ru = gettext.translation('myapp', localedir='locale', languages=['ru'])
		ru.install()
		
		_ = ru.gettext
		def print_some_strings():
		    print(_("Hello world"))
		    print(_("This is a translatable string"))
		if __name__=='__main__':
		    print_some_strings()
## How to create file.po?
- python pygettext.py myapp.py                    # create
- python msgfmt.py /locale/ru/LC_MESSAGES/myapp.po

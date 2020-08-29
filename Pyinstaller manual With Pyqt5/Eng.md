Pyinstaller manual With Pyqt5</br>
=========
## Test Environment</br>
##### Windows 10 with Anaconda3 + Python3 (In Anaconda Prompt)
***

## 1. Code Addition
Addition of the code on your previous code</br>
```python
def resource_path(relative_path):
    """ Get absolute path to resource, works for dev and for PyInstaller """
    try:
        # PyInstaller creates a temp folder and stores path in _MEIPASS
        base_path = sys._MEIPASS
    except Exception:
        base_path = os.path.abspath(".")
    return os.path.join(base_path, relative_path)
    
form = resource_path('Main.ui')
form_class = uic.loadUiType(form)[0]
```

> Example('...' is omission(The previous code))</br>
```python
...
import os

def resource_path(relative_path):
    """ Get absolute path to resource, works for dev and for PyInstaller """
    try:
        # PyInstaller creates a temp folder and stores path in _MEIPASS
        base_path = sys._MEIPASS
    except Exception:
        base_path = os.path.abspath(".")
    return os.path.join(base_path, relative_path)

form = resource_path('Main.ui')
form_class = uic.loadUiType(form)[0]


class WindowClass(QMainWindow, form_class):
...
```

***

## 2. Create your 'spec' file using options you want

<code>
pyinstaller --onedir --onefile --noconsole --clean --hidden-import=PyQt5-sip -p [dll path]
</code></br>

### Important!) You can using the options if you want. If you didn`t use '[Essential]' options, it causes an error

> [Essential] '--hidden-import=PyQt5-sip' : This option will import PyQt5-sip module.</br>
> [Not essential] '--onedir' : This option will make exe file to one path.</br>
> [Not essential] '--onefile' : This option will make exe file to one file</br>
> [Not essential] '--noconsole' : This option will not show the console.</br>
> [Recommend] '--clean' : This option will delete the temp files and cashs before build.</br>
> [Recommend] '-p [dll path]' : This option allows exe file to be used on non-Python environment.</br>
> > What is '[dll path]'?</br>
> > <code>C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Remote Debugger\x64</code> or <code>C:\Program Files (x86)\Windows Kits\10\Redist\ucrt\DLLs\x64</code></br>
> > Copy and paste all dll files in the '[dll path]'</br>

***

## 2. Revise spec file
When run step 2 command, [pyname].spec file will be created.</br>
Open the file with Notepad or Notepad++.</br>
Revise 'datas' part in the file.
> Before</br>
> <code> datas=[], </code></br>
> After</br>
> <code> datas=[ ('[ui file name].ui', '.') ], </code></br>
> Example('...' is omission(The previous code))</br>
```python
...
a = Analysis(['C:\\...'],
             pathex=['[dll path]', 'C:\\...'],
             binaries=[],</code></br>
             datas=[ ('Main.ui', '.') ],
             hiddenimports=['PyQt5-sip'],
             hookspath=[],
             runtime_hooks=[],
...</br></br>

```
> > + Addional : If img file was added on your ui file, you should add it on datas too.</br>
> > Example 2 ('...' is omission(The previous code))</br>
```python
...
a = Analysis(['C:\\...'],
             pathex=['[dll path]', 'C:\\...'],
             binaries=[],</code></br>
             datas=[ ('Main.ui', '.'), ('Test.png', '.') ],
             hiddenimports=['PyQt5-sip'],
             hookspath=[],
             runtime_hooks=[],
...
```


***

## 3. Creating exe file using revised spec
<code> pyinstaller --onefile main.spec </code></br>
> [Recommend] '--onefile' : This option will make exe file to one file</br>

***

## 4. End!

***

## Note : Sources</br>
Bundling data files with PyInstaller (--onefile) : [[Go](https://stackoverflow.com/questions/7674790/bundling-data-files-with-pyinstaller-onefile)]</br>
'-p [dll path]' part : [[Go](https://tina0430.tistory.com/34)]</br>


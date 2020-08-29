Pyinstaller manual With Pyqt5</br> : Pyqt5를 이용한 프로젝트에 Pyinstaller 사용법
=========
## 테스트 환경</br>
##### Windows 10 with Anaconda3 + Python3 (In Anaconda Prompt)
***

## 1. 코드 추가
기존에 프로그래밍 했던 코드 상단에 아래 코드를 추가한다.</br>
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

> 예시 ('...'은 생략입니다.)</br>
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

## 2. 원하는 옵션을 이용해 spec 파일 생성

<code>
pyinstaller --onedir --onefile --noconsole --clean --hidden-import=PyQt5-sip -p [dll경로(dll path)]
</code></br>

### 중요!) 사용자가 원하는 대로 옵션을 붙여 사용하면 되지만, '[필수]'는 넣지 않으면 오류가 남.</br>

> 한국어</br>
> [필수] '--hidden-import=PyQt5-sip' : PyQt5-sip 모듈을 추가해줌.</br>
> [선택] '--onedir' : 실행파일을 포함해 1개의 디렉토리(폴더)로 만듦</br>
> [선택] '--onefiler' : 1개의 파일로 만든다.</br>
> [선택] '--noconsole' : 콘솔(검은색 창)을 띄우지 않음.</br>
> [권장] '--clean' : 빌드 전 모든 캐시, 임시파일 제거 </br>
> [권장] '-p [dll경로(dll path)]' : Python이 없는 환경에서도 사용할 수 있도록 함.</br>
> > [dll경로(dll path)]'에는</br>
> > <code>C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Remote Debugger\x64</code> 또는 <code>C:\Program Files (x86)\Windows Kits\10\Redist\ucrt\DLLs\x64</code></br>
> > 둘중 한 경로에 있는 dll을 모두 복사해서 붙여넣는다.</br>

***

## 2. spec 파일 수정
위 명령을 실행하고 나면 [pyname].spec파일이 만들어져 있다.</br>
해당 파일을 메모장이나 Notepad++로 연다.</br>
해당 파일에서 datas부분을 수정한다.</br>
> 기존</br>
> <code> datas=[], </code></br>
> 변경 이후</br>
> <code> datas=[ ('[ui파일 이름].ui', '.') ], </code></br>
> > 예시('...'은 생략을 뜻함)</br>
```python
...
a = Analysis(['C:\\...'],
             pathex=['[dll경로]', 'C:\\...'],
             binaries=[],</code></br>
             datas=[ ('Main.ui', '.') ],
             hiddenimports=['PyQt5-sip'],
             hookspath=[],
             runtime_hooks=[],
...</br></br>

```
> > + 추가 : 만약 이미지 파일등이 ui에 추가되었다면, datas부분에도 추가되어야함.</br>
> > 예시2('...'은 생략을 뜻함)</br>
```python
...
a = Analysis(['C:\\...'],
             pathex=['[dll경로]', 'C:\\...'],
             binaries=[],</code></br>
             datas=[ ('Main.ui', '.'), ('Test.png', '.') ],
             hiddenimports=['PyQt5-sip'],
             hookspath=[],
             runtime_hooks=[],
...
```


***

## 3. 수정된 spec를 이용해 exe파일 생성
> [선택] '--onefiler' : 1개의 파일로 만든다.</br>

***

## 4. 끝! (End!)

***

## 출처 표기(Note : Sources)</br>
Bundling data files with PyInstaller (--onefile) : [[바로가기](https://stackoverflow.com/questions/7674790/bundling-data-files-with-pyinstaller-onefile)]</br>
Win10 + PyQt5 + pyinstaller + Python3.5 조합으로 exe 파일 만들 때 오류 : [[바로가기](https://tina0430.tistory.com/34)]</br>


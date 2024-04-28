## 동적 실행

파이썬에서 파일을 동적으로 추가하고 실행하는 방법에는 여러 가지가 있습니다. 사용할 수 있는 주요 기법은 다음과 같습니다:

1. **`exec()` 함수 사용**: 이 방법을 통해 문자열 형태로 된 파이썬 코드를 실행할 수 있습니다. 파일로부터 코드를 읽은 후 `exec()`를 사용하여 실행할 수 있습니다.

2. **`importlib` 모듈 사용**: Python의 `importlib` 모듈을 사용하면 프로그램 실행 중에 모듈을 동적으로 로드하고 실행할 수 있습니다. 이는 특히 모듈 이름이나 경로가 동적으로 결정되어야 할 때 유용합니다.

3. **스크립트 파일 실행**: `subprocess` 모듈을 사용하여 외부 파이썬 스크립트 파일을 직접 실행할 수 있습니다. 이 방법은 독립된 프로세스에서 스크립트를 실행하므로 메인 프로그램과의 격리가 필요할 때 유용합니다.

### 예제 코드

#### 1. `exec()` 함수 사용
```python
# 파일을 읽고 코드를 실행합니다.
filename = 'example.py'
with open(filename, 'r') as file:
    code = file.read()
    exec(code)
```

#### 2. `importlib` 모듈 사용
```python
import importlib.util
import sys

def load_and_run_module(filepath, module_name):
    spec = importlib.util.spec_from_file_location(module_name, filepath)
    module = importlib.util.module_from_spec(spec)
    spec.loader.exec_module(module)
    return module

# 모듈을 로드하고 사용합니다.
module = load_and_run_module('path/to/your/module.py', 'dynamic_module')
module.some_function()  # 모듈 내 함수 실행
```

#### 3. `subprocess` 모듈 사용
```python
import subprocess

# 외부 스크립트를 새 프로세스에서 실행합니다.
subprocess.run(['python', 'script.py'])
```

이러한 방법들을 사용하여 파이썬에서 동적으로 파일을 추가하고 실행할 수 있습니다. 사용하려는 방법은 파일의 종류, 실행 환경, 그리고 프로그램의 요구 사항에 따라 달라질 수 있습니다.

## 수동 추가

pip 없이 파이썬 라이브러리를 수동으로 추가하는 방법은 몇 가지가 있습니다. 이 방법들은 라이브러리 소스 코드에 직접 접근할 수 있을 때 유용합니다. 여기 몇 가지 일반적인 접근 방법을 설명하겠습니다:

### 1. 직접 설치
라이브러리의 소스 코드를 다운로드한 후, 해당 디렉토리로 이동하여 설치 스크립트(보통 `setup.py`)를 실행합니다. 이 방법은 라이브러리가 파이썬의 표준 `distutils` 또는 `setuptools`를 사용하는 경우에 적합합니다.

```bash
# 라이브러리 소스 코드를 다운로드합니다 (예: GitHub에서)
cd path/to/the/library
python setup.py install
```

### 2. PYTHONPATH 환경 변수 사용
라이브러리 폴더를 시스템의 `PYTHONPATH` 환경 변수에 추가하면, 파이썬이 프로그램을 실행할 때 해당 경로를 참조하여 모듈을 찾습니다. 이는 설치 과정 없이 라이브러리를 사용하고자 할 때 유용합니다.

```bash
# Bash (Linux/Mac)의 경우
export PYTHONPATH="$PYTHONPATH:/path/to/your/library"

# CMD (Windows)의 경우
set PYTHONPATH=%PYTHONPATH%;C:\path\to\your\library
```

### 3. sys.path에 직접 추가
파이썬 코드 내에서 직접 라이브러리 경로를 `sys.path` 리스트에 추가하는 방법입니다. 이는 프로그램 내에서만 임시적으로 라이브러리 경로를 추가할 때 사용됩니다.

```python
import sys
sys.path.append('/path/to/your/library')

import your_module  # 이제 your_module을 import 할 수 있습니다
```

### 4. 가상 환경에 로컬 라이브러리 설치
가상 환경을 사용하고 있을 경우, 로컬 라이브러리를 해당 환경에 직접 설치할 수 있습니다. 이 방법도 `setup.py`가 필요합니다.

```bash
# 가상 환경 활성화
source /path/to/venv/bin/activate  # Linux/Mac
\path\to\venv\Scripts\activate     # Windows

# 로컬 라이브러리 설치
cd path/to/the/library
python setup.py install
```

이러한 방법들은 pip을 사용할 수 없거나 원하지 않을 때, 특히 개발 중인 라이브러리를 테스트하거나 수정할 때 유용합니다. 항상 라이브러리의 종속성을 확인하고, 필요한 경우 이를 수동으로 관리해야 할 수도 있습니다.

## pyinstaller

PyInstaller는 파이썬 어플리케이션을 스탠드얼론 실행 파일로 패키징하는 도구입니다. 이는 코드를 한 파일로 묶어 별도의 파이썬 설치 없이도 실행할 수 있게 해 줍니다. 윈도우, 맥, 리눅스 등 다양한 운영 체제에서 사용할 수 있습니다. PyInstaller는 필요한 모든 파일과 라이브러리를 하나의 실행 가능한 파일 또는 디렉토리로 패키징하여, 응용 프로그램을 쉽게 배포할 수 있게 해 줍니다.

### PyInstaller 설치
PyInstaller는 pip를 통해 쉽게 설치할 수 있습니다:
```bash
pip install pyinstaller
```

### PyInstaller 사용 방법
1. **단일 파일 생성**: 이 옵션은 모든 필요한 라이브러리, 의존성 파일, 파이썬 인터프리터를 단일 실행 파일로 패키징합니다.
    ```bash
    pyinstaller --onefile your_script.py
    ```
    생성된 실행 파일은 `dist` 폴더 내에서 찾을 수 있습니다.

2. **폴더로 패키징**: 이 옵션은 필요한 모든 파일을 한 폴더에 패키징합니다. 실행에 필요한 모든 파일이 포함되어 있으므로, 이 폴더를 전체적으로 배포할 수 있습니다.
    ```bash
    pyinstaller --onedir your_script.py
    ```
    이 경우에도 생성된 파일들은 `dist` 폴더 내에 위치합니다.

### PyInstaller의 주요 옵션
- `--windowed` 또는 `-w`: 콘솔 창 없이 GUI 어플리케이션으로 패키징합니다.
- `--add-data`: 추가 데이터 파일을 패키징할 때 사용합니다. 사용법은 운영체제에 따라 다릅니다.
- `--icon`: 실행 파일에 아이콘을 추가할 때 사용합니다.
- `--name`: 패키지의 결과물 이름을 지정할 때 사용합니다.

### 예제
간단한 스크립트 `app.py`를 단일 실행 파일로 패키징하는 예:
```bash
pyinstaller --onefile --windowed --icon=app.ico app.py
```

### 주의 사항
- PyInstaller로 패키징된 응용 프로그램은 스크립트가 작성된 같은 운영 체제에서만 실행됩니다. 즉, 윈도우에서 패키징된 실행 파일은 윈도우에서만, 리눅스에서는 리눅스에서만 실행 가능합니다.
- 복잡한 의존성이 있는 경우, PyInstaller가 모든 필요한 파일을 포함하지 않을 수 있으므로 테스트를 철저히 수행해야 합니다.
- 보안 소프트웨어는 때때로 PyInstaller로 만들어진 실행 파일을 오진할 수 있습니다.

PyInstaller는 파이썬 프로젝트를 간단하고 효율적으로 배포할 수 있는 강력한 도구입니다. 개발 중인 프로젝트가 있다면 이 도구를 사용하여 쉽게 배포해 볼 수 있습니다.

# Python package 빌드 매뉴얼
파이썬으로 구현한 기능을 패키지화 할 때 참고하기 위한 매뉴얼

## 사전준비
1. 패키지화 할 기능들을 만들어둔다.
2. Reference의 파이썬 패키지 빌드 매뉴얼을 참고해 pyproject.toml을 작성한다.
3. `python3 -m pip install --upgrade build` 빌드를 위해 빌드 패키지를 업데이트 하고 설치한다.

## pyproject.toml 작성
1. Basic
   *  같은 폴더 내 `pyproject.toml`이 기본 양식이라고 할 수 있다.
2. Advanced
   *  `pyproject_ad.toml`는 필요에 의해 추가된 양식이 존재하며 추가된 사항은 아래와 같다.
      *  `[project.optional-dependencies]` : dependencies는 기본으로 무조건 설치되어야 하는 항목이고 optional-dependencies는 상황에 따라 설치되는 의존성이 다른 경우가 존재할 때 사용한다. 
         * CPU의 아키텍쳐가 arm 인지 x86_64 인가에 따라 설치될 의존성을 다르게 했으며 x86_64일 경우 pip가 아닌 지정된 URL에서 직접 의존성을 다운로드 받게 처리했다.
         * 다이렉트 URL을 사용 하기 위해 `[tool.hatch.metadata]` 에 `allow-direct-references = true` 를 추가해주었다.
         * 이렇게 optional-dependencies를 설정했을 경우 패키지를 whl로 만들고 설치 시 패키지 이름 뒤에 `[설정한 optional 이름]`을 입력해 설치하면 해당하는 의존성 옵션으로 설치가 된다(=`pip install fastapi[all]`과 같은 맥락으로 이해하면 된다.)
3. Version
    * 패키지의 버전은 `[tool.hatch.version]` 에 입력된 것처럼 path로 지정된 .py 파일의 version 변수에 따라 지정된다.
    * 지정된 version 변수는 패키지 빌드 시 패키지 명에 표기된다.

## Build
pyproject.toml이 있는 경로에서 커맨드로 `python -m build` 라고 입력해주면 dist라는 폴더가 새로 생성되며 .whl과 .tar.gz 파일이 생성된다.
호환성 및 접근성을 .tar.gz와 .whl 파일을 모두 제공하지만 주로 .whl을 사용한다.

## Install
.whl 파일이 존재하는 경로에서 `pip install [파일명].whl` 을 커맨드로 입력하면 패키지가 설치된다. 이 프로젝트에서 pyproject_ad.toml 기준으로는 아래와 같다.  
* `pip install exam1-0.0.1-py3-none-any.whl` : 기본 의존성만 설치
* `pip install exam1-0.0.1-py3-none-any.whl[CPU]` : 기본 의존성 + CPU 의존성 설치
* `pip install exam1-0.0.1-py3-none-any.whl[GPU]` : 기본 의존성 + GPU 의존성 설치

## Reference
[https://packaging.python.org/en/latest/tutorials/packaging-projects/](https://packaging.python.org/en/latest/tutorials/packaging-projects/)
[hatchling](https://hatch.pypa.io/latest/)
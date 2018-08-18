# Chromium 시작하기
- python 설치하기
- depot_tools 다운로드 하기 depot_tools는 Chromium 관련 프로젝트 소스 받고 빌드하는데 사용되는 tool의 모임
~~~shell
git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
~~~
 - 다운로드 받은 depot_tools를 path 환경변수에 등록한다
 - pwd 입력해서 현재위치 찾은후 경로에 넣어준다
~~~shell
export PATH="$PATH:${HOME}/depot_tools"
~~~
- 원하는 위치에서 소스를 다운로드함
- fetch는 최초에만 가능하고 나머지부터는 gclient sync로 업데이트
~~~shell
fetch —no hooks chromium
~~~

# 빌드를 위한 추가 설치
설치한곳에서 src로 들어가서 명령어 쳐야함 
~~~shell
cd src
./build/install-build.deps.sh (리눅스일경우)
gclient runhooks
~~~

# 빌드
~~~shell
gn args out/Debug
~~~

옵션
~~~
enable_nacl = false
is_component_build = true
is_debug = true
symbol_level = 1
use_jumbo_build = true
~~~

빌드하기
~~~shell
ninja -C out/Debug chrome 
~~~

# 실행
Run
~~~ shell
./out/Debug/Chromium.app/Contents/MacOS/Chromium 
~~~


## 참고사이트
- [컨트리뷰톤 chromium](https://github.com/romandev/contributhon2018)  
- [Checking out and building Chromium for Mac](https://chromium.googlesource.com/chromium/src/+/master/docs/mac_build_instructions.md)

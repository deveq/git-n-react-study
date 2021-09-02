## 3.1 깃 저장소 생성

깃은 변경 사항을 전용 저장소(repository)에 저장한다.

### 3.1.1 폴더와 깃 저장소

|                            폴더                             |                           깃 저장소                           |
| :---------------------------------------------------------: | :-----------------------------------------------------------: |
| 파일시스템(하드디스크와 같은 장치)에 데이터를 저장하고 관리 | 숨겨진 영역에 버전 관리 시스템에 필요한 파일 변경 이력을 기록 |

둘의 **차이점**은 **숨겨진 영역의 여부**

### 3.1.2 초기화

- 저장소를 생성하려면 **초기화 작업**이 필요하다.
- 깃에서의 초기화는 이미 존재하는 폴더에 초기화 명령어로 VCS관리를 위한 **숨겨진 영역을 생성하는 작업**을 의미한다

##### 지정된 폴더에서 깃 배쉬 열기 : 원하는 폴더에서 마우스 오른쪽 버튼을 누른 후 Git Bash Here 메뉴를 선택

![git init](https://images.velog.io/images/jiseon96/post/3c5bdc89-4611-4866-b3c6-676160f50b9a/image.png)

##### 리포지터리(repository)와 깃 저장소는 같은 표현이다.

> $ git init 경로명 ------- 경로명을 입력하지 않을시 현재 폴더에서 초기화

### 3.1.3 숨겨진 폴더 = .git 폴더

> $ ls -a

![ls -a](https://images.velog.io/images/jiseon96/post/58f0af21-c8b8-4237-aa83-baee20e328af/image.png)

폴더의 이름 앞에 점(.)이 있으면 숨겨진 폴더를 의미한다.
.git 폴더에는 깃 저장소에 필요한 모든 뼈대 파일이 담겨 있다. (깃 초기화를 통하여 자동 생성된다.)

.git 폴더

- 깃으로 관리되는 모든 파일 및 브랜치 등 이력을 기록하는 매우 중요한 폴더이다.
- 깃 저장소를 통째로 복사하고자 할 때는 숨겨진 .git 폴더까지 복사해야 한다.
- .git 폴더를 삭제하거나 함게 복제하지 않으면 깃의 모든 이력은 없어지고 일반적인 폴더 파일과 동일해진다. -> 숨겨진 폴더 복사시 `$ cp -r 원본폴더 복사폴더` 로 입력해야함

---

## 3.2 워킹 디렉터리(= 워킹트리)

### 3.2.1 워킹 디렉터리란?

깃의 저장공간

- 작업을 하는 공간(working)
- 임시로 저장하는 공간(stage)
- 실제로 저장하여 기록하는 공간(repository)

워킹 디렉터리 :

- 로컬 저장소에 접근 할 수 있다.
- 실제로 파일을 생성하고 수정하는 공간(쉽게 말하면 파일을 저장하는 공간)
- 스테이지와 연결되어 있다.

### 3.2.2 파일의 untracked 상태와 tracked 상태

![](https://images.velog.io/images/jiseon96/post/7d4b89fe-5904-48b2-a4ec-6923f46298c6/image.png)
깃은 워킹 디렉터리에 있는 파일들을 '추적됨(tracked)'와 '추적되지 않음(untracked)'상태로 구분한다.

#### untracked 상태

- 워킹 디렉터리에 새로 생성된 파일은 모두 추적되지 않음 상태이다.
- untracked 상태에서는 파일을 관리 할 수 없다.

#### tracked 상태

- untracked 상태의 파일을 별도로 명령어를 실행하여 tracked 상태로 변경해주어야한다.
- 명령어는 `$ git add` 를 사용한다.

깃은 tracked 상태로 표시된 파일들만 추적 관리한다.
깃이 tracked와 untracked 개념을 사용하는 것은 시스템 부하를 줄이고, 좀더 효율적으로 파일 이력을 관리하기 위해서이다.

---

## 3.3 스테이지

스테이지는 '임시로 저장하는 공간'을 의미 한다.
워킹 디렉터리에서 제출된 tracked 파일들을 관리하고 커밋작업과 연관이 매우 많다.

### 3.3.1 스태이지 = 임시영역

- 스테이지는 워킹 디렉터리와 '실제로 저장하여 기록하는 공간' 사이에 있는 임시 영역이다.
- 깃은 워킹 디렉터리에서 작업이 끝난 파일을 스테이지로 잠시 복사 한다.
- 파일의 콘텐츠 내용을 직접 가지고 있지 않고 커밋하려는 파일의 추적 상태 정보들만 기록한다.

스테이지 영역을 별도로 운영하는 이유는 커밋을 빠르게 처리하기 위해서이다.
'실제로 저장하여 기록하는 공간'인 저장소는 스테이지 영역에서 가리키는 파일 내용을 기반으로 변경된 차이점만 기록된다.

파일들의 스테이지 상태는 status 명령어 또는 git ls-files로 확인가능

> $ git status
> $ git ls-files --stage

스테이지 영역에 등록된 파일들은 다시 stage 상태와 unstage상태로 구분된다.
버전관리에서 제외하고 싶은 파일이 있으면 .gitignore파일에 등록된다.

### 3.3.2 파일의 stage 상태와 unstage 상태

- 스테이지 영역으로 등록된 모든 파일은 untracked 상태에서 tracked 상태로 변경된다
- 스테이지는 워킹 디렉터리 안에 있는 파일 들의 추적 상태를 관리하는 역할을 수행한다.
- 깃이 변화 이력을 기록하려면 파일들의 최종상태가 stage 상태여야 한다.
- 스테이지 영역에 있는 파일과 워킹 디렉터리에 있는 파일 내용이 차이가 있으면 unstage상태가 된다.
- 아직 스테이지 영역으로 등록하지 않은 워킹 디렉터리 안의 파일도 unstage 상태라고 생각할 수 있다. **이때는 unstage 상태이자 동시에 untracked 상태**
- unstage 상태라고 해서 실제파일이 없어지는 것은 아니고 파일이 수정되어 임시적으로 스테이지 목록에서 제외 된 것이다.(git add 명령어를 사용하면 스테이지에 다시 추가할 수 있다.)

### 3.3.3 파일의 modified 상태와 unmodified 상태

![](https://images.velog.io/images/jiseon96/post/92385a33-8d12-4748-823f-c105ff2b87f5/image.png)
코드를 변경한다는 것은 워킹 디렉터리에서 파일을 수정하는 것을 의미한다.
파일이 수정되면 워킹 디렉터리와 스테이지 간 내용이 일치하지 않는다.
=> 수정한 파일과 원본파일을 구분하기 위해 수정함 상태와 수정하지 않음 상태로 표현

#### - modified 상태

1. tracked 상태인 파일이 수정되면 스테이지는 파일 상태를 modified 상태로 변경한다.
2. 수정된 파일은 스테이지에서 잠시 제외가 된다.
3. 깃은 수정 여부만 체크 하기 때문에 modified상태로 변경된 파일은 스테이지로 재등록해야 한다.
   ($git add 명령어로 재등록)

#### - unmodified 상태

unmodified 상태는 tracked 상태이면서 스테이지에서 한번도 수정하지 않은 원본 상태를 말한다.
스테이지에 등록한 후 어떤 수정도 하지 않았다면 unmodified 상태이다

![](https://images.velog.io/images/jiseon96/post/5877c2f5-3342-4c94-a7d0-a9e4e7535665/image.png)

---

## 3.4 파일의 상태 확인

### 3.4.1 status 명령어로 깃 상태 확인

status 명령어를 사용하면 깃의 상태 메시지를 확인할 수 있다.
커밋 명령을 수행하면 status명령어로 파일들의 변경된 상태를 확인 가능하다.

### 3.4.2 소스트리에서 깃 상태 확인

![](https://images.velog.io/images/jiseon96/post/268d4c89-472c-4fdc-8ed3-904790b37f3b/image.png)

> $ git add 파일명

---

## 3.5 파일 관리 목록에서 제외: .gitignore

- 저장소를 다른 사람들과 공유한다면 깃으로 관리하고 싶지 않은 파일과 폴더는 별도의 .gitignore 설정 파일 안에 나열해서 적어준다.

### 3.5.1 .gitignore 파일

- git + ignore(무시하다)의 합성어이다.
- 앞에 .이 있어 숨겨진 파일로 관리된다.
- 깃에서 관리하지 않는 파일들의 목록을 가지고 있고 깃 은 이파일에 작성된 목록들을 추적하지 않는다.
- 로컬 저장소를 서버로 전송하거나 다른사 람과 공유할 때도 이를 분리하여 처리한다.
- 파일 이름만 .gitignore로 만들어 주면 되고 저장소 폴더의 최상위 디렉터리에 두어야 한다.

### 3.5.2 .gitignore 파일 표기법

- 주석처리 : #으로 시작하는 줄은 주석으로 처리
- #없이 완전한 파일 이름을 적어주면 그 파일은 깃의 관리 대상에서 제외(경로명이 있다면 같이 입력)
  > #DB 접속 파일을 제외함
  > dbinfo.php
- 패턴 정의 : 애스터리스크('_') 기호를 사용. _ 기호는 모든 문자열을 대체할 수 있음. 이러한 문자를 셸글로빙이라고 하고 글로빙 문자를 사용하여 패턴을 확장
  > #오브잭트 파일은 제외함
  > \*.obj
- 제외하지 않는 파일과 필요한 파일관리 : 파일이름 앞에 느낌표(!)를 사용.(!는 부정을 의미하는 not과 동일)
  > #환경 설정 파일은 제외하면 안 됨
  > !config.php
- 디렉터리를 표현하는 방법 : 슬래쉬(/)기호 사용
  > #현재 디렉터리 안에 있는 파일 무시
  > /readme.txt

> #/pub/ 디렉터리 안의 모든 것을 무시
> /pub/

> #doc 디렉터리 아래의 모든 .txt 파일 무시
> doc/\***\*/\*\***.txt

깃은 glob패턴을 지원하기 때문에 정규 표현식을 응용하여 작성 규칙을 넣을 수 있다.

##### - glob패턴이란 컴퓨터 프로그래밍에서, 특히 유닉스 계열 환경에서 글로브(glob) 패턴은 와일드카드 문자로 여러 파일 이름의 집합을 지정한다. 이를테면 유닉스 명령어 mv *.txt textfiles/은 현재 디렉터리의 .txt로 끝나는 이름의 모든 파일을 textfiles 디렉터리로 이동(mv)시킨다. 여기에서 *는 모든 문자열을 가리키는 와일드카드이고 \*.txt는 글로브 패턴이다. 그 밖의 일반적인 와일드카드는 하나의 문자를 가리키는 물음표(?)이다.

---

## 3.6 깃 저장소 복제

- 외부에 있는 기존 프로젝트(깃허브, 비트버킷)를 기반으로 저장소를 생성하려면 외부 저장소를 복제해서 생성해야 하고 외부저장소를 이용해 로컬 저장소를 생성하는 것을 '깃 저장소 복제'라고 한다

### 3.6.1 공개 저장소

- 대표적으로 깃허브, 비트버킷와 같은 깃 호스팅 사이트가 있다.
- 깃 호스팅 서비스는 공개된 저장소와 비공개된 저장소를 모두 지원하고 공개된 저장소는 누구나 복제하여 코드를 내려받을 수 있다 -요즘은 오픈 소스가 활성화 되어 저장소를 공개하고있고 수많은 오픈소스를 깃으로 관리하고 공개 저장소를 이용하여 배포한다.

### 3.6.2 다운로드 vs 복제

- 깃을 이용하여 저장소를 복제하면 최종코드 뿐만 아니라 중간에 커밋 같은 변화의 모든 이력도 같이 내려 받을 수 있고 일부코드를 변경하여 기여하는 것도 가능하다.

### 3.6.3 복제 명령어

- 깃의 저장소를 복제하는 명령어는 clone이다.
- 복제하려면 공개된 저장소의 URL이 필요하다
- 복제할때 폴더 이름을 지정하지 않으면 공개 저장소에서 사용된 폴더와 동일한 이름으로 새폴더를 만든다.

> $ git clone 원격저장소URL 새폴더이름

git clone 명령어를 사용하면 깃은 자동으로 깃서버에 접속하고 저장소의 모든 소스 코드를 자동을 내려받는다.
깃은 저장소 안에 있는 파일들과 .git 리포지터리를 기반으로 이력을 관리한다. => 복제한 뒤 복제된 폴더이름을 그대로 사용할 필요x

---
# 차례

[TOC]

# 시작하며....

지금까지 사용했던 형상 관리 툴을 보면 ClearCase, Perforce, SVN 정도가 될거 같아요.

Git은 회사에서 사용하지 않다 보니까 잘 사용하지 않게 되더라고요.

그런데 이번에 Vim과 Plantuml등을 정리 하면서 정리 내용을 Blog에 올리고는 있지만 좀 더 버전 관리라는 것을 하고 싶었죠.

그렇게 Git을 알아 가니 이게 정말 재미있더라고요.

기존 형상 관리 툴과는 다른 점도 나름 끌리기도 하고요.^^

그렇게 알게 된 점들을 여기에 정리 하려고 합니다.

하나라도 끝내면서 다음을 진행 해야 하는데...

하고 싶은것만 많은건 아닌지...

Git은 금방 끝내겠습니다!!!!

# Git과 다른 툴과의 차이점

처음 Git을 보면서 가장 어려웠던 점은 용어 정리 였어요.

다른 형상관리 툴에서 사용하던 명령어나 용어들과 다른 점이 너무 많았지요.

아마 처음에 **add** 라는 명령어 때문에 고민에 빠졌던거 같아요.

**add**는 **더하기** 잖아요???

그래서 처음에 저 명령어를 보았을 때 신규 파일을 추가 하는거라고 생각했어요.

근데 아니더라고요....;;;

또... **checkout**.... 이건 다른 툴에서는 내가 해당 파일을 수정 할 것이다라고 지정 하는거였죠.
(저런 느낌이 아닐 수도 있습니다. 지극히 개인적인 생각입니다.)

그런데 Git에서는 단순 Branch를 변경 하는 거 였어요.

저런 용어 정리 때문에 처음에 "Git은 어려운거!!!" 라는 편견을 가지고 있었죠.

그러다가 이제 필요 하기 시작하니 기초부터 차근차근 다시 배우기 시작한거죠.

기초 부터 다시 하니 Git에 대한 편견도 많이 사라지고 오히려 Git이 멋져 보이더라고요.

지금 이 글도 Git에 올리면서 쓰고 있을 정도니까요..^^

그럼 Git이 무엇 일까요????

# Local?? Remote?? Repository??

용어 정리가 필요 하겠죠?? 

우선 저의 정의나 설명이 정확하지 않을 수 있다는 것을 말씀 드립니다.;; ㅎㅎ

필요한 것만 배운 상태라 기본 원리와는 많이 다를 수 있어요.^^;;

```uml
@startuml
state Local {
    state "Working Directory" as WD {
    }
    state "Staging Area" as SA {
    } 
    state Repository {
    }
}

state Remote {
    state "Repository" as RemoteRepository {
    }
}
}@enduml
```

<br>

Git은 위의 그림처럼 크게 두가지로 나눌 수 있는거 같아요.

제가 보았을 때의 구분법은 다음과 같죠.

* Local : 개인이 작업 하는 공간
* Remote : Backup을 위해 자료가 저장되는 공간

그리고 Local에 있는 내용도 설명을 드리면 아래와 같을거 같아요.

```uml
@startuml
state Local {
    state "Working Directory" as WD {
    }
    state "Staging Area" as SA {
    } 
    state Repository {
    }
    WD : 신규 혹은 수정된 파일들\n
    WD : (= Working Copy, Working Tree)
    SA  : 다음 Commit을 위한 파일들\n
    SA : (= Index)
    Repository : Git에서 관리되는 모든 파일들;\nMetadata, object database\n
    Repository : (= .git Directory)
}
@enduml
```

<br>

위 처럼 나누다 보니 Remote의 역할이 처음에 이해가 되지 않았어요.

타 버전관리 툴을 보게 되면 보통 서버들이 버전들에 대한 정보를 모두 가지고 있어요.

버전에 대한 소스 파일들이나 다른 메타데이터들을요.

그런데 Git에서는 Remote라는건 단순 백업 서버라고 하니 이 때부터 맨붕에 빠져 있게 되었죠.

그럼 그런 버전 정보들은 어디에 보관 되어 있는건가요?

Local 영역의 Repository에 저장 된 거죠.

History나 각 버전 별 소스 들이나 모두 Local에 있다는 거죠.

즉... 네트워크로 연결 할 수 없는 비행기 안이나 산골 지방에서도 Git으로 버전 관리를 할 수 있는거죠.

그럼 다음에는 서로간의 상태(?) 이동을 위해 어떠한 명령어가 필요한지 이야기를 나누어 볼게요.

# git의 기본 명령어

```uml
@startuml
state Local {
    state "Working Directory" as WD {
    }
    state "Staging Area" as SA {
    } 
    state "Repository" as Re {
    }
    Re : 1. **git init**

    WD : 2. echo TEST > A.txt
    WD -> SA : 3. **git add A.txt**

    SA : 4. A.txt in Staging Area
    SA -> Re : 5. **git commit**

    Re : 5. A.txt committed
}
@enduml
```

<br>

위와 같이 Local에서의 상태의 이동 명령어를 간략하게 적어 보았습니다.

하나씩 설명을 해 드릴게요.

## 1. git init

우선 git은 모두 설치 되어 있고 터미널에서 사용 할 수 있다고 가정 하고 진행 하겠습니다.

만약에 Git으로 프로젝트를 관리 하고 싶다면 우선 해당 프로젝트의 디렉토리로 이동을 해요.


<pre>
cd $ProjectDirectory
git init
</pre>

그리고 init 명령어로 해당 디렉토리는 Git이 관리 한다라고 생각하면 되요.

## 2. 파일 추가와 상태 확인(git status)

파일을 하나 추가 해 보죠.

<pre>
echo TEST > A.txt
</pre>

그럼 A.txt가 Working Directory라는 영역에 있는거라고 생각하면 되요.

지금 파일을 생성을 했는데 현재 상태가 궁금하겠죠?

그럼 상태를 한번 볼게요.

git status라는 명령어를 입력 하면 되요.

<pre>
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add &ltfile&gt..." to include in what will be committed)

	A.txt

nothing added to commit but untracked files present (use "git add" to track)
</pre>

위처럼 나오지요?

간단한 영어니까 읽어 보면...

Untracked files가 있는데 그 파일 이름이 A.txt 라네요.

지금 저 파일이 Working Directory 상태라고 생각하면 되요.

## 3. git add A.txt

Working Directory 상태에서는 말 그대로 작업을 하는 상태예요.

지금 이 상태를 이제 거의 완료 했다고 가정한다면 이제 버전 관리를 해야 겠지요???

그럼 Repository 쪽으로 이동을 해야 하는데 중간 단계인 Staging Area로 먼저 이동을 해 두어야해요.

<pre>
git add 파일명
</pre>

위 명령어를 적게 되면 Staging Area로 이동하게 되요.

다시 상태를 볼까요???

<pre>
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   A.txt
</pre>

변경 사항이 있는데 그건 앞으로 commit을 해야할거라네요.

그 파일은 A.txt라고 적혀 있고요.

이제 남은 건 Commit 하는 것만 남았네요.^^

## 4. git commit -m '메세지'

모든 수정이 완료된 상태이니 이제 Repository로 이동 시키고 버전 관리를 할 수 있도록 하면 되겠지요?

<pre>
git commit
</pre>

위 명령어를 치면 Staging Area에 있는 모든 파일들이 Repository로 이동 하게 되는데...

메세지를 입력하기 위해 Vim이 자동으로 실행 되요.

메세지를 입력하고 Vim을 종료 하면 되는데 귀찮을 수 있으니 옵션인 -m을 입력 하면 Vim 실행 없이 바로 메세지가 입력 되요.

<pre>
git commit -m '메세지'
</pre>

위와 같이 입력 하면 되지요.

## 5. 상태 확인

Working Directory에도 없고 현재 버전 관리가 어떻게 되고 있는지 확인을 해 봐요.

status는 간단하죠??

<pre>
$ git status
On branch master
nothing to commit, working tree clean
</pre>

위 처럼 이제는 Commit 할 것도 없고 Working Tree도 깨끗하다고 하네요.

Working Tree는 Working Directory를 이야기 하는거예요.

그럼 버전 관리의 확인은 어떻게 될까요??

log라는 명령어를 입력하면 되요.

<pre>
$ git log
commit d03e82eadca57c205366eee6375a4c19e0e297a7 (HEAD -> master)
Author: 사용자이름 <사용자이메일주소>
Date:   Commit한 시간

    Add A.txt
</pre>

위처럼 나올거예요.

그러니까 Author의 정보를 가진 사람이 Date 시간에 무슨 일을 했는데...

그때 입력한 메세지는 "Add A.txt"네요.

# 마치며....

가장 기본적인 Git에 대해 나열했네요.

하지만 저 명령어 들만이라도 익숙해 진다면 어디가서 Git을 할 수 있다고 해도 될거 같습니다.

확장 기능들이 더 많지만 가장 기본부터 차근차근 하자고요.^^










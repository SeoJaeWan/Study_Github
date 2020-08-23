## Git에서 여기저기로 옮겨다니니

Git의 커밋 트리에서 이동할 수 있는 여러가지 방법을 여기서 알아본다!

### HEAD 분리하기

HEAD는 현재 체크아웃된 커밋을 가리킨다. 즉, 현재 작업중인 커밋이다.

HEAD는 항상 작업 트리의 가장 최근 커밋을 가리킨다. 작업 트리에 변화를 주는 git 명령어들은 대부분 HEAD를 변경하는 것으로 시작된다!
일반적으로 HEAD는 브랜치의 이름을 가리키고 있다. 커밋을 하게 되면, 현재 브랜치의 상태가 바뀌고 이 변경은 HEAD를 통해 확인이 가능하다!

HEAD를 분리하는 것은 HEAD를 브랜치 대신 커밋에 붙이는 것을 의미한다. 명령을 사용하기 전의 모습은

HEAD -> master -> C1 이다.

<!-- ![rebase](https://user-images.githubusercontent.com/52366178/90951097-7bb46700-e492-11ea-972a-a38ce048a65a.JPG) -->

이것을 <code>git checkout C1</code>을 통해 HEAD를 브랜치대신 커밋에 붙일 수 있다.

그러면 HEAD -> C1 이렇게 된다.

### 상대 참조

Git에서 여기저기 이동할 때 커밋의 해시를 사용하는 방법은 조금 귀찮다. Git에서 해시를 사용해 이전 레벨로 가려면 `fed2da64c0efc5293610bdd892f82a58e8cbc5d8`를 입력해야한다.
하지만 Git은 똒똒 해서 해시가 고유한 값을 입력해주면 된다. 위 긴 Hash 대신 `fed2`만 입력해도 된다. 하지만 커밋들을 해시로 구분하고 사용하는 것은 편하다고 할 수 없다.
여기서! Git의 상대 참조(Relative Ref)가 등장한다.

상대 참조는 우리가 기억할 만한 지점(브랜치 bugFix, HEAD 등등)에서 출발하여 이동해 다른 지점에 도달해 작업을 할 수있게 해준다.

이러한 상대 커밋은 강력한 기능인데, 이번엔 두 가지 방법을 사용한다.

- 한번에 한 커밋 위로 움직이는 <code>^</code>
- 한번에 여러 커밋 위로 올라가는 `~<num>`

#### 캐럿 (^)

먼저 캐럿 (^) 연산자 부터 알아보자면, 참조 이름에 하나씩 추가할 때마다, 명시한 커밋의 부모를 찾게 된다.

`master^` 는 "master의 부모" 와 같은 의미이다.
`master^^` 는 master의 조부모(부모의 부모)를 의미한다.

master 위에 있는 부모를 체크아웃 해 보자면 아래 이미지에서

<!-- ![rebase](https://user-images.githubusercontent.com/52366178/90951097-7bb46700-e492-11ea-972a-a38ce048a65a.JPG) -->

`git checkout master^` 를 입력한다.

<!-- ![rebase](https://user-images.githubusercontent.com/52366178/90951097-7bb46700-e492-11ea-972a-a38ce048a65a.JPG) -->

Wow! 커밋의 해시를 입력하는 것보다 훨씬 쉬운 방법이다!

브랜치 뿐만아니라 HEAD도 상대 참조를 위해 사용할 수 있다!

<!-- ![rebase](https://user-images.githubusercontent.com/52366178/90951097-7bb46700-e492-11ea-972a-a38ce048a65a.JPG) -->

여기서 `git checkout C3; git checkout HEAD^; git checkout HEAD^; git checkout HEAD^` 를 하면

<!-- ![rebase](https://user-images.githubusercontent.com/52366178/90951097-7bb46700-e492-11ea-972a-a38ce048a65a.JPG) -->

이렇게 상위 커밋으로 올라갈 수 있다.

#### ~ 연산자

커밋 트리를 위로 여러 단계를 올라가고 싶을 수 있다. `^`를 계속 입력해서 올라가는 방법 말고 더 좋은 방법이 있다!
바로 Git의 `~ 연산자` 입니다.

~ 연산자는 선택적으로 올라가고 싶은 부모의 갯수가 뒤에 숫자로 온다!

<!-- ![rebase](https://user-images.githubusercontent.com/52366178/90951097-7bb46700-e492-11ea-972a-a38ce048a65a.JPG) -->

위 커밋 트리에서 4단계 위로 올라가고 싶다면, `git checkout HEAD~4` 를 입력한다!

<!-- ![rebase](https://user-images.githubusercontent.com/52366178/90951097-7bb46700-e492-11ea-972a-a38ce048a65a.JPG) -->

Wow!!

#### 브랜치 강제로 옮기기

상대 참조를 사용하는 가장 일반적인 방법은 브랜치를 옮길 때 이다. `-f` 옵션을 이용해서 브랜치를 특정 커밋에 직접적으로 재지정 할 수있다.

<!-- ![rebase](https://user-images.githubusercontent.com/52366178/90951097-7bb46700-e492-11ea-972a-a38ce048a65a.JPG) -->

`git branch -f master HEAD~3` 이렇게 하면 master 브랜치를 HEAD에서 세 번 강제로 뒤로 옮길 수 있다.!
상대 참조를 통해 C1을 간결한 방법으로 참조할 수 있고, 브랜치 강제(-f)를 통해 브랜치를 빠르게 뒤로 이동 시킬 수 있다.

### Git에서 작업 되돌리기

Git에서 작업한 것을 되돌리는 여러가지 방법이 있다.

변경 내역을 되돌리는 것도 커밋과 마찬가지로 낮은 수준의 일(개별 파일이나 묶음을 스테이징 하는 것)과 높은 수준의 일 (실제 변경이 복구되는 방법)이 있다.
여기서는 후자에 대해서 알려준다!

Git에서 변경한 내용을 되돌리는 방법은 크게 두 가지 있다.
하나는 `git reset`을 쓰는 것이고, 다른 하나는 `git revert`를 사용하는 것이다.

#### Git reset

`git reset`은 브랜치로 하여금 예전의 커밋을 가리키도록 이동시키는 방식으로 변경 내용을 되돌린다.
이런 관점에서 " 히스토리를 고쳐쓴다" 라고 말할 수 있다.

즉, `git reset`은 애초에 커밋하지 않은 것처럼 예전 커밋으로 브랜치를 옮기는 것이다.

<!-- ![rebase](https://user-images.githubusercontent.com/52366178/90951097-7bb46700-e492-11ea-972a-a38ce048a65a.JPG) -->

여기서 `git reset HEAD~1`을 입력해보자!

그림처럼 master 브랜치가 가리키던 커밋을 C1으로 다시 옮겼다! 이러면 로컷 저장소에서 C2 커밋이 아에 없었던 것 같은 상태가 된다.

#### Git revert

각자의 컴퓨터에서 작업하는 로컬 브랜치의 경우 리셋(reset)을 잘 쓸 수 있지만, " 히스토리를 고쳐쓴다 " 는 점 때문에 다른 사람이 작업하는 리모트 브랜치에는 쓸 수 없습니다.

변경분을 되돌리고, 이 되돌린 내용을 다른 사람과 <em>공유하기</em>위해서는, `git revert`를 써야한다.

<!-- ![rebase](https://user-images.githubusercontent.com/52366178/90951097-7bb46700-e492-11ea-972a-a38ce048a65a.JPG) -->

여기서 `git rever HEAD`를 쓴다면

<!-- ![rebase](https://user-images.githubusercontent.com/52366178/90951097-7bb46700-e492-11ea-972a-a38ce048a65a.JPG) -->

어색하게도, 되돌리려고한 커밋의 아래에 새로운 커밋이 생긴다.
C2`라는 새로운 커밋에 변경내용이 기록되는데, 이 변경내역은 C2 커밋 내용의 반대되는 내용이다.

이렇게 리버트를 하면 다른 사람들에게도 변경 내역을 push 할 수 있다.

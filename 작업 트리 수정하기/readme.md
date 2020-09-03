## 작업 트리 수정하기

여기서 배울 것은 작업을 여기저기 옮기기를 배울 것이다

### Git 체리-픽( Cherry-pick )

첫 번쨰로 git cherry-pick 이다.

아래와 같이 사용한다.
` git cherry-pick <Commit1> <Commit2> <...>`

현재 위치(HEAD) 아래에 있는 일련의 커밋들에 대한 복사본을 만들겠다는 것을 줄인 것이다.

![cherry-pick](https://user-images.githubusercontent.com/52366178/92064349-4edc4a00-edd8-11ea-9394-6a20e4b0a46a.JPG)

위와 같은 repository가 있다. `master`와 master로 복사하고 싶은 작업이 있는 브랜치 `side`가 있다.
해당 작업은 rebase를 통해 할 수도 있다.

```
git checkout side

git rebase master

git checkout master

git rebase side
```

위와 같이 말이다.

하지만 체리-픽을 사용하면 어떻게 작업을 수행하는지 확인해보자

`git cherry-pick C2 C4`

![cherry-pick finish](https://user-images.githubusercontent.com/52366178/92064351-500d7700-edd8-11ea-823b-c93f6ce17bd0.JPG)

`C2`와`C4`의 커밋을 master로 보내기 위해서 위와같이 아주 간단하게 수행할 수 있다.

### Git 인터렉티브 리베이스 (Interactive Rebase )

git cherry-pick은 원하는 커밋이 무엇인지 알 때 아주 유용하다.

하지만! 원하는 커밋을 모르는 상황에선?! 다행스럽게 git은 이런 상황에 대한 대안이 있다.
우리는 이런 상황에 인터렉티브 리베이스를 사용하면 된다.

인터렉티브 리베이스는 `rebase` 명령어를 사용할 때 `-i` 옵션을 같이 사용한다.

이 옵션을 추가하면, git은 리베이스의 목적지가 되는 곳 아래에 복사될 커밋들을 보여주는 UI를 띄워준다.
이때 각 커밋을 구분할 수 있는 각각의 해시들과 메시지도 보여준다.

"실제" git 에서는 UI창을 띄우는것 대신에 `vim`과 같은 텍스트 편집기에서 파일을 연다.

인터렉티브 리베이스 대화창이 열리면, 3가지를 할 수 있다.

- 적용할 커밋들의 순서를 UI를 통해 바꿀수 있다.

- 원하지 않는 커밋들을 뺼 수 있다. 이것은 `pick`을 이용해 지정할 수 있다.

* 마지막으로, 커밋을 스쿼시(squash)할 수 있다? 한마디로 커밋을 합칠 수 있다.

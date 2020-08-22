## Git Commit

커밋은 Git 저장소에 디렉토리에 있는 모든 파일에 대한 스냅샷을 기록하는 것이다.
디렉토리 전체를 복사하여 붙여넣는 것과 유사!!

Git은 가능한 한 커밋을 가볍게 유지하고자 하기 때문에, 커밋할 때마다 디렉토리 전체를 복사하진 않는다.
즉 각 커밋은 저장소의 이전 버전과 다음 버전의 변경내역(=> delta 라고 부름)을 저장한다. 그래서 대부분의 커밋이 그 커밋 위의 부모 커밋을 가리킨다.

한마디로! 커밋은 프로젝트의 스냅샷이며, 매우 가볍고 커밋 사이의 전환도 빠르다!

<code> git commit </code> 명령어로 커밋을 진행할 수 있다.

## Git Branch

Git 에는 브랜치라는 친구가 있다!
이 친구도 놀랍도록 가볍습니다! 🕊

브랜치를 많이 만들어도 메모리나 디스크 공간에 부담이 느껴지지 않기 때문에, 작업을 커다란 브랜치로 만드는 것 보단,
잘게 잘라 작은 단위로 나누는 것이 좋다.

브랜치를 " 하나의 커밋과 그 부모 커밋들을 포함하는 작업 내역"( => 무슨말이지?! ) 이라고 기억하면 된다.

<code> git branch [브랜치명] </code> 로 브랜치를 만들 수 있다.
<code> git checkout [브랜치명] </code> 으로 만든 브랜치로 현재 브랜치를 이동시킬 수 있다.

## Git Merge

<code> git commit </code> 과 <code> git Branch </code>로 커밋과 브랜치를 만드는 방법을 알았다.
이제 별도의 브랜치를 합치는 몇가지 방법을 알아볼 차례이다.

첫 번째 방법은 <code> git marge </code>이다!

Git의 Marge는 두 개의 부모를 가리키는 특별한 커밋을 만들어 낸다.
두 개의 부모가 있는 커밋이라는 것은 " 한 부모의 모든 작업 내역과 나머지 부모의 모든 작업, 그리고 그 두 부모의 모든 부모들의 작업 내역을 포함한다. " 라는
의미가 있다.

<img src ="https://github.com/SeoJaeWan/Study_Github/blob/master/Git%20%EA%B8%B0%EB%B3%B8/Assets/marge/marge.JPG" width="300px>

각 브랜치에 독립된 커밋이 한개씩 있다.
즉, 지금까지 작업한 내역이 2개의 브랜치에 나눠서 저장되어있단 말이다.

master 브랜치 입장에서 bugFix 브랜치와 master 브랜치를 marge 하면 <code> git merge bugFix </code>

<img src ="https://github.com/SeoJaeWan/Study_Github/blob/master/Git%20%EA%B8%B0%EB%B3%B8/Assets/marge/margeFinish.JPG" width="300px>

master가 두 부모가 있는 커밋을 가리키고 있다.

마찬가지로 <code> git checkout </code> 명령어를 통해 bugFix로 브랜치를 이동하며 <code> git merge master </code> 하면
두 브랜치 모두 같은 작업 내역을 포함하게 된다!

<img src ="https://github.com/SeoJaeWan/Study_Github/blob/master/Git%20%EA%B8%B0%EB%B3%B8/Assets/marge/margeFinish2.JPG" width="300px>

## Git 리베이스(Rebase)

브랜치끼리 작업을 합치는 두 번째 방법으론 리베이스(Rebase)가 있다.
리베이스는 기본적으로 커밋들을 모아서 복사한 뒤, 다른 곳에 떨궈 놓는 것이다.

이렇게 리베이스를 하면 커밋들의 흐름을 보기 좋게 한 줄로 만들 수 있다는 장점이 있다.

<img src ="https://github.com/SeoJaeWan/Study_Github/blob/master/Git%20%EA%B8%B0%EB%B3%B8/Assets/rebase/rebase.JPG" width="300px>

또다시 브랜치 두 개가 있다.
현재 bugFix블랜치가 현재 선택 되어있다.

bugFix 브랜치에서 작업을 master 브랜치로 직접 옮겨 놓으려고 한다.
그렇게 하면 실제로 두 기능을 따로 개발했지만, 마치 순서대로 개발한 것처럼 보이게 된다.

<code> git rebase master </code> 명령어를 통해 합치게 되면

<img src ="https://github.com/SeoJaeWan/Study_Github/blob/master/Git%20%EA%B8%B0%EB%B3%B8/Assets/rebase/rebaseFinish.JPG" width="300px>

이렇게 bugFix 브랜치의 작업 내용이 master 바로 위에 깔끔한 한 줄 커밋으로 보이게 된다.
이때 C3 커밋은 어딘가에 아직 남아있고, C3'은 master 위에 올려 놓은 복사본이 된다.

하지만 아직 master가 그대로 남아있다.

master 브랜치를 선택한 상태에서 <code> git rebase bugFix </code> 입력하여 리베이스를 하면 된다.

<img src ="https://github.com/SeoJaeWan/Study_Github/blob/master/Git%20%EA%B8%B0%EB%B3%B8/Assets/rebase/rebaseFinish2.JPG" width="300px>

보이는 바와 같이, master가 bugFix의 부모쪽에 있었기 때문에, 단순히 그 브랜치를 앞쪽의 커밋을 가리키게 이동시킨 것이다.

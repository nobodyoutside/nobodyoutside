# git

## 원격 저장소에 올라간 커밋 되돌리기

https://jupiny.com/2019/03/19/revert-commits-in-remote-repository/

`$ git revert [되돌리고 싶은 commit의 hash]`

## 두개의 리포지토리 병합하기

메인 저장소에 대상 저장소를 원격 연결한 다음 이것 fatch로 받아서 합침

> [출처 : syki66](https://syki66.github.io/blog/2020/09/07/merge-repos.html)

```text
1. 레포지토리 주소 연결 및 fetch
프로젝트2에서 아래 명령어를 입력하여, 가져올 프로젝트1의 깃 주소를 연결해주며, 동시에 fetch를 진행한다.

git remote add --fetch 프로젝트1 https://github.com/syki66/프로젝트1.git


2. 레포지토리 병합
아래 명령어를 입력하여 레포지토리를 병합한다.

git merge 프로젝트1/master --allow-unrelated-histories


이후 vi 에디터가 뜨게되고, 병합에 대한 커밋 메세지를 적고 나서 esc -> :wq 를 입력하고 빠져나오게 되면, 프로젝트1의 변경 이력들이 모두 병합된 것을 확인할 수 있다.



3. 연결된 주소 제거
더이상 프로젝트1의 주소가 필요 없다면 삭제해주도록 한다.

git remote remove 프로젝트1


이후 원격 저장소로 git push를 해주면 된다.
```

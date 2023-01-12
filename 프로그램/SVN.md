# 무시 폴더 및 파일 관리

## 커맨드 라인으로 하는 방법

  > 문법: `svn propedit svn:ignore {path}svnpropedit svn:ignore`
  > 
  `{path}`에는 원하는 경로를 써 주면 된다. 위 명령은 현재 폴더의 무시할 파일을 설정하는 명령이다.
  
  에디터가 뜨면 무시할 파일이나 폴더를 한 줄에 하나씩 써 주자.
  
  무시할 파일을 하위 디렉토리에도 똑같이 적용하게 하고 싶다면 svn:global-ignores 속성을 지정하면 된다.
  
  > 문법: svn propedit svn:global-ignores {path}svn propedit svn:global-ignores .
  > 
  역시 에디터에 한 줄에 하나씩 써 주면 된다.



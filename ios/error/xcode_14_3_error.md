# Xcode 14.3이상에서 발생하는 아카이브 오류

아카이빙을 하면 PhaseScriptExecution [CP]\ Embed\ Pods\ Frameworks ... 에러가 발생하는데   
XCode 14.3부터 발생하기 시작했다. (14.3.1로 업데이트 했는데 여전히 발생...)

고치는 방법은 간단하다.  

Pods-ProjectName-frameworks.sh 파일에있는

```
source="$(readlink "${source}")"
```

을

```
source="$(readlink -f "${source}")"
```

로 변경하고 아카이브 하면 잘된다.

Xcode가 문제인지 CocoaPod이 문제인지... 찾아봐도 잘 안나오는거 같아서 원인은 패스~
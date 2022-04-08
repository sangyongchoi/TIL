# set-if-not-exists

개발을 하다보면 키가 존재하지 않는다면 값을 set 하고 존재한다면 set 을 하지 않아야할때가 존재한다.

Redis에는 그럴때 유용하게 사용할 수 있는 명령어가 존재한다.

> setnx
> 

setnx는 set if not exists 의 약자로 키가 존재하지 않을때만 set 을 해주는 명령어다.

key 가 정상적으로 set 됐다면 반환값으로 1을 돌려준다.

key 가 set 되지 않았다면 반환값으로 0을 돌려준다.

아래와 같이 확인할 수 있으며 key 가 존재할때는 값을 덮어쓰지 않는다.

![Untitled](set-if-not%204f1de/Untitled.png)

간단한 distributed lock 이  필요할 때 사용할 수 있다.
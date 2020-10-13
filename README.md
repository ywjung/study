# study

본자료는 인터넷의 자료와 Tacademy자료를 참고하여 추가적으로 테스트를 통하여 내부 직원들과 세미나를 하기 위해 만든 자료입니다.


```
모든 컨테이너 삭제하기

docker stop $(docker ps -a -q)

docker rm $(docker ps -a -q)



모든 이미지 삭제하기

docker rmi $(docker images -q)



Exit 상태의 모든 컨테이너 삭제하기

docker rm $(docker ps --filter 'status=exited' -a -q)
```

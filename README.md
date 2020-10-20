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


https://nullsweep.com/dynamic-security-scanning-in-a-ci-zap-scanning-with-jenkins/

https://www.we45.com/blog/step-by-step-guide-integrate-zap-into-jenkins-ci-pipeline

https://github.com/zaproxy/zaproxy/releases/download/v2.9.0/ZAP_2.9.0_Linux.tar.gz

[2019] PAYCO 쇼핑 마이크로서비스 아키텍처(MSA) 전환기
https://www.youtube.com/watch?v=l195D5WT_tE



* WSL restart
관리 PowerShell 프롬프트에서 : Restart-Service LxssManager



WSL2에 메모리 할당 강제
이는 wsl2의 컨테이너에 할당되는 메모리를 강제하는 방식이다. 먼저 자신의 유저 디렉터리 밑( C:\Users\사용자이름 )에. wslconfig 파일을 하나 만들어준다. 그 뒤 아래의 내용을 적어준다
 
```
[wsl2]
memory=6GB
swap=0
```

memory의 설정은 자신의 컴퓨터 메모리의 여유 정도를 봐가며 설정하자.

이로써 메모리 할당 문제에 대해 임시방편적 대처가 가능하다. 적용이 됐나 확인을 해보자. 우분투 컨테이너에서 free -h로 메모리 사용량을 확인하는 방법으로 진행했다.


---

docker compose에서 특정 컨테이너를 재시작해서 테스트하고 싶을 때가 있다.

Failover와 같은 문제를 테스트하려면 특정 시간 멈춤 뒤에 재현할 때 유용하다.

 

docker-compose restart mysql

docker-compose restart -t 30 mysql


http://localhost:9000/api/issues/search?componentKeys=b2c_api_prj

https://medium.com/@jyson88/jenkins-github-gradle-%EB%B9%8C%EB%93%9C-7db73c09139b

https://beomseok95.tistory.com/201


```
while true; do 실행을 희망하는 명령어; sleep 반복 희망하는 초(n); done
```

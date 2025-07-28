## **✅ 문제 : 간단한 서버 관리 스크립트 작성**

### **🔧 요구사항**

* `start`: 포트 8000에서 `http.server`를 백그라운드로 실행 (`nohup`, 로그는 `server.log`)

* `stop`: 실행 중인 프로세스를 찾아 종료

* `status`: 프로세스가 실행 중인지 확인하여 출력

* `restart`: 중지 후 다시 실행

  ### **🎯 실행 예시**

  $ ./webserver.sh start  
  서버가 백그라운드에서 시작되었습니다.  
    
  $ ./webserver.sh status  
  서버 실행 중입니다. PID: 13579  
    
  $ ./webserver.sh stop  
  서버가 종료되었습니다.  
    
  $ ./[webserver.sh](http://webserver.sh) tail\_log  
  … log message 확인


문제 모두 조건에 따라:

* `if [ "$1" == "start" ]` 식으로 흐름 제어

* 변수 `PORT`, `PID`, `LOGFILE` 등을 정의해 구성 가능

```bash
#vi 파일

#!/bin/bash
V_STATUS="$1"
V_PORT=8000
#프로세스에서 http.server 찾기
V_PROC="$(ps aux | grep "http.server")"
V_LOGFILE="server.log"
#echo ""없이 출력시 공백문자를 1개까지 모두 생략
V_PID="$(echo $V_PROC | cut -d" " -f2)"

#시작
if [ "$V_STATUS" == "start" ]; then
        nohup python3 -m http.server "$V_PORT" --bind 0.0.0.0 &>> "$V_LOGFILE" &
        echo "server start from background"
        #상태
elif [ "$V_STATUS" == "status" ]; then
        echo "server is running PID = $V_PID"
        #중지
elif [ "$V_STATUS" == "stop" ]; then
        kill -9 "$V_PID"
        echo "server end"
        #로그
elif [ "$V_STATUS" == "tail_log" ]; then
        tail -f "$V_LOGFILE"
elif [ "$V_STATUS" == "restart" ]; then
        kill -9 "$V_PID"
        nohup python3 -m http.server "$V_PORT" --bind 0.0.0.0 &>> "$V_LOGFILE" &
        echo "server restart from background"
else
        echo
fi

#출력
[seungjae@192.168.0.43 ~/quests]$ ./webserver.sh start
server start from background
[seungjae@192.168.0.43 ~/quests]$ ./webserver.sh status
server is running PID = 5256
[seungjae@192.168.0.43 ~/quests]$ ./webserver.sh stop
server end
[seungjae@192.168.0.43 ~/quests]$ ./webserver.sh tail_log
#서버 실행중 오류
    with ServerClass(addr, HandlerClass) as httpd:
  File "/usr/lib64/python3.9/socketserver.py", line 452, in __init__
    self.server_bind()
  File "/usr/lib64/python3.9/http/server.py", line 1302, in server_bind
    return super().server_bind()
  File "/usr/lib64/python3.9/http/server.py", line 137, in server_bind
    socketserver.TCPServer.server_bind(self)
  File "/usr/lib64/python3.9/socketserver.py", line 466, in server_bind
    self.socket.bind(self.server_address)
OSError: [Errno 98] Address already in use


```
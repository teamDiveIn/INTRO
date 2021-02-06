# FocusMe-Readme

##  Server

### Server Introduction
1. WebRTC 서버 :: 실시간 영상전송 서버 `Node.js`<br>
WebRTC 기반 openvidu 이용 실시간 영상전송<br><br>

2. WebRTC -- Django Adapting Server `Node.js`<br>
Django와 Node 사이에서 세션마다 필요한 token 발행 및 삭제하는 서버<br><br>

3.Thumbnail 서버 :: 실시간 썸네일 전송 서버 `Node.js`<br>
frontend에서 인식하여 캐릭터를 입힌 이미지 썸네일을 5초 간격으로 포워딩하는 서버<br><br>

4. 인증 서버 :: 회원 정보 관리 서버 `Node.js`<br>
회원가입, 로그인, 토큰 등의 멤버 정보 관리<br><br>

5. Pool 서버 :: 풀 정보 관리 서버 `Django`<br>
풀 입장, 풀의 각 멤버의 상태 트래킹하는 서버<br><br>

### Server Architecture Diagram<br>



###  Development Stack<br>
- Web Front: HTML,CSS,JS<br>
- Language/Framework<br>
  - Node.js :: 실시간 정보처리<br>
  - Django :: 그외<br>
- DB/Cache: MySQL, redis<br>
- Cloud Service : EC2, S3, RDS<br><br>


### API<br>

1. WebRTC 서버 :: 실시간 영상 전송 서버<br>
socket<br><br>

2. WebRTC -- Node Adapting Server (Node.js)<br>
기본 url :: https:/webrtc.clubapply.com/ <br>
레포지토리 확인 :: <br>
`POST` /token 방 입장 시 socket connection 설정 위한 pool마다의 토큰 제공 <br>
`DEL` /token 방 퇴장 시 socket connection 해제, 삭세<br><br>

3. 실시간 썸네일 전송 서버<br>
socket<br><br>

4. 인증 서버 <br>
기본 url :: /api/auth
`POST` /auth/user 회원가입<br>
`DELETE` /auth/user 회원탈퇴<br>
`POST` /auth/user/signin 로그인<br>
`POST` /auth/user/logout 로그아웃<br>
`GET` /auth/user/refresh 리프레시 토큰 발급<br>
`POST` /auth/user/verify 토큰 해독 및 유효성 확인, 유효하면 해당 유저 정보 제공<br><br>

5. Pool Server<br>
기본 url :: /pool<br>
`GET` /pool 기존에 존재하는 풀 리스트 <br>
`POST` /pool/enter 새로 풀에 입장<br>
`POST` /pool/leave 자리 비움<br>
`POST` /pool/back 자리 복귀<br>
`POST`/pool/exit 방 나감<br><br>

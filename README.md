# FocusMe-Readme

# FocusMe-Server

## Server

### 서버마다의 role
1. WebRTC 서버 :: 실시간 영상전송 서버 `Node.js`
WebRTC 기반 openvidu 이용 실시간 영상전송

2. WebRTC -- Django Adapting Server `Node.js`
Django와 Node 사이에서 세션마다 필요한 token 발행 및 삭제하는 서버

3.Thumbnail 서버 :: 실시간 썸네일 전송 서버 `Node.js`
frontend에서 인식하여 캐릭터를 입힌 이미지 썸네일을 5초 간격으로 포워딩하는 서버

4. 인증 서버 :: 회원 정보 관리 서버 `Node.js`
회원가입, 로그인, 토큰 등의 멤버 정보 관리

5. Pool 서버 :: 풀
풀 입장, 풀의 각 멤버의 상태 트래킹하는 서버

### Diagram



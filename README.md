# 🤽 할 일에 빠져드는 시간, DiveIn

<div align="center">
  <img src="https://user-images.githubusercontent.com/7090906/107105445-ad1ac680-6869-11eb-9eb9-65e95b945424.png"  width="320" />
  <h3>
    효율적인 시간관리와 집중을 위해<br/>
    AI가 늘 당신과 함께
  </h3>

  <div>
    <img src="https://badgen.net/badge/AI/DiveIn/blue"/>
  </div>
</div>

## UI/UX
### 🎨 Main page
<a href="https://ibb.co/SwtYQp6"><img src="https://i.ibb.co/N29D1zF/design2.png" alt="design2" border="0"></a><br>
### 🎨 Overview
<a href="https://ibb.co/Zcp4TZv"><img src="https://i.ibb.co/j3P04pq/design.png" alt="design" border="0"></a>
<br><br>


## 🤷‍ Service Needs
'줌터디'와 같은 언택트 시대의 새로운 공부법이 등장했습니다.
비대면 시대에서 집에서 혼자 공부하려니 쉽지 않아 친구들과 줌을 켜서 `상호 감시하는` 용도로 화상회의를 쓰기도 합니다.

혼자 있으면 경각심이 떨어져 해야할 일들에 대해서 소홀히 하거나 다른 곳에 시선이 가기 마련입니다.
이 때 웹캠을 통해서 감시를 하는 것도 좋지만, 얼굴이 공개되는 것을 꺼리거나 기존에 화상회의 플랫폼을 통해 타인에 의해 집중력을 관리하는 것에 대한 한계가 있음에 주목했습니다.

얼굴인식을 통해서 집중도&감정 분석 결과를 제공하고, 같은 스터디원들 간에 집중력을 대결할 수 있는 Gamification요소를 넣어 혼자있어도 집중력있게 일/공부를 끝낼 수 있도록 만들고자 합니다.
뿐만 아니라 untact 에서 ontact 로, 많은 사람들과 함께 작업을 하며 상호 생산성을 높여주는 기제로서 작용시키고자 합니다.
<br><br>

## 🔑 Core Service Logic

- 캠을 통해 **얼굴을 인식하여 감정을 분석**해 현재 집중하고 있는지, 또는 자리를 비웠는지를 체크해 집중도를 계산합니다.
- 기존의 카페에서 각자 공부하지만 개인의 부담감을 높여 일에 몰입하게 만드는 것처럼, **화상 통화 기술로 온라인에서도 한 공간에 모여 스터디**를 하게 합니다.
- 스터디하는 중에는 캠이 신경 쓰이지 않게 **나의 화면을 아바타로 대체**하고, 현재 집중하고 있는지 그 척도를 아바타에서 보여줍니다.
<br><br>
## 🚀 Tech

1. 얼굴 인식

|                                                 안면인식                                                  |                                               이모지 씌우기                                               |                                                 배경 처리                                                 |                                               스켈레톤 제거                                               |
| :-------------------------------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------------------------------: |
| ![](https://user-images.githubusercontent.com/7090906/107106329-68455e80-686e-11eb-9b0c-5f35dab93f7c.png) | ![](https://user-images.githubusercontent.com/7090906/107106333-6a0f2200-686e-11eb-9147-0448b5a67d81.png) | ![](https://user-images.githubusercontent.com/7090906/107106335-6b404f00-686e-11eb-9211-500b5e2bd375.png) | ![](https://user-images.githubusercontent.com/7090906/107106339-6d0a1280-686e-11eb-8e60-e9f4774cc584.png) |

2. 썸네일 공유
   1. 짧은 시간 간격으로 (해당 서비스에서는 2초) 프론트에서 ML 분석 후, Canvas에 옮겨 아바타 이미지를 생성
   2. Server에 Presigned URL을 요청해 S3 업로드 URL을 받는다.
   3. S3에 직접 업로드하고, 소켓 통신을 통해 같은 방에 있는 유저들에게 나의 업데이트된 아바타 URL 정보를 전송한다.
   4. 다른 유저들은 해당 URL을 받아 화면을 업데이트 해준다.
<br><br>
## 💻 Development Stack
### FrontEnd<br>
- React<br>
- Ant.d, styled-components

### Server<br>
- Language/Framework<br>
  - Node.js :: 실시간 정보처리<br>
  - Django :: 그외 API 서버 및 서버간 통신 <br>
- DB/Cache: MySQL, redis<br>
- Cloud Service : EC2, S3, RDS, ACM, Route53<br><br>

## 👨‍💻 Frontend

1.  Atomic Design & Contoller 패턴으로 구성
2.  ML :: 웹캠을 통하여 실시간으로 영상을 받아내, 얼굴 모션인식을 분석<br/>
    얼굴을 인식하여 그 위치와 각도에 맞게 이모지를 붙여내 가상의 아바타를 생성

## 👨‍💻 Server
서버 부하분산을 위해 MSA 도입, 다음과 같은 서버들로 구성

1. WebRTC 서버 :: 실시간 영상전송 서버 `Node.js`<br>
   WebRTC 기반 openvidu 이용 실시간 영상전송<br>
2. WebRTC -- Django Adapting Server `Node.js`<br>
   Django와 Node 사이에서 세션마다 필요한 token 발행 및 삭제하는 서버<br>
3. Thumbnail 서버 :: 실시간 썸네일 전송 서버 `Node.js`<br>
   frontend에서 인식하여 캐릭터를 입힌 이미지 썸네일을 5초 간격으로 포워딩하는 서버<br>
4. [ 인증 서버 ](https://github.com/teamDiveIn/FocusMe-Auth-Server) :: 회원 정보 관리 서버 `Node.js`<br>
   회원가입, 로그인, 토큰 등의 멤버 정보 관리<br>
5. [ Pool 서버 ](https://github.com/teamDiveIn/FocusMe-Pool-Server) :: 풀 정보 관리 서버 `Django`<br>
   풀 입장, 풀의 각 멤버의 상태 트래킹하는 서버<br><br>

## 📑 Server Architecture Diagram<br>

<img src="https://user-images.githubusercontent.com/7090906/107105446-b015b700-6869-11eb-8506-9ca9e39ad4e8.png" width="600px"  /><br><br>

## 📑 ERD

<a href="https://ibb.co/gJjcC7p"><img src="https://i.ibb.co/DQVjnby/2021-02-05-13-30-52.png" alt="2021-02-05-13-30-52" border="0"></a><br /><a target='_blank' href='https://imgbb.com/'></a><br><br>


## 🤙 API<br>

1. WebRTC 서버 :: 실시간 영상 전송 서버<br>
   socket<br><br>

2. WebRTC -- Node Adapting Server (Node.js)<br>
   기본 url :: https:/webrtc.clubapply.com/ <br>
   `POST` /token 방 입장 시 socket connection 설정 위한 pool마다의 토큰 제공 <br>
   `DEL` /token 방 퇴장 시 socket connection 해제, 삭세<br><br>

3. 인증 서버 <br>
   기본 url :: /api/auth<br>
   `POST` /auth/user 회원가입<br>
   `DELETE` /auth/user 회원탈퇴<br>
   `POST` /auth/user/signin 로그인<br>
   `POST` /auth/user/logout 로그아웃<br>
   `GET` /auth/user/refresh 리프레시 토큰 발급<br>
   `POST` /auth/user/verify 토큰 해독 및 유효성 확인, 유효하면 해당 유저 정보 제공<br><br>

4. 풀 Server<br>
   기본 url :: /pool<br>
   `GET` /pool 기존에 존재하는 풀 리스트 <br>
   `POST` /pool/enter 새로 풀에 입장<br>
   `POST` /pool/leave 자리 비움<br>
   `POST` /pool/back 자리 복귀<br>
   `POST` /pool/exit 방 나감<br><br>

---
title: "Github actions로 라즈베리파이 헬스체크 알림 만들기 - 구상"
excerpt_separator: "<!--more-->"
categories:
  - Projects
tags:
  - 라즈베리파이 헬스 체크 알림
---

## 라파가 죽을 때 알림이 있으면 괜찮겠다.
 아직도 라즈베리파이가 가끔 죽는다. 대개 20일? 정도 지나면, 꺼지는 것 같기도 하고, 가끔 내가 자바로 부하를 걸다가, 잡시 뒤에 갑자기 죽어버리는 것 같기도 하다. 원인은 파악중인데, 로그가 없어서 당장 해결하는 것은 어려울 것 같다.

 그러다가 라파가 죽는 걸 체크할 수 있는 무언가가 있을까? 라고 생각했는데, 그렇다고 또 서버를 만들 수도 없는 노릇이다. 서버를 위한 서버라니.. 이상하지 않나. 그러다가 문득 github actions가 크론으로 돌아가면 상시 체크가 가능하겠다는 생각이 들었다!
 
 [다음 글](https://yceffort.kr/2020/07/cron-job-with-github-actions)을 참고해서 바로 가능하겠구나 생각하고 작업할 [리포](https://github.com/qlqhqo2341/check_live_my_rpi/)부터 당장 만들었다. 

## 프로젝트 구상
 node.js 코드를 작성해서, 만약 200응답이 없거나, 커넥션 조차 되지 않을 경우라면? 간단히 해당 리포에 이슈가 자동으로 등록 되어서 메일이 저절로 날아오게 할 수 있을 것 같다. actions 자체 비밀키 설정으로, 리포 토큰을 하나 만들어 두고, 저절로 이슈가 달리게 하고, 만약 이슈가 존재하면, 코멘트를 계속 달리게해도 좋을 것 같고, (나중에 이슈 보기 힘들어질 것 같기도 하다.)


## 프로젝트 진행
 매우 간단하게 진행 했다. node.js를 쓰진 않았고, 파이썬으로 requests 사용해서 넣었다. 단순히 체크해서 결과를 콘솔로 프린트하고, 에러 파일도 하나 뱉은 뒤에, exit code를 오류가 있을때 다르게 주기만 했는데, 아래 이미지 처럼 github에서 오류 메일을 날려준다.

 ![오류 메일 이미지](/assets/images/check_error.png)

 이정도만 해도 감지덕지인 것 같다. 오늘도 갑자기 라파가 접속이 안되서, 메일을 보니 이미 몇 개의 메일 알림이 와있더라, 정상적으로 작동하는 것 같아서 기분이 좋았다 :)
# Table of Contents

- [1. Session (세션)](#1-session-세션)
  - [A. 예](#a-예)
    - [a. server에 최초 Request](#a-server에-최초-request)
    - [b. client cookie에 저장된 session id를 통해 server에 request](#b-client-cookie에-저장된-session-id를-통해-server에-request)

---

# 1. Session (세션)

> 세션은 클라이언트에게 세션 ID만 주고, 다음에 올때 세션 ID를 서버에게 넘기면, 클라이언트가 준 ID 정보를 가지고 서버에 저장된 기록을 찾아서 가공하고 응답한다.

- 클라이언트에게 세션 ID 정보만 [[Cookie]](http://github.com/mildsalmon/Study/blob/Network/Network/docs/Cookie.md)로 전달하고 세션들의 특성은 서버가 관리한다.
- 요즘은, 악용될만한 정보들은 모두 세션으로 관리한다.

## A. 예

### a. server에 최초 Request

- Session이 없음

![](/bin/Network_image/network_3_22.png)

![](/bin/Network_image/network_3_23.png)

Set-Cookie에 쿠키가 아닌, session id를 준다.

### b. client cookie에 저장된 session id를 통해 server에 request

![](/bin/Network_image/network_3_24.png)

![](/bin/Network_image/network_3_25.png)

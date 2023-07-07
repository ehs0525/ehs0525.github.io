---
layout: single
title: "[WebRTC] WebRTC 이론"
categories: WebRTC
tag: [WebRTC, STUN, TURN, SDP, ICE Candidates]
author_profile: false
search: true
use_math: false
sidebar:
  nav: "counts"
---

# WebRTC란?

![image-20230705205111918]({{siteURL}}/images/2023-07-05-webrtc-theory/image-20230705205111918.png)

WebRTC는 브라우저 또는 애플리케이션(모바일) 간의 peer-to-peer 통신을 가능하게 하는 오픈 소스 프로젝트입니다. 즉, WebRTC를 사용하면 필요한 플러그인이나 프레임워크 없이 웹을 통해 모든 종류의 미디어(비디오, 오디오 및 데이터 등)를 교환할 수 있습니다.

WebRTC를 사용하는 응용프로그램 : Google Meet/Google Hangouts, Facebook 메신저, Discord

두 사용자 간의 직접적인 연결을 위해, Signaling Server를 통해 필요한 데이터를 교환해야 합니다.



## Signaling Server란?

- Signaling Server는 WebRTC 관련 작업을 수행하지 않습니다.

- Signaling Server는 사용자 간의 직접 연결을 설정하는 데 필요한 정보를 교환하는 데 도움이 됩니다.

- 신호 전달을 위해 WebSocket에서 XMLHttpRequest에 이르기까지 원하는 모든 것을 사용할 수 있습니다.

WebRTC 피어 연결이 수립되기 전에, 우리는 Signaling Server를 통해 몇 가지 데이터를 교환해야 합니다. 그것은 우리의 인터넷 연결 세부사항, 사용하고 있는 코덱에 대한 정보, 브라우저에 대한 정보, 통신할 수 있는 방법과 관련된 데이터가 될 것입니다.



## STUN Server란?

연결을 수립하기 전에 인터넷 연결 세부 정보를 가져와야 합니다. 이는 STUN 서버 덕분에 할 수 있습니다.

연결을 수립하기 전에, STUN 서버에 우리의 인터넷 연결 세부 정보를 요청하면 우리에게 return해줄 것이고, 우리는 그 정보를 Signaling Server를 통해 callee와 교환할 수 있습니다.

- **STUN(Session Traversal Utilities for NAT)**: 클라이언트가 자신의 public IP 주소와 지원하는 NAT 유형을 검색할 수 있습니다. 이 정보는 미디어 연결을 수립하는 데 사용됩니다.

- 15-20%의 경우 STUN 서버가 실패할 수 있습니다. 이때, 피어 간 연결을 설정하기 위해 TURN 서버가 필요합니다.

  

## TURN Server란?

지금까지 연결을 수립하기 위해 Signaling Server를 통해 필요한 데이터를 교환해야 한다는 것을 배웠습니다. 또한 STUN Server에게 우리의 인터넷 연결 세부사항을 물어보고 상대방과 데이터를 교환해야 합니다. 그렇다면 TURN Server란 무엇일까요?

- TURN Server는 Traversal Using Relay NAT의 약어이며, 네트워크 트래픽을 전달하기 위한 프로토콜입니다.
- STUN Server가 실패할 경우 TURN Server가 사용됩니다.
- TURN Server는 피어 간의 연결을 수립하는 보조 도구로 사용됩니다.
- 서버를 통과하는 트래픽으로 인해 발생할 수 있는 비용 때문에 TURN Server는 public하지 않습니다.

정리하자면,

STUN Server는 발신자와 수신자 측 모두에게 각자의 인터넷 연결 세부 정보를 반환하고 이를 Signaling Server를 통해 교환할 것입니다. 그리고 나중에 두 사용자 간의 연결을 설정하려고 하면 사용자가 아닌 결함이 있는 라우터일 수 있기 때문에 가능하지 않을 수 있습니다. 이제 TURN Server가 나옵니다.

TURN Server는 단지 이동할 트래픽을 라우팅할 뿐이므로 먼저 사용자가 데이터를 TURN Server에 전달하면 TURN Server는 해당 데이터를 상대방에게 전달합니다. 그 데이터는 오디오, 비디오 스트림일 수 있습니다. 반대 방향도 마찬가지로 이루어집니다. TURN Server는 해당 데이터를 상대 측에 전달만 할 뿐, 그것을 사용하여 아무것도 수행하지 않는 것에 유의하십시오.



## SDP란?

WebRTC 직접 연결을 설정하기 전에 데이터를 교환해야 합니다. 우리가 교환해야 할 첫 번째 데이터는 **SDP**입니다.

- SDP는 Session Description Protocol의 약어로, 세션 공지 및 세션 초대를 위해 멀티미디어 통신 세션을 설명하는 형식입니다.
- 미디어 폼의 컨텐츠 그 자체를 위해서 제공된 것은 아니지만, 다양한 오디오 및 비디오 코덱, 소스 주소, 오디오 및 비디오의 시간 정보의 피어 간 협상에 사용되며 제안 및 수락(Offer & Answer) 모델로 동작합니다.

발신자는 Signaling Server를 통해 수신자에게 전달되는 WebRTC Offer를 생성하고, 수신자는 해당 Offer를 저장하고 SDP 정보가 포함된 Answer을 준비합니다. 수신자 측에서 Signaling Server를 통해 해당 정보를 발신자에게 다시 전송합니다. 따라서 두 사용자 모두 자신의 SDP 정보와 연결하고자 하는 사용자의 SDP를 알 수 있습니다.

따라서 두 사용자 간의 직접 연결을 설정하기 위해 SDP 교환은 필요한 단계입니다. 그리고 우리가 교환해야 할 두 번째는 인터넷 연결 세부사항(**ICE Candidate**)입니다.



## ICE Candidates란?

직접 연결을 수립하려면 발신자와 수신자 간에 SDP 정보를 교환해야 합니다. 그 외 인터넷 연결 세부사항들은 STUN Server로부터 얻고 있는 **ICE Candidates**입니다. 그렇다면 ICE candidates는 무엇일까요?

- 피어는 미디어에 대한 정보를 교환할 뿐만 아니라(위의 Offer/Answer 및 SDP에서 설명함) 네트워크 연결에 대한 정보도 교환해야 합니다. 이를 **ICE candidate**라고 하며 피어가 직접 또는 TURN Server를 통해 통신할 수 있는 방법을 자세히 설명합니다. 일반적으로 각 피어는 가장 적합한 후보를 먼저 제안하고 더 나쁜 후보를 향해 내려갑니다. candidates는 UDP가 이상적이지만(더 빠르고 미디어 스트림이 비교적 쉽게 interruption으로부터 복구할 수 있기 때문에) ICE 표준은 TCP candidates도 허용합니다.

정리하자면,

우선 우리는 SDP 정보를 교환해야 합니다. 그리고나서 인터넷 연결 세부사항을 교환할 것입니다. 그래서 STUN Server에서 ICE candidates를 얻습니다. 그 정보를 포함해서 말이죠. 이 정보들은 Signaling Server를 통해 교환이 되고, 수신자 측도 마찬가지로 동작합니다.



# 피어간 직접 연결을 수립하는 방법

![image-20230705205111918]({{siteURL}}/images/2023-07-05-webrtc-theory/how-to-establish-direct-connection-between-peers.png)

두 사용자 간에 직접 연결을 설정하려면 첫 번째 단계는 ==**1. WebRTC Offer**==이며, 이 offer은 발신자 측에서 작성됩니다. 이 Offer는 Signaling Server로 전달될 것이고 Signaling Server는 수신자 Kate에게 전달합니다.

Kate는 WebRTC Offer을 수락하고, 이를 remote description으로 저장한 후, SDP 정보를 포함한 ==**2. WebRTC Answer**==을 준비합니다. 왜냐하면 그 offer에는 발신자 John의 SDP 정보가 포함되어 있기 때문입니다. 이 answer는 Signaling Server를 통해 발신자 John에게 다시 전송됩니다.



이제 성공적으로 SDP 정보가 교환됐지만, 여전히 충분하지 않습니다. 왜냐하면 우리가 해야 할 두 번째 일은 인터넷 연결 세부사항과 누군가가 우리와 연결할 수 있는 방법을 교환하는 것이기 때문입니다.

이제 STUN Server에 ==**3. ICE Candidates를 요청**==합니다. STUN Server가 발신자 측과 착신자 측 모두에서  ==**4. ICE Candidates를 반환**==합니다.

ICE Candidates를 얻으면, SDP 정보를 교환할 때와 마찬가지로 Signaling Server를 통해 그들을 ==**5. 상대 쪽으로 전달**==할 것입니다.



발신자가 WebRTC Offer을 생성하는 즉시 ICE Candidates를 요청한다는 것을 명심하시기 바랍니다.

이제 SDP 정보(브라우저에 대한 자세한 정보)와 ICE Candidates(인터넷 연결에 대한 정보 및 다른 사용자가 우리에게 연결할 수 있는 방법)를 성공적으로 교환했습니다. 다음 단계로, ==**5. 직접 연결이 작동을 시작**==해야 합니다. 그러면 비디오 스트림, 오디오 스트림 또는 RTC 데이터 채널을 사용하는 데이터 등을 전송할 수 있습니다.

그러나 때때로 한 쪽이 다른 사용자와 직접 연결할 수 없으면 해당 ==**5-1. 연결이 작동하지 않을 수 있습니다**==. 이때는 경로를 그냥 통과할 TURN Server가 필요합니다. 따라서 ==**TURN Server의 도움으로 직접 연결이 수립**==됩니다.

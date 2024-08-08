## 선착순 시스템 설계

`요구사항`

1. 아이폰 출시 이벤트가 진행되는데 물량이 1000개.
2. 12시에 오픈하여 선착순 1000명에게만 살수 있게 한다.
3. 예상 이벤트참가 인원은 수십만명이다.
4. 안전하게 이벤트를 진행할 시스템을 디자인하라.

## 시스템 설계도

![Pasted image 20240808175040](https://github.com/user-attachments/assets/6b06333e-e091-489a-9939-091d939d5d10)

## 구현 방법

- 선착순 요청은 TimeAttack Producer가 먼저 받는다
- Producer는 먼저 Redis에 DECR 커맨드로 재고가 0이하인지 확인 한다.
- if문 분기 처리로 0이하이면 429(Too Many Request)로 응답한다
- Producer는 재고가 남아 있다면 RabbitMQ로 userId를 전달한다
- Producer는 RabbitMQ로부터 ACK를 확인하고 RabbitMQ는 Consumer로부터 ACK를 확인한다.
- Consumer는 메시지를 받아 이후 결제, 저장등의 로직을 시행하고 로직이 실패했다면 Failed Event에 기록을 남겨 나중에라도 처리할 수 있게 한다

## 왜 Redis인가

재고를 감소시키는 로직에 락을 사용할 시 재고읽기 - 감소- 저장으로 락 범위가 커지고 성능이 저하될 가능성이 있습니다. 모두 DB커넥션이 필요하기 때문입니다. Redis의 DECR 커맨드는 읽기-감소-저장을 모두 하나의 실행으로 처리하고 싱글쓰레드로 동작하기 때문에 성능과 재고의 원자성을 둘 다 지킬 수 있어 선택하였습니다.

## 왜 메시지 기반인가

Redis로 1000개의 요청만 처리할 수 있게 하더라도 이 후 로직에 DB커넥션이 필요하게 되면 길어지는 처리 시간때문에 timeout이 발생할 수 있기 때문입니다. 따라서 DB를 거치지 않고 선착순에 든 userId를 메시지로 보고 이를 다른 시스템에서 처리하게 하면 처리량을 조절할 수 있기 때문입니다.

## 왜 RabbitMQ인가

메시지의 신뢰성 있는 데이터 전달과 처리량 조절을 위해서입니다.

1. RabbitMQ에서 Publisher Confirm, Consumer Confirm을 설정하여 Producer <- RabbitMQ, RabbitMQ <- Consumer로 메시지 전달을 받았음을 ack로 알리게 함. 실패시 로그라도 남길 수 있음
2.  DB콜을 시행할 Consumer에서 메시지를 적당량 처리할 수 있게 prefetchCount를 설정
3.  consumer에서 nack만 보내면 일정 횟수 이상 retry하고 Deadletter Queue로 메시지를 적재이후 따로 처리할 수 있게 함.

의 방법을 사용할 것 같습니다.

## Consumer는 어떻게 구성되나

RabbitMQ또한 Producer, Consumer사이에서 네트워크 통신을 하므로 ack를 활용해도 메시지가 중복처리될 수 있습니다.(전달 받았는데 ack가 안감 등)

userID를 기반으로 이후 로직이 실행되기 때문에 로직 실행전 DB를 통한 중복확인이 필요할 것입니다. 이 과정과 이후 트랜잭션, 외부 API 호출과 같은 로직에서 실패할 수도 있기 때문에 Failed Message에 실패한 메시지를 저장한다면 나중에 수동으로라도 처리할 수 있을 것입니다.

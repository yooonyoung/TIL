<h3>Working with AWS SQS Standard Queues</h3>

SQS : AWS 에서 가장 먼저 생긴 리소스



메시지 (message)

- XML, JSON 과 같은 형태를 가진 데이터

- 256KB

- 유니코드 사용이 가능

  

큐 (queue)

- 메시지를 담는 공간
- 리전 별로 생성해야 함
- 큐 이름은 모든 리전에서 유일해야 함



배치 API

- 용량이 작은 메시지를 자주 처리하면 과금이 발생하므로
- 메시지를 모아서 동시에 처리



Visibility Timeout

- 메시지를 받은 뒤 특정 시간 동안 다른 곳에서 메시지를 볼 수 없도록 하는 기능
- 여러 서버가 메시지를 처리하는 경우 동일한 메시지를 동시에(중복해서) 처리하는 것을 방지
  - Message Available => Visible => 메시지를 꺼내 볼 수 있는 상태
  - Message in Fight => Not Visible => 다른 곳에서 메시지를 보고 있어 볼 수 없는 메시지

Delay Delivery

- 특정 시간 동안 메시지를 받지 못하게 하는 기능
- Message in Fight 상태



Dead Letter Queues

- 일반적으로 메시지를 받아서 처리하면 메시지를 삭제
- 삭제되지 않고 남아 있는 경우에는 처리 실패 큐(Dead Letter Queues)로 보냄



Long Polling

- 메시지가 있으면 바로 가져오고, 없으면 올 때까지 기다림
- 기본은 20초이고, 1초부터 20초까지 설정이 가능



Short Polling

- 메시지가 있으면 바로 가져오고, 없으면 빠져 나옴
- Receive Message 요청에서 WaitTimeSeconds를 0으로 설정
- Queue 설정에서 ReceiveMessageWaitTimeSeconds를 0으로 설정



![Pasted image 20240906191327](https://github.com/user-attachments/assets/11341d7e-327d-47a0-8713-7257d22da6f0)

### 서버 성능 테스트

성능 테스트는 모든 엔드포인트 /api/places, /api/place/{placeId}, /api/search/{keyword}에 vUser가 요청을 한번씩 보내는 스크립트 작성

부하를 보내는 환경 : Mac M1 (로컬 노트북)
부하를 받는 환경 : GCP e2 Micro (vCPU2, memory 1GB)
부하 툴 : K6

1. 서버의 한계를 알기 위해 vUser300까지 부하를 늘리며 스트레스 테스트 진행

![Pasted image 20240906185602](https://github.com/user-attachments/assets/f673a7b8-5b5a-4544-9ce2-22e98a5e6bfb)

결과 : vUser 100명 이상시 처리량(노란선)이 급격히 느려지고 150이상부터 에러가 발생하기 시작
응답시간 평균 : 5초.  최대 응답시간(실패 시) 3분이 넘음 ㅎㅎ;

2. 100명의 vUser를 계속 수용할 수 있는지 테스트

![Pasted image 20240906185830](https://github.com/user-attachments/assets/19c31114-2e2f-467c-9b21-284906c81974)

결과 : vUser 100명이 유지될때 에러도 같이 나기 시작.  부하를 줄여서 평균 응답시간은 2초 최대는 실패시 1분

JVM 메트릭에서 보면 알 수 있듯이 GC가 발생하고 힙이 비워지면 응답처리량이 같이 줄어드는 패턴을 볼 수 있음.  힙사이즈에 따라 응답시간이 결정되고 있는 패턴을 발견

50vUser로 낮췄을 때도 마찬가지의 패턴. GC이후 힙이 비워지면 응답시간이 빨라지는 패턴 반복 


3. vUser 10명일때

![Pasted image 20240906204946](https://github.com/user-attachments/assets/e861df59-14d6-4c87-a6d1-01f04c1943bc)
![Pasted image 20240906205050](https://github.com/user-attachments/assets/b7fd1853-2012-46a8-a418-a0dfec7dceba)

모든 응답이 1초 내로 이루어 지는 것을 볼 수 있음

이 때의 패턴을 기준으로 잡고 vUser100명을 안정적으로 유지하는 것을 서버 1차 목표로 잡았음

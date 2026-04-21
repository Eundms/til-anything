# Pod Status | Pod Conditions

## Pod Status
- Pending
- Running
- ContainerCreating

## Pod Conditions : 각 단계별 체크리스트
- `PodScheduled` true/false 
    - 스케줄링(어느 노드에 배치될지 결정)된 경우 
    - 리소스 부족, nodeSelector 불일치
- `Initialized` true/false
    - init container 다 끝난 경우 
- `ContainersReady` true/false 
    - 모든 컨테이너가 준비된 경우 
- `Ready` true/false 
    - 트래픽을 받아들일 준비가 된 경우  


### 예시 : Spring 서버 
> ContainersReady=false, Ready=false

1. 컨테이너 실행됨 
2. JVM 뜸 
3. 앱 로딩 중 (DB 연결, 캐시 warm-up)

> ContainersReady=true, Ready=true

4. 서버 완전히 기동됨 

# Probe

## startupProbe : 초기 기동 오래 걸리는 앱 보호 (다른 Probe차단)
- 앱 시작이 느림 -> livenessProbe가 먼저 실패 
- startupProbe가 있는 동안 이것만 체크 -> 성공하면 readiness, liveness 시작 
- 실패 반복하면 컨테이너 재시작

## livenessProbe : 재시작 제어 (컨테이너가 비정상 상태인지 판단)
- 컨테이너 self-healing : 컨테이너 재시작

- 언제 쓰는가 
    - 무한 루프 걸림
    - deadlock
    - 응답 멈춤

- livenessProbe=x
    - kubelet : 컨테이너 강제 재시작

## readinessProbe : 트래픽 제어 (Service -> Pod 여부 결정)
- 앱이 준비가 아직 안되었을 떄 
- DB 연결 기다리는 중
- 캐시 워밍업 중
- readinessProbe=x
    - Service -> 이 파드로 트래픽 안 보냄


## 장애 예시
1. Pod 시작
2. readinessProbe x -> 트래픽 안 받음
3. 준비완료 -> readinessProbe O
4. 트래픽 받기 시작 
5. 이후 장애 발생
6. livenessProbe x -> 재시작 

- readinessProbe없이 livenessProbe만 있음 : 준비x 트래픽 들어옴 -> 장애
- livenessProbe 너무 aggressive : 정상인데 계속 재시작 (CrashLoopBackOff)
- readinessProbe 너무 늦기ㅔ true : 트래픽 계속 못받음

### startupProbe 없을 때 문제
앱 시작 느림
→ livenessProbe 실패
→ 재시작
→ 다시 느림
→ 무한 루프 (CrashLoopBackOff)

### readinessProbe 없을 때
앱 아직 준비 안됨
→ 트래픽 들어옴
→ 장애 발생

### livenessProbe 없을 때
앱 hang 상태
→ 계속 살아있는 척
→ 장애 지속


## spring Boot 기준:
- startupProbe → 충분히 길게
- readinessProbe → (/actuator/health/readiness)
- livenessProbe → 너무 민감하게 하지 말 것 (/actuator/health/liveness)
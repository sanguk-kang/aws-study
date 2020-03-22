# ###########################
# * VPC 구성하기
 DOD : VPC 및 서브넷 기본환경 구성 완료 console 확인

## 1. vpc 생성
 ksu-ntw-test-vpc
 10.90.0.0/16

## 2. subnet 생성
### 1) pub
 ksu-ntw-pub-1a-sn
 10.90.10.0/24

 ksu-ntw-pub-1c-sn
 10.90.11.0/24

### 2) pri
 ksu-ntw-mgmt-1a-sn
 10.90.20.0/24

 ksu-ntw-mgmt-1c-sn
 10.90.21.0/24

## 3. igw 생성 (vpc 연결)
 ksu-ntw-igw

## 4. route table 생성 및 맵핑
 ksu-ntw-pub-rt : 3번 IGW 맵핑
 ksu-ntw-pri-rt : 5번 NAT 맵핑

## 5. nat (public 1a 영역 생성)
 ksu-ntw-nat
 
# ###########################

# ###########################
# * 네트워크 구성하기 (ASG 제외)
 DOD : ALB를 통한 pri EC2에 접속하여 화면 접속 확인 (http & https 확인)

## 6. security groups (EC2, ELB 사용)
1) ksu-ntw-alb-sg : 80, 433
2) ksu-ntw-jenkins-sg : 8088, 8081
3) ksu-ntw-mgmt-sg : 22( myIp, vpcIp )

## 7. ec2 (private 영역)
ksu-ntw-jenkins-ec2
ami : ami-07ab3fd16141e82e4

## 8. taget group (ELB 사용)
ksu-ntw-jenkins-tg8088 : 8088
ksu-ntw-mgmt-tg22 : 22

## 9. elb
### 1) alb : ksu-ntw-jenkins-alb
 - 가용영역 pub

## 10.auto scaling (하위 추가 Study)
ksu-ntw-ui-lc
ksu-ntw-jenkins-asg

# ###########################

# ###########################
# * 추가 작업 - auto scaling
~~~
 launch configuration - Auto scaling 구성을 위한 인스턴스
 Auto scaling group 생성
 DOD : ALB를 통하여 추가한 UI 서브넷 확인(8080 포트)
~~~

## 11. 추가 subnet
### 1) pri sub
 ksu-ntw-ui-1a-sn
 10.90.50.0/24

 ksu-ntw-ui-1c-sn
 10.90.51.0/24

## 12. route 테이블 서브넷 추가 연결
 기존 ksu-ntw-pri-rt에 subnet 맵핑

## 13. launch configuration -> 이미지 생성a
 Auto scalling을 위한 이미지 생성
 ksu-ntw-ui-lc

## 14. Auto scaling group -> EC2 동시 생성
 ksu-ntw-jenkins-asg
 ksu-ntw-ui-ec2

## 15. alb 연결 (8080테스트)
### 1) security groups의 ksu-ntw-alb-sg에 8080 추가
### 2) sg ui 생성
 ksu-ntw-ui-sg : 8088, 8081
### 3) tg 생성
 ksu-ntw-ui-tg8088 : 8088
### 4) 기존 ALB 리스너 추가
  HTTP : 8080 -> ksu-ntw-ui-tg8088

### 5) ksu-ntw-ui-tg8088 에 인스턴스 연결
 14번에 생성한 ksu-ntw-ui-ec2 연결
 
# ###########################

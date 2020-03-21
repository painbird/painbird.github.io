---
title: Google Cloud Platform 영구 무료 서버
description: 
header: 
---

#### Google Cloud Platform의 15개 서비스를 영구 무료 한도를 제공

구글은 2017년 3월 10일 Google Cloud Next 17의 3일째 기조 강연에서 15개 서비스의 영구 무료 쿼터를 발표.
영구 무료 한도 대상 서비스 이름과 월 제한은 다음과 같다(제품에 따라서는 아래 외에도 제한이 있다).

- Google Cloud Engine(미국 지역의 f1-micro 1 인스턴스 및 HDD 30GB)  
- Google Cloud Storage(5GB)  
- Google Cloud Datastore(1GB, 읽기 5만회, 쓰기 2만회, 삭제 2만건)  
- Google App Engine(24 인스턴스시간/일, Cloud Storage 5GB, 공유 memcache)  
- Google Pub/Sub(메시지 10 GB)  
- Google Cloud Functions(호출 200 만회)  
- Google Container Engine(5 노드 이하, 다만 가상 머신 과금은 있다)  
- Google Cloud Container Builder(120 빌드 시간/일)  
- Google Cloud Source Repositories(개인 호스팅 1GB)  
- Google Stackdriver(로그 5 GB, 보유 기간 7일)  
- Google BigQuery Public Datasets(쿼리 1TB)  
- Google Cloud Vision API  
- Google Cloud Speech API  
- Google Cloud Natural Language API  
- Google Cloud Shell(무제한, 영속 스토리지 5GB 붙음)  

##### 1. GCP 계정 생성 (Credit 카드 인증)

[https://cloud.google.com]()
1년 $300 크레딧 {: .right}

- 프로젝트명 변경 - "프로젝트 설정" 메뉴  
![](https://i.postimg.cc/BbzrCxFD/gcp1.png)

- 메뉴 고정  
<p class="left">
<img src="https://i.postimg.cc/SRj08Znk/gcp2.png" width="75%"/>
</p>

##### 2. 평생무료 VM 인스턴스

무료 설정 기준 -  [https://cloud.google.com/free/docs/gcp-free-tier?hl=ko#always-free-usage-limits]()  

> 리전 : 오리건, 아이오와, 사우스캐롤라이나  
> 영역 : 리전 선택 후 자유  
> 머신 : N1 시리즈 f1-micro  
> OS : 자유  
> 부팅디스크 유형 : 표준 영구 디스크 / 30GB

![](https://i.postimg.cc/Kv6XkDxh/gcp3.png)
![](https://i.postimg.cc/k4T32rXT/gcp4.png)

##### 3. IP 고정

!{width:85%}gcp10.png!

!{width:85%}gcp11.png!

##### 4. SSH 연결

!{width:85%}gcp20.png!

1) 브라우저 연결

!{width:85%}gcp21.png!

2) SSH 클라이언트 연결

https://cloud.google.com/compute/docs/instances/connecting-advanced#thirdpartytools

2-1) 로컬 - ssh 키 생성
https://cloud.google.com/compute/docs/instances/adding-removing-ssh-keys#createsshkeys
<pre>
ssh-keygen -t rsa -f ~/.ssh/키이름 -C "구글 계정"
</pre>

!gcp15.png!

2-2) 로컬 - ssh 키 복사
<pre>
cat ~/.ssh/gcp-rsa.pub
</pre>
!gcp16.png!

2-3) 서버 - 메타데이터 SSH 키에 추가

!gcp17.png!

!gcp18.png!

2-4) 로컬 터미널에서 연결
<pre>
ssh -i "키파일" "구글 계정 ID"@"VM 외부 IP주소"
</pre>

!gcp19.png!


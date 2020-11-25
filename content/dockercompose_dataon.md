docker-compose 개발/운영 환경세팅
===

방향성
---
오픈메이트온에서는 **트렌드온, 세이프온, 라이트온, 스마트온, 데이트온(출시예정)** 등 다양한 서비스를 운영하고 있다. 그러면서 기존 서비스 운영환경은 상황에 따라 다르게 독립적으로 관리하고 있는 중이다. 운영서버 인프라 구조는 Iaas형태를 띄고 있으며 2가지 종류로 크게 나눌 수 있을 것 같다.  
1. ubuntu + Docker(nginx + Sprigin framework + postgreSQL)
2. ubuntu + Docker(nginx + nuxtJS + dJango + postgreSQL)

개발서버 인프라 구조는 On-Premise(요즘 라이젠 사양이 어마무시...)형태를 가지며, 모든 애플리케이션을 도커기반으로 구성하고 있다.  
**But,**  이렇게 운영환경이 제각기 달라 현재 CI/CD(지금 만들고 있는 중...)가 없는 상태에서 배포과정이 매우 복잡하기 때문에 통일성을 가지기 위해서 모든 애플리케이션을 docker-compose 기반으로 관리하려고 한다. 추후에 Gitlab CI/CD를 구축할 때 Kubernates까지 생각하고 있기에 docker를 선택하게 되었다. 


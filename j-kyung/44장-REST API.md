# 44장-REST API
- REST : HTTP 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍처
- REST API : REST를 기반으로 서비스 API 구현한 것

## 44.1 REST API의 구성
- 자원, 행위, 표현의 3가지 요소로 구성
    - 자원 - 자원 - URI
    - 행위 - 자원에 대한 행위 - HTTP 요청 메서드
    - 표현 - 자원에 대한 행위의 구체적 내용 - 페이로드

## 44.2 REST API 설계 원칙
- URI는 리소스를 표현하는 데 집중
- 행위에 대한 정의는 HTTP 요청 메서드를 통해 함
    - GET, POST, PUT, PATCH, DELETE 5가지 요청 메서드로 CRUD 구현

## 44.3 JSON Sever를 이용한 REST API 실습
1. JSON Sever 설치
2. db.json 파일 생성
3. JSON Sever 실행
4. GET 요청
5. POST 요청
6. PUT 요청
7. PATCH 요청
8. DELETE 요청
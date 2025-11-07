# Implementation requirements
서비스 “Yummy Pass”를 구현하기 위한 요구사항은 두 가지로 나눌 수 있다. 각각의 요소는 (1) Software Environment (2) Hardware Environment로, 아래에 같이 기술한다.

## **(1) Software Environment**

Software Environmen란, 서비스를 동작시키는 시스템을 구현하기 위하여 사전에 구성해야 할 소프트웨어 기반 환경을 가리킨다. Yummy Pass는 모바일,데스크톱을 포함하여 인터넷 접속이 가능한, 다양한 환경에서 사용할 수 있는 시스템 설계를 목표로 한다. 이와 같은 연유로 서비스를 구축하려면 디자인 단계와 구현 단계에서 여러 도구를 활용해 적절한 개발 환경을 조성해야 한다. 원활한 개발 프로세스를 위하여 소프트웨어 조건은 아래와 같이 설정했다.

| **항목** | **설명** |
| --- | --- |
| **Java Development Kit (JDK)** | 본 프로젝트에서는 JDK 17 버전을 사용한다. JDK 17은 LTS 버전으로 장기간 지원과 안정성을 제공할 수 있는 버전이다. 해당 버전은 안정성이 검증되어 프로덕션 환경에 적합하다.  |
| **Spring Boot** | Spring Boot 3.5.5 버전이 필요하다. 프로젝트를 초기화 할 때는 Spring Initializer를 이용하여 손쉽게 설정할 수 있으며, 필요한 의존성을 선택할 수 있다. |
| **빌드 자동화 도구** | 프로젝트 관리 및 빌드를 위해 Gradle을 사용한다. Spring Initializer에서 선택 가능하며, 빌드 관리에 유용하다.  |
| **통합 개발 환경**
**(IDE)** | 백엔드의 경우 IntelliJ IDEA 또는 Visual Studio Code (VS Code)를 사용하고, 프론트엔드의 경우 VS Code IDE를 사용한다. 이와 같은 도구들은 코드 작성, 디버깅, 버전 관리 연동 등을 효율적으로 수행할 수 있게 해준다. |
| **프론트엔드 프레임 워크** | React 18 이상을 사용하여 사용자 인터페이스를 구축한다. 컴포넌트 기반 아키텍처를 통해 재사용 가능하고 유지보수가 쉬운 UI를 개발하며, JavaScript(ES6+)를 기반으로 작성한다. Node.js 환경에서 실행되며, NPM을 통해 필요한 패키지를 관리한다. |
| **데이터베이스** | Oracle에서 제공하는 오픈소스 관계형 데이터베이스인 MySQL과 AWS에서 제공하는 다양한 용도로 사용 가능한 S3을 활용한다. 이미지를 제외한 데이터들은 MySQL에서, 이미지 파일은 S3를 통해 관리한다. MySQL은 8.0 이상의 버전을 활용하는 것으로 한다. |
| **브라우저** | Chrome, Naver Whale 등 현대적인 웹 표준을 지원하는 브라우저를 활용한다. |
| **소스 버전 관리** | Git을 사용하여 소스 코드의 버전 관리를 수행한다. 프로젝트 폴더를 Git 저장소로 초기화함으로써, 코드 변경 이력 추적, 협업, 백업 등이 가능해진다. |
| **서버 배포 형식** | Amazon에서 제공하는 클라우드 컴퓨팅 플랫폼인 AWS의 EC2를 활용하여 서버를 배포한다. 코드가 변경되어 푸시될 때마다 자동으로 빌드와 배포 과정을 수행할 수 있도록 GithubAction을 사용한다. 서버는 Docker를 이용해 컨테이너 형태로 배포한다. 이를 사용하면 환경까지 통째로 묶어서 배포할 수 있다. 데이터는 MySql을 통해 관리하며 안정적인 데이터베이스를 구축한다.<br>서버에는 NGINX를 설치해 웹 서버 및 리버스 프록시 역할을 수행하도록 한다. SSL 인증서를 적용하여 HTTPS 통신을 구축한다. 이를 통해 사용자와 서버 간 데이터가 암호화되어 안전하게 전송되도록 한다. |

 백엔드의 Spring Boot Initializer에서 프로젝트를 위한 설정을 진행할 때 다음 목록의 의존성을 선택한다.

| **항목** | **설명** |
| --- | --- |
| **Spring Web** | 웹 애플리케이션을 구축하기 위한 핵심 모듈로, RESTful API 구현을 지원한다. |
| **Spring Data JPA** | 데이터베이스와 연동하기 위한 ORM(Object Relational Mapping) 모듈로, Entity 기반의 CRUD를 단순화한다. |
| **MySQL Driver** | MySQL 데이터베이스와의 연결을 위한 JDBC 드라이버. 런타임 환경에서만 동작한다. |
| **Lombok** | 반복되는 Getter, Setter, Constructor 코드를 줄이기 위한 어노테이션 기반 코드 생성 도구다. |
| **Spring Boot DevTools** | 코드 변경 시 자동 재시작 등 개발 편의 기능을 제공하는 도구다. |
| **Spring Boot Starter Validation** | 입력값 검증을 위한 유효성 검사(Validation) 모듈. @Valid, @NotNull 등의 어노테이션 제공한다. |
| **Spring Security** | 인증(Authentication) 및 권한(Authorization) 관리를 위한 보안 모듈이다. |
| **Spring OAuth2 Client** | Google, Naver, Kakao 등 외부 OAuth2 Provider와 연동을 지원하는 클라이언트 모듈이다. |
| **JWT (io.jsonwebtoken)** | JWT(Json Web Token) 생성, 서명, 파싱을 지원하는 라이브러리. Access Token 발급 및 검증에 사용된다. |
| **SpringDoc OpenAPI** | Swagger 기반의 자동 API 문서화 도구. /swagger-ui.html로 API 명세를 확인할 수 있다. |
| **ZXing (core, javase)** | QR 코드 생성 및 디코딩을 위한 Google ZXing 라이브러리. QR 식권 기능 구현에 사용한다. |
| **Spring Cloud AWS / AWS SDK for S3** | AWS S3 연동을 위한 모듈로, 파일 업로드 및 다운로드 기능을 제공한다. |
| **JUnit Platform** | 단위 테스트 및 통합 테스트 수행을 위한 테스트 프레임워크다. |

 프론트엔드의 React 세팅에서는 다음 목록의 의존성을 선택한다.

| **항목** | **설명** |
| --- | --- |
| **react** | React 핵심 라이브러리로 UI를 컴포넌트 기반으로 구성할 수 있게 해준다. |
| **react-dom** | React 컴포넌트를 실제 DOM에 렌더링하기 위한 패키지이다. |
| **react-router-dom** | React에서 라우팅(페이지 전환)을 처리하는 라이브러리이다. |
| **@testing-library/react** | React 컴포넌트를 사용자 관점에서 테스트할 수 있게 도와주는 라이브러리이다. |
| **@testing-library/jest-dom** | Jest 테스트 환경에서 사용할 수 있는 DOM 관련 커스텀 매처를 추가해주는 라이브러리다. |
| **@testing-library/user-event** | 사용자 상호작용(클릭, 입력, 키보드, 드래그 등)을 시뮬레이션하는 유틸이다. |
| **@testing-library/dom** | DOM 테스트의 기본 유틸리티 세트이다. |
| **react-scripts** | Create React App(CRA)의 핵심 빌드 스크립트로 프로그램 실행을 위해 사용된다. |
| **web-vitals** | 웹 성능 지표(LCP, FID, CLS 등)를 측정하는 도구이다. |

## **(2) Hardware Environment**

Hardware Environment는 시스템과 개발 도구가 원활히 동작할 수 있도록 개발자에게 요구하는 물리적 환경을 가리킨다. 서비스 “Yummy Pass”를 개인용 노트북을 기반으로 개발하기 때문에, 다음과 같은 하드웨어 환경이 필요하다.

| **항목** | **설명** |
| --- | --- |
| **CPU** | Intel Core i5-1135G7 (11세대) 2.40GHz 이상의 프로세서, 또는 Apple M4 4코어 Everest 모델의 프로세서를 요구한다. 이는 IntelliJ와 같은 통합 개발 환경(IDE), 빌드 툴, 데이터베이스 서버 등을 동시에 실행해도 성능 저하 없이 개발 작업을 수행할 수 있다. |
| **RAM** | 최소 8GB 이상을 지원해야 하며, 16GB 이상을 권장한다. 이러한 성능은 소스 코드를 작성한 후 컴파일, 애플리케이션의 실행, 로컬 환경에서 브라우저 테스트, 가상 데이터베이스 구동 등을 원활히 처리하기 위하여 필수적이다. |
| **스토리지** | SSD 217GB 이상의 저장 공간이 필요하다. 이는 프로젝트 파일, 로컬 저장소(Git), 로그 및 캐시 파일 등을 원활히 저장하기 위해 요구하는 적정 여유 공간에 해당한다. |
| **운영체제** | Windows 11 Home, macOS Sequoia (15.0) 이상, 또는 Ubuntu 20.04 이상의 운영체제 등 편의에 맞추어 활용한다. Spring Boot, 리액트와 같은 개발 도구는 일반적으로 멀티 플랫폼을 지원하므로, 사용자의 선호와 환경에 따라 선택이 가능하다. |
| **인터넷 환경** | 라이브러리 및 의존성 다운로드, Git을 활용한 소스 버전 관리, Spring Initializer를 이용한 프로젝트 생성 등 초기 작업에 해당하는 절차 대다수가 인터넷 연결이 되어있다는 전제 하에 진행된다. |
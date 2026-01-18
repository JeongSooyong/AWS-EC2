AWS EC2 인스턴스 생성 및 Spring Boot 애플리케이션 배포 가이드
본 가이드는 AWS EC2 인스턴스를 생성하고, SSH 접속을 통해 Spring Boot 애플리케이션을 배포하는 과정을 단계별로 설명합니다.

1. AWS EC2 서비스 접속
AWS 콘솔에 로그인 후, 검색창에 EC2를 입력하거나 서비스 목록에서 EC2를 클릭하여 EC2 대시보드로 이동합니다.

EC2 서비스 접속

2. 새 인스턴스 시작
EC2 대시보드 왼쪽 메뉴에서 인스턴스를 클릭한 뒤, 인스턴스 시작 버튼을 클릭하여 새로운 인스턴스 생성을 시작합니다.

인스턴스 시작

3. 인스턴스 설정 구성
새 인스턴스 시작 페이지에서 다음 항목들을 설정합니다.

이름: 인스턴스를 식별할 이름을 입력합니다. (예: MyNetflixClone-Server)
애플리케이션 및 OS 이미지 (Amazon Machine Image - AMI): Ubuntu Server 22.04 LTS (프리티어 사용 가능)를 선택합니다.
인스턴스 유형: t2.micro 또는 t3.micro (프리티어 사용 가능)를 선택합니다.
키 페어 (로그인): 기존 키 페어를 선택하거나 새로 생성합니다. (자세한 내용은 4단계에서)
네트워크 설정: SSH 트래픽 허용, HTTP 트래픽 허용 등을 체크하고 새 보안 그룹 생성을 선택합니다.
인스턴스 설정 (1) 인스턴스 설정 (2) 인스턴스 설정 (3)

4. 로그인용 키 페어 생성 및 보관
EC2 인스턴스에 SSH로 접속하기 위한 키 페어를 생성합니다. 키 페어 이름을 입력하고 생성 버튼을 클릭하면 .pem 파일이 다운로드됩니다. 이 파일은 인스턴스 접속에 필수적이므로 안전하게 잘 보관해야 합니다.

키 페어 생성

5. 인스턴스 시작 및 대기
모든 설정을 마친 후 페이지 하단의 인스턴스 시작 버튼을 클릭합니다. 인스턴스 생성 및 초기화에는 몇 분 정도 시간이 소요됩니다.

인스턴스 시작 버튼

6. 생성된 인스턴스 확인 및 퍼블릭 IP 주소 기록
인스턴스가 성공적으로 생성되면 인스턴스 목록에서 확인할 수 있습니다. 인스턴스를 선택하고 **퍼블릭 IPv4 주소**를 기록해 둡니다. 이 주소는 SSH 접속 및 웹 애플리케이션 접근에 사용됩니다.

생성된 인스턴스 확인

7. SSH 접속 및 서버 프로그램 목록 갱신
로컬 PowerShell 또는 Git Bash 터미널에서 SSH 명령어를 사용하여 EC2 인스턴스에 접속합니다.

bash


ssh -i "C:/Users/Administrator/Downloads/your-key-pair.pem" ubuntu@YOUR_PUBLIC_IP_ADDRESS
접속 후, 서버의 설치 가능한 프로그램 목록을 최신으로 갱신합니다.

bash


sudo apt update
sudo apt update

8. Java 런타임 환경 설치
서버가 Java 코드를 실행할 수 있도록 Java Development Kit (JDK)를 설치합니다.

bash


sudo apt install openjdk-17-jdk -y
설치 후, java -version 명령어를 통해 정상적으로 설치되었는지 확인합니다.

bash


java -version
Java 설치 및 확인 Java 버전 확인 결과

9. Git 설치
GitHub 리포지토리의 코드를 가져오기 위해 Git을 설치합니다.

bash


sudo apt install git -y
Git 설치

10. GitHub 프로젝트 클론
본인의 GitHub 프로젝트 코드를 EC2 인스턴스로 복사(클론)합니다.

bash


git clone YOUR_GITHUB_REPOSITORY_URL
Git 클론

11. 프로젝트 폴더로 이동
Git 클론 후 생성된 프로젝트 폴더로 이동합니다.

bash


cd your-project-name
폴더 이동

12. Maven 프로젝트 빌드 (JAR 파일 생성)
Maven 프로젝트의 경우, 이전 빌드 결과를 삭제하고 실행 가능한 .jar 파일을 생성합니다.

bash


sudo apt install maven -y # Maven이 설치되어 있지 않다면 먼저 설치
mvn clean package
Maven 빌드

13. JAR 파일 확인
빌드 후 target 폴더로 이동하여 생성된 .jar 파일을 확인합니다.

bash


ls target
target 폴더 내용 확인

14. target 폴더로 이동
bash


cd target
target 폴더로 이동

15. Spring Boot 애플리케이션 백그라운드 실행
nohup 명령어를 사용하여 터미널을 종료해도 애플리케이션이 계속 실행되도록 하고, 로그를 app.log 파일로 저장하며, 백그라운드(&)로 실행합니다.

bash


nohup java -jar your-application-0.0.1-SNAPSHOT.jar > app.log 2>&1 &
애플리케이션 백그라운드 실행

16. 애플리케이션 로그 확인
tail -f 명령어를 사용하여 애플리케이션의 로그를 실시간으로 확인하고, 정상적으로 실행되었는지 검증합니다.

bash


tail -f app.log
마지막에 Started YourApplication in ... 와 같은 문구가 나오면 정상적으로 애플리케이션이 시작된 것입니다.

로그 확인 로그 확인 결과

이 가이드를 통해 AWS EC2에 Spring Boot 애플리케이션을 성공적으로 배포하시길 바랍니다!

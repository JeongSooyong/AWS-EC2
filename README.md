# AWS EC2 인스턴스 생성 및 Spring Boot 애플리케이션 배포 가이드

본 가이드는 AWS EC2 인스턴스를 생성하고, SSH 접속을 통해 Spring Boot 애플리케이션을 배포하는 과정을 단계별로 설명합니다.

---

## 1. AWS EC2 서비스 접속

AWS 콘솔에 로그인 후, 검색창에 `EC2`를 입력하거나 서비스 목록에서 `EC2`를 클릭하여 EC2 대시보드로 이동합니다.

![EC2 서비스 접속](https://github.com/user-attachments/assets/cdbaee6b-0c57-497a-b3be-574d563a18d5)

## 2. 새 인스턴스 시작

EC2 대시보드 왼쪽 메뉴에서 `인스턴스`를 클릭한 뒤, `인스턴스 시작` 버튼을 클릭하여 새로운 인스턴스 생성을 시작합니다.

![인스턴스 시작](https://github.com/user-attachments/assets/af32f968-1c6e-4abc-b5b3-61cce550a948)

## 3. 인스턴스 설정 구성

새 인스턴스 시작 페이지에서 다음 항목들을 설정합니다.

-   **이름:** 인스턴스를 식별할 이름을 입력합니다. (예: `MyNetflixClone-Server`)
-   **애플리케이션 및 OS 이미지 (Amazon Machine Image - AMI):** `Ubuntu Server 22.04 LTS` (프리티어 사용 가능)를 선택합니다.
-   **인스턴스 유형:** `t2.micro` 또는 `t3.micro` (프리티어 사용 가능)를 선택합니다.
-   **키 페어 (로그인):** 기존 키 페어를 선택하거나 새로 생성합니다. (자세한 내용은 4단계에서)
-   **네트워크 설정:** `SSH 트래픽 허용`, `HTTP 트래픽 허용` 등을 체크하고 `새 보안 그룹 생성`을 선택합니다.

![인스턴스 설정 (1)](https://github.com/user-attachments/assets/f1d771cd-956a-4b8c-b5b3-61cce550a948)
![인스턴스 설정 (2)](https://github.com/user-attachments/assets/dfc82330-b4db-40f6-9e69-8e6fe2b6acdf)
![인스턴스 설정 (3)](https://github.com/user-attachments/assets/243799a2-48c3-4dd3-a751-1c1b06dfef5d)

## 4. 로그인용 키 페어 생성 및 보관

EC2 인스턴스에 SSH로 접속하기 위한 `키 페어`를 생성합니다. `키 페어 이름`을 입력하고 `생성` 버튼을 클릭하면 `.pem` 파일이 다운로드됩니다. 이 파일은 인스턴스 접속에 필수적이므로 **안전하게 잘 보관**해야 합니다.

![키 페어 생성](https://github.com/user-attachments/assets/8aed8e9e-c9e3-4089-bcc9-8b4b025399a1)

## 5. 인스턴스 시작 및 대기

모든 설정을 마친 후 페이지 하단의 `인스턴스 시작` 버튼을 클릭합니다. 인스턴스 생성 및 초기화에는 몇 분 정도 시간이 소요됩니다.

![인스턴스 시작 버튼](https://github.com/user-attachments/assets/83df1668-ce81-49fb-b388-d65204464b40)

## 6. 생성된 인스턴스 확인 및 퍼블릭 IP 주소 기록

인스턴스가 성공적으로 생성되면 인스턴스 목록에서 확인할 수 있습니다. 인스턴스를 선택하고 **`퍼블릭 IPv4 주소`**를 기록해 둡니다. 이 주소는 SSH 접속 및 웹 애플리케이션 접근에 사용됩니다.

![생성된 인스턴스 확인](https://github.com/user-attachments/assets/01746f64-0cfc-405e-b0b8-d32c3f760905)

## 7. SSH 접속 및 서버 프로그램 목록 갱신

로컬 PowerShell 또는 Git Bash 터미널에서 SSH 명령어를 사용하여 EC2 인스턴스에 접속합니다.


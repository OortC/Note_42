1. 프로젝트 개요
	- 가상머신 동작방식
		- 물리자원인 하드웨어의 리소스를 논리자원으로 가지고 오는 추상화 과정을 거치고, 하나의 디바이스로 묶어 사용 가능하게 하는 가상화 과정을 거쳐 가상머신을 구성
	- 가상머신 목적
		- 하나의 하드웨어 위에서 독립적인 여러 가상 환경을 구축하여, 다양한 운영체제나 소프트웨어를 실행 및 관리 할 수 있음, 주로 다른 환경에서의 개발 및 테스트 목적으로 씀
	- CentOS와 Debian의 차이점
		- CentOS
			- RHEL에서 파생된 리눅스 배포판, RHEL과 호환성 높음
			- Red Hat이 아닌 커뮤니티를 통해 지원되어 패치 느림
			- RPM 기반의 패키지 시스템, `yum`이나 `dnf`를 통해 패키지 관리
			- 주로 기업 서버 환경에서 주로 쓰임
		- Debian
			- 자발적인 커뮤니티에서 개발된 리눅스 배포판
			- 레드햇 계열에 비해 사후 지원 및 배포가 느림
			- dpkg 기반의 패키지 시스템, `apt`를 통한 패키지 관리
			- 진입장벽이 낮아 개인 사용자에게 인기가 많음
	- aptitude와 apt의 차이점
		- apt
			- 주로 CLI를 이용하며, 간단하고 직관적인 명령어 사용
			- 기본적인 의존성 해결 가능
		- aptitude
			- CLI 뿐만 아니라 대화형 인터페이스도 지원
			- 패키지 간의 복잡한 의존성 처리 용이
			- 패키지 삭제시 의존성때문에 설치된 다른 패키지도 삭제
	- AppArmor란?
		- 리눅스 커널 수준 보안 모듈 중 하나로, 사용자와 프로세스 간의 보안 강화
		- 특정 어플리케이션 및 프로세스의 시스템 리소스 접근 방식을 제한함으로써 시스템 보안 강화
		- 프로파일이라는 규칙 세트를 통해 응용 프로그램의 동작을 제한
		- 프로파일에는 어떤 파일, 디렉토리, 네트워크 리소스에 접근 가능한지 명시
	- Graphical Environment
		- echo $DISPLAY
2. 기본 설정
	- UFW, SSH 서비스 확인
		- `systemctl status ufw`
		- `systemctl status ssh`
	- OS 확인
		- `uname -a` / `lsb_release -a` / `hostnamectl`
3. 사용자
	- 유저 그룹 확인
		- `id [유저명]`
	- 새로운 유저 생성 및 비밀번호 설정
		- `useradd [유저명]`
		- `passwd [유저명]`
	- 비밀번호 정책 파일 확인
		- `vi /etc/login.defs`
		- `vi /etc/pam.d/common-password`
		- `chage -l [유저명]`
	- 새로운 그룹 생성 및 할당
		- `groupadd [그룹명]`
		- `usermod -aG [그룹명] [유저명]`
	- 비밀번호 정책의 장점 및 단점
		- 장점
			- 일반적인 패스워드에 비해 보안 강화
			- 보안 강화에 따른 안전한 계정 관리 용이
		- 단점
			- 사용자의 계정 관리 편의성 저하
			- 편의성 저하에 따른 관리 부주의 가능성 증가
4. 호스트 및 파티션
	- 호스트명 체크
		- `hostname`
	- 호스트명 영구변경 후 복원
		- `hostnamectl set-hostname [변경명]`
	- 가상머신의 파티션 체크
		- `lsblk`
	- LVM
		- LVM란?
			- `Logical Volume`을 효율적이고 유연하게 관리하기 위한 커널의 한 부분이자 프로그램
			- 직접 물리 스토리지를 사용하는 것보다 다양한 측면에서 유연성을 제공
			- 유연한 용량 및 크기 조정 가능, 편의에 따른 장치 이름 지정 등
		- 동작 방식
			- 블록 장치 전체 또는 파티션들을 LVM에서 사용할 수 있게 PV로 초기화하여 변환.
			- PV는 4MB의 기본 크기를 가진 PE로 구성되어 있음.
			- PV들로 초기화된 장치들을 VG로 통합. VG 안의 공간을 쪼개어 LV로 만들 수 있음
			- LV는 사용자가 최종적으로 다루는 논리적 스토리지. LV를 구성하는 LE는 PE와 1대1로 맵핑됨 
5. SUDO
	- sudo 설치 확인
		- `apt-get list --installed | grep sudo`
	- sudo 그룹 할당
		- `usermod -aG sudo [유저명]`
	- sudo의 가치 및 동작 확인
		- 사용자의 특정 명령을 관리자 권한으로 실행할 수 있게 만드는 도구
		- 보안 및 권한 관리를 위해 사용되며, 일반 사용자가 시스템의 핵심 부분에 접근하지 못하게 함
		- 모든 사용자에게 관리자 권한을 부여하는 것은 위험하게 때문에, 이를 통해 일부 사용자에게만 필요한 작업을 수행할 권한을 부여함으로써, 시스템의 보안을 강화.
		- 불필요한 권한 부여를 줄이고, 보안 문제를 최소화 할 수 있도록 도와줌
	- sudo 규칙 파일 확인
		- `visudo`
		- TTY (TeleTypeWriter)
			- 리눅스 시스템에서 현재 로그인한 콘솔이나 터미널을 뜻함
			- tty를 활성화 시켜줌으로써, sudo 명령어를 어느 터미널에서 사용했는지 확인
	- sudo 명령어 로그 확인
		- `ls /var/log/sudo/...`
		- `cat /var/log/sudo/../../log`
6. UFW (Uncomplicated Firewall)
	- UFW 설치 및 동작 체크
		- `apt search ufw`
		- `ufw status` / `systemctl status ufw`
	- UFW 설명 및 장점
		- 말 그대로, 리눅스에서 사용하는 간단한 방화벽 설정 도구 중 하나
		- 주로 네트워크 보안을 강화하고, 외부에서의 액세스를 통제하기 위해 사용
		- 특정 포트 및 IP 주소의 연결을 제어함으로써, 네트워크 트래픽으로부터 시스템을 보호
		- 간단한 명령 및 사용법으로 사용자 친화적인 방법 제공
	- UFW 규칙 나열
		- `ufw status verbose`
	- UFW 새로운 규칙 추가 및 제거
		- `ufw allow [포트]`
		- `ufw deny [포트]`
7. SSH
	- SSH 설치 및 동작 체크
		- `apt search openssh-server`
		- `systemctl status ssh`
	- SSH 설명 및 장점
		- 원격 접속을 이용하여 터미널 환경을 안전하게 사용될 수 있도록 고안한 프로토콜
		- 모든 통신을 암호화된 형태로 전송하기 때문에, 다른 원격 통신 프로토콜보다 안전함
	- SSH 포트 확인
		- `vi /etc/ssh/sshd-config`
	- SSH 연결 확인
		- `ssh [유저명]@[IP주소] -p [포트]`
8. Monitoring Script
	- 스크립트 코드 확인
		- `vi /root/monitoring.sh`
			- `uname -a` : 현재 시스템의 정보를 출력하는 명령어, -a로 모든 시스템 정보 출력
			- `cat /proc/cpuinfo` : 시스템에서 CPU 정보에 대한 다양한 세부사항 출력
			- `nproc` : 현재 시스템에서 사용 가능한 프로세서의 수를 출력함
			- `free -m` : 시스템의 메모리 사용 및 여유 공간에 대한 정보 출력
			- `df -BM / -BG` : 시스템에서 디스크의 여유 공간 및 사용량에 대한 정보 제공
			- `mpstat` : CPU 사용량을 모니터링하는 명령어, 프로세서의 사용률 및 부하에 대한 통계 제공
			- `who` : 현재 시스템에 로그인한 사용자들에 대한 정보를 제공하는 명령어
				- `-b` : 시스템의 최근 부팅 시간을 보여줌.
			- `lsblk` : 블록 장치에 대한 정보를 보여주는 명령어, 주로 저장장치를 의미
			- `ss` : 소켓 통계를 보여주는 명령어, 시스템에서 현재 활성화된 소켓 및 상태를 표시 
				- `-t` : TCP 소켓에 대한 정보만 출력
			- `hostname -I` : 시스템의 네트워크 인터페이스에 할당된 IP 주소를 출력
			- `ip link` : 시스템에 연결된 네트워크 인터페이스의 정보를 제공하는 명령어
			- `journalctl` : `systemd`의 로그 메세지를 조회하고 표시하는 명령어
	- Cron이란?
		- 일정한 간격으로 프로그램이나 스크립트를 자동으로 실행하기 위한 시간 기반 작업 스케쥴러
		- 작업은 주기적으로 실행되며, 설정한 시간, 주기, 날짜에 맞추어 작업을 실행함
		- 작업 설정은 crontabe 파일을 사용하여 정의함. 이 파일에 실행할 명령 라인과 실행 주기를 정의
	- 스크립트 설정 확인 (Crontab)
		- `crontab -e`
	- 스크립트 주기 변경 및 영구정지
		- `systemctl disable cron`
9. Bonus
	- lighttpd
		- 가벼우면서 빠른 웹서버 소프트웨어
		- 클라이언트로 부터 http 요청을 받아드리고, 웹 페이지를 제공하는 역활을 수행
		- `vi /etc/lighttpd/conf-available/15-fastcgi-php.conf`
			- 파일 내에서 PHP FastCGI 소켓 경로 설정
		 - `lighttpd-enable-mod fastcgi`
			 - Lighttpd가 FastCGI 프로토콜을 지원하도록 함
		 - `lighttpd-enable-mod fastcgi-php`
			 - Lighttpd가 PHP FastCGI를 사용하여 PHP 프로그램을 처리할 수 있도록 함
	- php
		- 서버 측에서 실행되는 프로그래밍 언어로, 주로 웹 개발에 사용됨
		- 동적 웹 페이지를 생성하는데 특화되어 있으며, HTML에 삽입하여 사용할 수 있는 스크립트 언어
	- mariaDB
		- 관계형 데이터베이스 관리 시스템으로, MySQL을 기반으로 만들어진 오픈 소스 데이터베이스 서버
		- `mysql -u root -p`
	- WordPress
		- 오픈소스 컨텐츠 관리 시스템. 웹사이트를 만들고 관리하는데 사용되며, 블로그, 온라인 상점 등 다양한 종류의 웹사이트를 쉽게 만들 수 있도록 설계되어 있음
		- PHP와 MySQL 데이터베이스를 기반으로 함
		- `vi /var/www/html/wordpress/wp-config.php`
	- FTP
		- 파일 전송 프로토콜 중 하나로써, 파일을 컴퓨터 네트워크 상에서 전송하기 위한 표준 네트워크 프로토콜
		- 주로 서버와 클라이언트 간에 파일을 주고 받을 때 사용
		- 21번 포트를 통해 클라이언트와 서버를 연결하고, 연결되면 제어 명령을 이를 통해 주고 받음. 사용자 인증, 명령 전송, 디렉토리 탐색 등 파일 전송에 관련된 명령어를 여기서 처리함
		- 20번 포트를 통해 파일이나 디렉터리 같은 실제 데이터를 전송하는데 사용함.
		- `vi /etc/vsftpd.conf`
		- 서비스인지 확인하는 법
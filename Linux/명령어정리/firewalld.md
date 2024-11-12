# 방화벽 등록/수정/삭제

Linux 서버에서 방화벽을 추가/적용 하는 방법이다. 

RHEL/CentOS 환경에서 사용할 수 있다.

### 1. 방화벽 설치

```bash
yum install firewalld
systemctl start firewalld
systemctl enable firewalld
```

### 2. 방화벽 확인
    
```bash
firewall-cmd --permanent --list-all
```
    

### 3. 방화벽 허용 포트 추가
- 단일 포트

```bash
firewall-cmd --permanent --zone=public --add-port=8080/tcp
```

- 포트 범위

```bash
firewall-cmd --permanent --zone=public --add-port=4000-4100/tcp
```
    

### 4. 방화벽 포트 삭제

```bash
firewall-cmd --permanent --zone=public --remove-port=8080/tcp
```
    

### 5. 방화벽 재구동

```bash
firewall-cmd --reload
```
    

### 6. 방화벽 목록 확인

```bash
firewall-cmd --list-all
```
---
layout: post
title:  "[PHP] CodeIgniter(코드 이그나이터) 시작하기"
date:   2016-05-28
tag:
- PHP
- codeigniter
- framework
comments: true
---

## CodeIgniter(코드 이그나이터)

CI(CodeIgniter)는 PHP 프레임워크 중 많이 알려진 프레임워크이다.
가벼우면서 다양한 기능을 제공하기 때문에 많이 사용되고 있다.
한국 사용자 포럼이 잘 운영되고 있기 때문에 접근하기 쉬운 프레임워크이기도 하다.

### CodeIgniter 한국 사용자 포럼
> http://codeigniter-kr.org


### CI 설치 
CI 설치하기 위해서는 웹 서버(Apache, Nginx..)와 PHP, MySQL(or Oracle)이 필요하다.
웹 서버 구축 후 CI를 설치하도록 하자!

#### -CI 다운로드
직접 프로젝트 폴더에 다운받고 싶을 땐 http://www.codeigniter.com/download에 접속해서 다운로드 해주면 된다.
현재 최신버전은 3.0.6 버전이다. (2016년 5월 28일 기준)
리눅스에서 직접 다운 받을 땐 다음 명령어를 입력해주면된다.

```bash
wget https://github.com/bcit-ci/CodeIgniter/archive/3.0.6.zip
apt-get install unzip
unzip 3.0.6.zip
```

그러면 CodeIgniter-3.0.6 폴더가 생성된다.

#### -폴더 경로 바꿔주기
폴더 안의 내용을 /var/www으로 옮겨주고 폴더를 지워주면 CI 내용을 사용할 때 폴더경로가 좀 더 간단해진다.
```bash
mv CodeIgniter_3.0.6/* .	// CodeIgniter_3.0.6 폴더 안에 있는 모든 파일을 현재 디렉토리로 옮긴다.
rm -r CodeIgniter_3.0.6
```

### 참고
> http://www.opentutorials.org/course/697/3825 생활코딩


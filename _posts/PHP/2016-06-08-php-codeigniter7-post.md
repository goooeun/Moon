---
layout: post
title:  "[PHP] CodeIgniter(코드 이그나이터) Session (세션)"
date:   2016-06-06
tag:
- PHP
- codeigniter
- framework
- session

comments: true
---

## Session Class
CI에서의 세션 클래스는 사용자의 상태를 관리하고 사이트에서 하는 행위를 추적할 수 있도록 해준다.
페이지가 로드될 때 세션 클래스는 사용자의 세션쿠키를 확인하여 유효한 세션데이터가 있는지 확인한다.
세션에 데이터가 없다면 새로운 세션이 생성되어 쿠키에 저장되고, 세션이 있다면 세션 정보가 업데이트되며 쿠키도 업데이트된다.

### 1.세션 사용하기
세션을 사용하기 전에 config파일을 수정해주어야한다. application/config/config.php 파일을 수정해준다.

```php
$config['sess_driver'] = 'file';
$config['sess_cookie_name'] = 'ci_session';
$config['sess_expiration'] = 7200;
$config['sess_save_path'] = None;
$config['sess_match_ip'] = FALSE;
$config['sess_time_to_update'] = 300;
$config['sess_regenerate_destroy'] = FALSE;
```

처음 config.php 파일을 실행시키면 다음과 같이 설정되어 있다.
세션은 저장하는 방식에는 파일, 데이터베이스, 레디스(redis), 멤캐시(memcached) 등이 있는데, 초기엔 파일로 설정되어있다.
세션을 데이터베이스에 저장시키는 방법을 사용하기 위해서는 sess_driver를 'database'로 변경해주면된다.

그리고 save_path를 변경시켜줘야하는데, 데이터베이스에서 세션을 저장시킬 테이블을 생성할 때 save_path와 동일하게 네이밍을 해주어야한다. 

```bash
CREATE TABLE IF NOT EXISTS `ci_sessions` (
        `id` varchar(40) NOT NULL,
        `ip_address` varchar(45) NOT NULL,
        `timestamp` int(10) unsigned 기본값 0 NOT NULL,
        `data` blob NOT NULL,
        PRIMARY KEY (id),
);
```

MySQL에선 다음을 참고하여 테이블을 생성해주면 된다.

### 2.세션 클래스 초기화
세션을 사용하기 위해선 세션 library를 로드해주어야한다. 

```php
$this->load->libary('session');
```

그러나 대부분 세션은 전역적으로 쓰이므로 auto-load해서 사용해주는 것이 좋다.
application/config/autoload.php 파일을 수정해주면 된다.

```php
$autoload['libraries'] = array('session');	// array에 session 추가
```

### 3.세션 데이터 추가/가져오기

```php
$item = array (
		'name' => 'apple',
		'color' => 'red',
		'exist' => TRUE
);

$this->session->set_userdata('item');		// 세션에 item이라는 이름의 데이터 저장

$this->session->userdata('item');			// 세션에서 item이라는 데이터 가져오기	
```

### 4.세션 데이터 삭제 / 제거

```php
$this->session->unset_userdata('item');		// 세션에서 item이라는 데이터 삭제
$this->session->sess_destroy();				// 세션 데이터 제거
```

### 5.Flashdata
사용자에게 피드백을 할 때 정보나 오류, 상태 메세지를 단 한번만 출력할 수 있는 데이터이다.

```php
$this->session->set_flashdata('message','your message');	// 플래시데이터 생성
$this->session->flashdata('item');		// 플래시 데이터 출력
```


### 참고
> http://www.opentutorials.org/course/697/3982 생활코딩
> http://www.ciboard.co.kr/user_guide/kr/libraries/sessions.html 한국 사용자 포럼


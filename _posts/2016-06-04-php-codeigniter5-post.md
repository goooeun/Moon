---
layout: post
title:  "[PHP] CodeIgniter(코드 이그나이터) URL Routing / Helper"
date:   2016-06-04
tag:
- PHP
- codeigniter
- framework
- URL Routing
- Helper

comments: true
---

## 1.URI Routing

### -URI 매핑 변경
/application/config/route.php 파일에서 변경한다.

#### 1.example.com/Topic/get/1을 example.com/Topic/1로 변경하기
```bash
$route['Topic/(:num)'] = "Topic/get/$1";
```
*$1은 첫번째 괄호 값으로 치환한다는 뜻이고 (:num)의 값에 $1값이 치환된다.

#### 2.example.com/Topic/get/1을 example.com/post/1로 변경하기
```bash
$route['post/(:num)'] = "Topic/get/$1";
```
*post로 접속했을때도 topic이라는 controller 파일을 쓰고싶을 때 사용하면된다.

#### 3.정규식 사용하기
```bash
$route['Topic/([a-z]+)/([a-z]+)/(\d+)'] = "$1/$2/$3";
```
*Topic뒤에 오는 첫 값과 두번째 값은 a-z까지의 알파벳이 1개이상 들어가야하고 세번째 값은 숫자 중 하나 이상이 들어가야한다는 정규식이다.

#### 4.대문페이지와 404페이지
```bash
$route['default_controller'] = 'welcome';
$route['404_override'] = '';
```

* route.php의 기존 설정값이다. default_controller는 example.com만 입력했을 시 실행되는 값이며 자신이 원하는 페이지로 변경하여 대문 페이지를 설정해주면 된다. 404_override는 존재하지 않는 url 입력시 나오는 페이지로 자신이 원하는 페이지로 변경 가능하다.

## 2.Helper
헬퍼는 자주 사용하는 로직을 재사용할 수 있도록 만든 일종의 라이브러리다.
CI에는 Helper와 Libraries가 둘다 존재하는데 이 둘은 약간의 차이가 있다.
Libraries가 객체지향인 클래스로 이루어졌다면 Helper는 독립적인 함수로만 이루어졌다.

### 1.CI가 제공하는 Helper
#### -배열(Array Helper)
#### -캡챠(CAPTCHA Helper)
#### -쿠키(Cookie Helper)
#### -날짜(Date Helper)
#### -디렉토리(Directory Helper)
#### -다운로드(Download Helper)
#### -이메일(Email Helper)
#### -파일(File Helper)
#### -폼(Form Helper)
#### -HTML Helper
#### -인플렉터(Inflector Helper)
#### -언어(Language Helper)
#### -숫자(Number Helper)
#### -경로(Path Helper)
#### -보안(Security Helper)
#### -스마일리(Smiley Helper)
#### -문자열(String Helper)
#### -텍스트(Text Helper)
#### -타이포그라피(Typography Helper)
#### -URL Helper
#### -XML Helper

### 2.Helper 로드하기
#### 1.직접 로드하기
```bash
$this->load->helper('사용할 헬퍼 이름');
$this->load->helper(array('Helper1','Helper2'));	// 여러 개 사용할 떄
```

#### 2.autoload 하기
/application/config/autoload.php 수정

```bash
$autoload['helper'] = array(); 		// array  안에 사용할 헬퍼들을 넣어준다.
```

### 3.Helper 직접 만들기
헬퍼는 라이브러리보다 가볍고 만들기도 용이하다. 자주사용하는 함수는 헬퍼로 만들어주면 사용이 용이하다.
application/helper폴더에 파일을 생성한 뒤 함수를 넣어주고 사용하면 된다.

### 참고
> http://www.opentutorials.org/course/697/3838 생활코딩 URL Routing
> http://www.opentutorials.org/course/697/3836 생활코딩 Helper

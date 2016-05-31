---
layout: post
title:  "[PHP] CodeIgniter(코드 이그나이터) Model"
date:   2016-05-30
tag:
- PHP
- codeigniter
- framework
- Model

comments: true
---

## Model
View가 사용자에게 보여지는 부분을 담당한다면 Model은 데이터 부분을 담당한다.
Model은 주로 데이터베이스와 연동하기 위해 사용된다.

## Model의 사용법 

모델은 application/models/ 폴더에 저장하면 된다. 
모델의 이름은 controller의 이름에 _model이라고 붙여주는 방법으로 지어주면 사용하기 용이하다.
*ex) controller -> News.php, model -> News_model.php

모델의 클래스 이름은 반드시 첫글자는 대문자, 나머지 글자는 소문자로 만들어야한다.

### 1.데이터베이스 생성
자신이 사용하는 데이터베이스(ex. MySQL, Oracle..)를 사용하여 데이터베이스를 생성해준다.
생성 후 application/config/ 폴더의 database.php 파일을 수정해준다.
```php
$db['default'] = array(
	'hostname' => '서버 주소',
	'username' => '데이터베이스의 사용자 이름',
	'password' => '사용자 비밀번호',
	'database' => '데이터베이스명',
	'dbdriver' => '자신이 사용하는 데이터베이스(ex. MySQL, Oracle)',
```

### 2.Model의 기본 프로토타입
모델 파일을 생성해준다.
```php
<?php
class Model_name extends CI_Model {		// CI_Model을 상속받는 Model_name 클래스
        public function __construct()	// 초기화를 해주는 부분
        {
                parent::__construct();
        }
        // 여기에 추가로 사용하고 싶은 메소드를 넣어주면 된다.
}
?>
``` 

* __construct에 함수 내에서 반복적으로 사용되는 부분을 넣어주면 좋다.
```php
ex) $this->load->database(); 
```

### 3.Controller에서 Model 사용하기
```php
<?php
$this->load->database(); 				// 데이터베이스 설정에 맞추어 데이터베이스 초기화
$this->load->model('Model_name');
$this->load->model('Model_name')->Model의 메소드();
?>
```

### 4.Model에서 쿼리문 사용하기
#### -일반적인 쿼리 사용법
```php
<?php
class Model_name extends CI_Model {
	function __construct() {
		parent::__construct();
	}
	public function method() {
		return $this->db->query("SELECT * FROM table")->result();
	}
}
?>
```

*여기서 method가 ruturn해주는 result 값의 형태는 객체형태이며 
*array형태로 return해주고 싶을 땐 result_array()라고 해주면 된다.

#### -Active Record 방식
정보의 추출,삽입, 업데이트를 최소한의 코드로 수행할 수 있게 도와주는 방식이다.
```php
$this->db->get('테이블 이름', $limit, $offset);	// "SELECT * FROM 테이블 이름 LIMIT offset, limit"
```

```php
$this->db->get_where('테이블 이름',array('id'->$id), $limit, $offset);	// "SELECT * FROM 테이블 이름 WHERE id = $id LIMIT offset, limit"
```

### 5.View에서 데이터 출력하기
#### -controller에서 view로 값을 보낸다.
```php
$data = $this->Model_name->method();			// method의 결과를 $data에 저장
$this->load->view('view이름',array('datas'=>$data));	// $data의 내용을 datas라는 변수에 담아 view로 전달
```

#### -view에서 값을 받아 출력하기
```php
foreach('datas' as $entry) {
	echo $entry->id;							// datas 객체 각각의 id를 출력
}
```

### 참고
> http://www.opentutorials.org/course/697/3827 생활코딩
> http://codeigniter-kr.org CodeIgniter 한국 사용자 포럼


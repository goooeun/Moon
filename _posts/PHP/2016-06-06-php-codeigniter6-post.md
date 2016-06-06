---
layout: post
title:  "[PHP] CodeIgniter(코드 이그나이터) Config (설정)"
date:   2016-06-06
tag:
- PHP
- codeigniter
- framework
- config
- 설정

comments: true
---

## 설정

### 1.application/config 폴더의 주요 파일

#### -config.php : 필수적인 설정 파일

#### -database.php : 데이터베이스 관련, 데이터베이스 핸들링 

#### -autoload.php : 자주/항상 사용되는 라이브러리 autoload하고 싶을 때

#### -hooks.php : system 폴더 안에 들어있는 CI의 로직을 수정하고 싶을 때 직접 system 폴더 안의 파일들을 수정하는 것은 좋지 않기 떄문에 hooks.php를 사용하여 수정

#### -routes.php : URI routing 관련

### 2.설정 파일 관리 하는 방법
database.php에서 데이터베이스와 연결되는 부분인 hostname, username, password, database 부분은 버전 관리시 오픈소스로 공개되면 보안이 취약해져 정보 유출의 위험이 있다. 이런 부분은 보안상의 이유로 버전 관리에 포함하면 안된다.

.gitignore 파일 안에 내용을 추가해준 후 버전 관리 해준다.

```bash
/application/config/database.php
```

### 3.환경(environment) 설정
root 폴더의 index.php 파일을 확인한다.
	
```php
define('ENVIRONMENT', isset($_SERVER['CI_ENV']) ? $_SERVER['CI_ENV'] : 'development');
```

두번째 파라미터에 값을 정해주면 된다.
개발시에는 모든 에러메시지를 확인할 수 있는 'development'로 설정해주면 된다. 
에러메시지가 출력되지 않아야하는 실서비스 환경에서는 'production', 테스트 환경에서는 'testing'이라고 설정해주면 된다.

$_SERVER['CI_ENV']는 .htaccess이나 아파치의 경우 SetEnv를 사용해서 설정할 수 있다.

### 4.config.php 파일 해설
```php
$config['language']    = 'english';			// 언어를 설정
$config['enable_hooks'] = FALSE;		// hook 기능을 활성화
$config['subclass_prefix'] = 'MY_';		// Core 클래스를 상속 받아 커스터마이징 할 때 클래스 이름의 약속된 접두사를 변경
$config['log_threshold'] = 0;				// 로그 상태 변경 
									// 0: 로깅 하지 않는다  1: 에러만 출력  2: Debug Message  3: Informaion Message  4: 모든 메세지
$config['log_path'] = '';					// 로그 파일을 저장 위치 지정
$config['cache_path'] = '';				// 캐쉬 파일을 저장 위치 지정
```

### 5.설정 정보의 사용
```php
$this->config->item('사용할 config');		// 설정 정보가 리턴되며 값이 없을 시 FALSE가 리턴
```

*Model, View, Controller 모두에서 사용 가능하다.

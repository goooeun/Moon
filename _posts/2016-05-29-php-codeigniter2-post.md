---
layout: post
title:  "[PHP] CodeIgniter(코드 이그나이터) Controller와 View"
date:   2016-05-29
tag:
- PHP
- codeigniter
- framework
- MVC
- Controller
- View
comments: true
---

## MVC 패턴
CI는 모델-뷰-컨트롤러(Model–View–Controller, MVC) 패턴에 기반한다.
MVC는 사용자 인터페이스 부분과 비즈니스 로직부를 분리시켜 서로에게 영향을 주지 않고 개발할 수 있다는 장점이 있다.

- Model : 데이터구조를 표현, 일반적으로 모델 클래스는 데이터를 추출,입력,갱신하는등의 함수를 포함
- View : 사용자에게 보여질 부분, UI에 해당
- Controller : 일반적으로 모델과 뷰(혹은 HTTP 요청을 처리하여 웹페이지를 생성하는 어떤 것) 사이에서 동작

## Controller

MVC 중 C에 해당되는 것이 Controller이다.
간단하게 설명하자면 Controller의 하는 일은 URL에 매핑되는 작업을 수행하는 것이다.
CI에서 URL 규칙은 다음을 참고하면 된다.
http://www.ciboard.co.kr/user_guide/kr/general/urls.html

* 처음 설정해주어야하는 것들

	1. base_url 수정 
		처음 설치 후엔 base_url이 설정되지 않은 상태이므로 applcation/config/config.php에서 base_url부분을 자신이 사용하는 웹서버 url로 수정해주어야한다.
		{% highlight bash %}
		$config['base_url'] = '사용할 웹 서버 url';
		{% endhighlight %}

	2. index.php url에서 숨기기
		처음엔 url로 controller의 클래스에 접근할때 http://example.com/index.php/접근할클래스 라는 식으로 index.php을 입력해줘야한다.
		하지만 url이 길어지는 건 비효율적이기 때문에 저 부분을 없애는 것이 좋다.
	
		1.rewrite module 활성화
		```bash
		sudo a2enmod rewrite
		sudo service apache2 restart
		```

		2./etc/apache2에서 apache2.conf 수정 
		```bash
		<Directory /var/www/html/>
			Options Indexes FollowSymLinks
			AllowOverride All 		// none으로 되있던 것을 All로 수정
			Require all granted
		</Directory>
		```

		3.아파치를 재시작해준다.
		```bash
		apachectl restart
		```

		4.applicatoin/config/config.php 파일 내용을 수정해준다.
		```bash
		$config['index_page'] = 'index.php';
		-> $config['index_page'] = '';
		```

		5..htaccess 파일을 만들고 다음 내용을 넣어준다.
		```bash
		<IfModule mod_rewrite.c>
		    RewriteEngine On
		 RewriteBase /
		 RewriteCond $1 !^(index\.php|images|captcha|data|include|uploads|robots\.txt)
		 RewriteCond %{REQUEST_FILENAME} !-f
		 RewriteCond %{REQUEST_FILENAME} !-d
		 RewriteRule ^(.*)$ /index.php/$1 [L]
		</IfModule>
		```

		6..httaccess 파일의 권한을 변경해준다.
		```bash
		chmod 755 .httaccess
		```

		### 참고
		> http://www.codeigniter-kr.org/bbs/view/lecture?idx=7073


http://example.com/News/latest/10 이라는 URL이 있을 때
Controller는 News 클래스의 latest 메소드를 호출하고 인자 값으로 10을 준다는 의미이다. news 클래스를 작성하는 법은 다음과 같다.

	1. Controller 폴더 안에 news.php 파일 생성하기
		application 폴더 안에 있는 Controller 폴더에 News.php 파일을 만든다.
		```php
		<?php
		defined('BASEPATH') OR exit('No direct script access allowed');
		class News extends CI_Controller {
			public function index()
			{
				echo "controller test";
			}
		}
		```
		index 부분은 http://example.com/News 라고 입력했을 때 실행되는 부분이다.

	2. 파라메터가 있는 latest 함수 만들기
		다음은 latest가 인자값을 echo로 출력해주는 소스이다.

		```php
		<?php
		defined('BASEPATH') OR exit('No direct script access allowed');
		class News extends CI_Controller {
			public function index()
			{
				echo "controller test";
			}
			public function latest($id) {
				echo $id;
		}
		?>
		```
		http://example.com/news/latest/10이라고 입력했을 땐 페이지에 10이 출력되고
		http://example.com/news/latest/5라고 입력했을 땐 페이지에 5가 출력되는 것을 확인할 수 있다.

## View

사용자에게 보여주는 부분으로 Controller에서도 echo하여 페이지를 출력할 수 있으나 view를 사용하여 관리하는 것이 훨씬 관리하기 용이하다.
화면에 보여지는 것들은 view에 작성해주는 것이 좋다.
view 파일은 View 폴더에 넣어주면 된다.
Controller의 news 클래스에서 latest.php라는 view 파일에 접근하는 방법은 다음과 같다.

```php
<?php
defined('BASEPATH') OR exit('No direct script access allowed');
class News extends CI_Controller {
	public function index()
	{
		echo "controller test";
	}
	public function latest($id) {
		$this->load->view('latest',array('id'=>$id));
}
?>
```


 ### 참고
 > http://www.opentutorials.org/course/697/3826 생활코딩
 > http://codeigniter-kr.org CodeIgniter 한국 사용자 포럼


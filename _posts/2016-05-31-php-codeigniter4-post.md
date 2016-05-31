---
layout: post
title:  "[PHP] CodeIgniter(코드 이그나이터) Database Reference"
date:   2016-05-30
tag:
- PHP
- codeigniter
- framework
- 데이터베이스 레퍼런스

comments: true
---

## 1.데이터베이스 연결

### -자동 연결
/application/config/autoload.php 파일 수정
```php
$autoload['libraries']=array('');
->$autoload['libraries']=array('database');
```

### -수동 연결
데이터베이스 사용시마다 호출
```php
$this->load->database();
```

*/application/config/database.php의 데이터베이스 설정에 맞추어 데이터베이스가 초기화된다.

### -여러개의 데이터베이스 사용(동시 연결)
```php
$DB1 = $this->load->database('group_one',TURE);
$DB2 = $this->load->database('group_two',TURE);
``` 

*$this->db->result()를 $DB1->result()로 사용 가능

## 2.쿼리 실행

### -일반 쿼리
```php
$query = $this->db->query('QUERY문');	// $query에 쿼리 실행 결과 객체가 들어간다.
```

### -단순 쿼리
```php
$this->db->simple_query('QUERY문');		// 결과가 TRUE,FALSE로만 반환된다.
```

### -쿼리 바인딩
```php
$sql = "SELECT * FROM table_name WHERE id = ? AND name = ?";
$this->db->query($sql,array(3,'goeun'));		// SELECT * FROM table_name WHERE id = 3 AND name = 'goeun';
```

### -에러 랜들링
$this->db->error();						// 코드와 메시지를 포함한 배열로 

## 3. 쿼리 결과 생성

### -Result Arrays

#### 1.result() : 객체로 리턴
foreach 사용
```php
foreach($query->result() as $entry) {
	echo $entry->id;
	echo $entry->name;
}
```

#### 2.result_array() : 배열로 리턴
foreach 사용
```php
foreach($query->result() as $entry) {
	echo #entry['id'];
	echo $entry['name'];
}
```

### -Result Rows
한 줄의 결과만 리턴된다. 쿼리가 한 줄 이상일 땐 첫번째 줄만 리턴된다.

#### 1.row() : 객체로 리턴
특정열의 결과를 리턴 받고 싶을 땐 row(5); 식으로 사용해주면 된다.

#### 2.row_array() : 배열로 리턴

#### 3.row의 추가 함수
```php
$row = $query->first_row();				// 첫번째 레코드로 이동
$row = $query->last_row();				// 마지막 레코드로 이동
$row = $query->next_row();				// 다음 레코드로 이동
$row = $query->previous_row();			// 이전 레코드로 이동
```

*위 함수들의 결과는 객체로 리턴된다. 배열로 리턴 받고 싶다면 괄호안헤 'array'를 넣어주면된다.

## 4.결과 헬퍼 함수

#### -num_rows() : 쿼리 결과열의 개수를 리턴

#### -$this->db->affected_rows() : insert, update 등에서 쿼리 수행시 적용된 결과 열 수를 리ㅓㄴ

## 5.쿼리 빌더 클래스 (액티브 레코드)

### -SELECT
```php
$this->db->get('테이블명');				// SELECT * FROM 테이블명;
$this->db->get_where('테이블명',array('id'=>$id));		// SELECT * FROM 테이블명 WHERE id = $id;
// select를 지정할 때
$this->db->select('id,name,email');
$query->$this->db->get('테이블명');
```

#### *집계 함수
```sql
$this->db->select_max();				// MAX
$this->db->select_min();				// MIN
$this->db->select_sum();				// SUM
$this->db->select_avg();				// AVG
```

#### *JOIN(조인)
```php
$this->db->select('*');
$this->db->from('테이블명');
$this->db->join('변수명', '테이블A.id = 테이블B.id');
$query = $this->db->get();
```

### -조건 검색

#### 1.LIKE
여러 개를 나열하면 AND로 연결되고 OR로 연결하고 싶을 땐 or_like()를 써준다.
```php
$this->db->like('name','match');				// LIKE '%match%'
$this->db->like('name','match','before');		// LIKE '%match'
$this->db->like('name','match','after');		// LIKE 'match%'
$this->db->like('name','match','both');			// LIKE '%match%'

// 연관 배열로 사용
$array = array('name'=>$match,'title'=>$match);
$this->db->like($array);
```

#### 2.GROUP BY
```php
$this->db->group_by();
$this->db->group_by(array("id","date")); 		// 여러 값 사용시 배열로 사용
```

#### 3.DISTINCT
```php
$this->db->distinct();
$this->db->get('table_name'); 		
```

#### 4.HAVING
여러 개를 나열하면 AND로 연결되고 OR로 연결하고 싶을 땐 or_having()를 써준다.
```php
$this->db->having('user_id = 45');  
$this->db->having('user_id',  45);				// 둘 다 사용 가능

// 배열 사용시
$this->db->having(array('title =' => 'My Title', 'id <' => $id));	
```

#### 5.ORDER BY
정렬 방향은 ASC, DESC, RANDOM이 있다.
```php
$this->db->order_by('title', '정렬 방향');
$this->db->order_by('title 정렬 방향, name 정렬 방향');
```

#### 6.LIMIT
```php
$this->db->limit(10);							// LIMIT 10
$this->db->limit(10,20);						// LIMIT 20, 10
```

### -WHERE

#### 1.단순 키 값
```php
$this->db->where('id',$id);				// 여러 개를 이어쓰면 AND로 연결
$this->db->or_where('name',$name);		// OR로 연결하고 싶을 땐 or_where 사용
```

#### 2.사용자 키/값 (연산자 사용)
```php
$this->db->where('id >', $id);			// 첫번째 파라메터에 연산 기호 삽입, 아무것도 입력안했을 땐 자동으로 =이 추가된다.
```

#### 3.연관 배열
```php
$array = array('id'=>$id, 'name !='=>$name);
$this->db->where($array);				
```

#### 4.사용자 문자열
```php
$where = "id = 3 AND name = 'goeun";
$this->db->where($where);
```

### -INSERT

#### 1.배열 사용
```php
$data = array (
		'title' => 'My Title',
		'content' => 'My Content',
		'date' => 'My Date'
);
$this->db->insert('table_name', $data);
```

#### 2.객체 사용
```php
class Myclass {
        public $title = 'My Title';
        public $content = 'My Content';
        public $date = 'My Date';
}
$object = new Myclass;
$this->db->insert('mytable', $object);
```

### -UPDATE

#### 1.replace()
```php
$data = array(
        'title' => 'My title',
        'name'  => 'My Name',
        'date'  => 'My date'
);
$this->db->replace('table', $data);
```

*실행결과는 REPLACE INTO mytable (title, name, date) VALUES ('My title', 'My name', 'My date')와 동일하다.

#### 2.set()
insert, update 시 데이터 배열을 사용하는 방법 대신 사용, 객체와 배열 모두 사용 가능하다.
```php
$this->db->set('name', $name);
$this->db->insert('mytable');
```

*실행결과는 INSERT INTO mytable (name) VALUES ('{$name}')와 동일하다.

#### 3.update()
배열과 객체 모두 사용가능하다.
```php
// 배열 사용
$this->db->where('id', $id);
$this->db->update('mytable', $data);

// 객체 사용
$object = new Myclass;
$this->db->where('id', $id);
$this->db->update('mytable', $object);
```

### 4.update()시 where 사용
where 조건을 사용하고 싶을 땐 세번째 파라미터로 값을 전달해주면 된다.
```php
$this->db->update('mytable', $data, "id = 4"); 				// 객체 사용
$this->db->update('mytable', $data, array('id' => $id)); 	// 배열 사용
```

### -DELETE
```php
$this->db->delete('mytable', array('id' => $id)); 			// 두 번째 파라미터는 where절
$this->db->empty_table('mytable');							// 테이블의 모든 데이터 삭제
```

# yiiFramework 1.1 Sample

yiiFramework 1.1 을 이용한 샘플 소스 입니다.

## 초기 환경 설정, 확인하기

http://www.yiiframework.com/download/ 에서 다운로드 후, 일정 경로에 압축을 풉니다.

원하는 웹 document 경로를 생성하고(빈 폴더 형태로), vhost, hosts 파일을 수정합니다.

저는 local.yii11.com 로 설정하였습니다.

아파치나 웹 서버 설정이 다 되었다면, yiiFramework 소스의 requirements 디렉토리 안의 내용을 웹 document 경로로 복사합니다.

http://local.yii11.com/ 로 접근하면, 현재 프레임워크가 잘 뜰 수 있는 환경인지 체크를 합니다. failed 가 있는 경우에 해당 모듈을 설치하도록 합니다.

여기 까지가 01 브랜치의 내용입니다.

## 브랜치별로 예제 돌려보기

### 01. 초기 환경 설정, 확인하기

위의 초기 환경 설정, 확인하기 내용을 참조해주세요.


### 02. 프로젝트 생성하기

먼저 requirements 에서 복사했던 파일들을 삭제 후 시작하도록 합니다.

- 압축을 푼 yiiFramework 경로 쪽으로 이동합니다.

```
$ cd ~/Development/PHP/yiiFramework/yii-1.1.14.f0fee9/framework
```

- yiic 명령어를 이용하여 프로젝트를 생성합니다.

*NIX 의 경우의 실행 예

```
$ ./yiic webapp ~/Projects/yii1-1-sample
```

이 부분은 선택사항입니다.
- ~/Development/PHP/yiiFramework/yii-1.1.14.f0fee9/framework 디렉토리를 프로젝트 경로(~/Projects/yii1-1-sample)로 복사합니다.


```
$ cp ~/Development/PHP/yiiFramework/yii-1.1.14.f0fee9/framework ~/Projects/yii1-1-sample
```

framework 디렉토리를 복사한경우 /index.php 파일에 ```$yii=dirname(__FILE__)``` 쪽 정보를 다음과 같이 수정합니다.

```
$yii=dirname(__FILE__).'/framework/yii.php';
```

- 브라우저에서 http://local.yii11.com/ 경로를 띄워봅니다.

브라우저에서 화면이 정상적으로 뜨면 설정파일을 수정 해줍니다.


/protected/config/main.php 파일을 열어
- ```'name'``` 부분을 어플리케이션 이름으로 변경합니다.

- ```'gii'``` 부분 주석을 풀어줍니다.

- 로그를 웹 페이지에서 보려면 'log'=>array( 부분 을 수정합니다. 밑의 예처럼 주석을 풀어줍니다.


```
array(
  'class'=>'CWebLogRoute',
),

```

- DB 설정을 수정합니다. 기본은 sqlite 를 사용하도록 설정되어있습니다.

밑의 부부분을 주석처리하고...

```
'db'=>array(
	'connectionString' => 'sqlite:'.dirname(__FILE__).'/../data/testdrive.db',
),
```

다음과 같이 DB 정보를 세팅합니다.
디비명이 ```yii_test_db``` 로 세팅되어있는 경우의 예입니다.

```
// uncomment the following to use a MySQL database
'db'=>array(
  'connectionString' => 'mysql:host=localhost;dbname=yii_test_db',
  'emulatePrepare' => true,
  'username' => 'root',
  'password' => '1234',
  'charset' => 'utf8',
),
```



### 03. 코드 생성하기

yiiFramework 코드 생성하는 부분이 제공됩니다.

먼저 DB 에 두개의 테이블을 생성하도록 합니다.


```
CREATE TABLE Employee (
    id MEDIUMINT UNSIGNED NOT NULL AUTO_INCREMENT PRIMARY KEY,
    departmentId TINYINT UNSIGNED NOT NULL
        COMMENT "CONSTRAINT FOREIGN KEY (departmentId) REFERENCES Department(id)",
    firstName VARCHAR(20) NOT NULL,
    lastName VARCHAR(40) NOT NULL,
    email VARCHAR(60) NOT NULL,
    ext SMALLINT UNSIGNED NULL,
    hireDate TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    leaveDate DATETIME NULL,
    INDEX name (lastName, firstName),
    INDEX (departmentId)
);

CREATE TABLE Department (
    id TINYINT UNSIGNED NOT NULL AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(40),
    UNIQUE (name)
);
```

- 이제 http://local.yii11.com/index.php?r=gii 페이지롤 접근해 봅니다. (code generator 웹 경로)

- 파라메터를 넘기는 부분인 r=gii 부분을 좀 더 예쁘게 표시해주기 위해 /protected/config/main.php 파일을 열어 다음부분의 주석을 풀어줍니다.

```
'urlManager'=>array(
  'urlFormat'=>'path',
  'rules'=>array(
    '<controller:\w+>/<id:\d+>'=>'<controller>/view',
    '<controller:\w+>/<action:\w+>/<id:\d+>'=>'<controller>/<action>',
    '<controller:\w+>/<action:\w+>'=>'<controller>/<action>',
  ),
),
```

- 이제 http://local.yii11.com/index.php/gii 로 접근하면 code generator 페이지를 볼 수 있습니다.


Model Generator 링크를 클릭하여 Employee, Department model 을 생성합니다.
- Table Name 부분에 테이블명을 적습니다.
- Preview 버튼을 눌러 생성되는 파일을 확인합니다.
- Generate 버튼을 눌러 코드를 생성합니다.

* 권한이 없는 경우 파일을 쓸수 없다고 나올 것 입니다. 이 경우 /protected 경로의 퍼미션을 조정 후에 다시 코드를 생성해 봅니다.

- /protected/models 디렉토리에 Employee.php, Department.php 파일이 생성 되었는지 확인합니다.



다음으로 Crud Generator 링크를 클릭하여 코드를 생성합니다.

- Model Class 부분에 Employee, Department 입력하여 코드를 생성합니다.
- 마찬가지로 Preview 버튼을 누른 후 Generate 버튼을 눌러 코드 생성을 하면 됩니다.
* Controller ID 부분은 자동으로 소문자로 시작하게 나오는데 이는 그대로 두도록합니다.


코드가 잘 생성되었는지 확인을 하기 위해 다음의 페이지들로 이동해 봅니다.
- http://local.yii11.com/index.php/department
- http://local.yii11.com/index.php/employee

* view, show, index php 파일이 생성됩니다. (index 는 list 페이지 임)







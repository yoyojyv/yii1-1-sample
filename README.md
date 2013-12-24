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


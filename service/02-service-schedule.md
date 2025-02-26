# Schedule Service

---
## 1. Schedule Service 란
### 1.1. 정의

>등록된 DB 서비스, Custom 서비스를 특정시간, 반복 실행이 필요할때 사용

## 2. 사용법
### 2.1. 생성

🎈 __menu > 서비스 > Schedule Service > 생성__

<img src = "./images/02-service-schedule-01.png" width = "750px"> </img>

### 2.2. 속성

<img src = "./images/02-service-schedule-01-2.png" width = "600px"> </img>

| 구분 | 설명 |
|:--:|:--|
| Schedule 서비스 ID | 고유한 ID(중복 불가, 영어, 숫자, underscore('_') 5자 이상 50자 이내 |
| Schedule 서비스명 | 이름, 혹은 설명입력, 작업자가 구분하기 위해 사용 |
| 그룹 | 작업자가 구분하기 위해 사용 |
| 구분 | Java class 파일, DB Service 선택 |
| 상태 | 서비스 사용 상태 구분, 활성 / 비활성 선택하여 사용 선택가능 |
| 클래스명<br/>구분: Java class 파일 일때 | 실행될 클래스파일 지정 |
| 실행될 서버 | 만들어질 서비스가 실행될 서버 지정 |
| Cron Exp | 실행 주기 설정 |


#### 2.2.1. DB Service ID

<img src = "./images/02-service-schedule-01-3.png" width = "550px"> </img>

- __DB Service ID 선택 시__
	- "DB Service" 는 platform에 등록된 DB Service를 호출하는 방식  
	- Schedule 서비스는 자동으로 실행되는 서비스 이므로 입력값이 필요없는 DB Service를 만들어 사용


#### 2.2.2. JAVA class 파일
- __구분 - JAVA class 파일 선택 시__
	- "Java class 파일" 은 org.quartz.Job의 interface를 사용하여 빌드해야 만 정상 작동  
	- 매뉴얼 참조 및  샘플파일 참고  
	- 등록이나 수정시에 반드시 class 파일이 platform 내부에 먼저 존재 해야 가능

```java
Schedule 서비스에 등록될 JAVA 기본 구조

...

import org.quartz.JobDataMap;
import org.quartz.JobDetail;
import org.quartz.JobExecutionContext;
import org.quartz.StatefulJob;

...

public class CronSample implements StatefulJob {
    public void execute(JobExecutionContext context) {
        ...
    }
}
```

---
## 3. 별첨
### 3.1. Cron exp

```
1 Seconds (0–59)
2 Minutes (0–59)
3 Hours (0–23)
4 Day of month (1–31)
5 Month (1–12 or JAN–DEC)
6 Day of week (1–7 or SUN–SAT)
7 Year (1970–2099)

*는 all, ?는 설정않함.
examples..

0 0 10 * * ?
매일 오전 10시에 실행

0 0 10 1 * ?
매월 1일 오전 10시에 실행

0 0 10 ? ? 1 ?
매주 일요일 오전 10시에 실행

0/5 * * * * ?
5초마다 실행

필드 허용범위 허용문자
초 0-59 , – * / 
분 0-59 , – * / 
시 0-23 , – * / 
일 1-31 , – * ? / L W
월 1-12 or JAN-DEC , – * / 
요일 1-7 or SUN-SAT , – * ? / L # 
년(옵션) 1970-2099 , – * /

* 모든 값
? 특정 값 없음
- 범위 지정에 사용
, 여러 값 지정 구분에 사용
/ 초기값과 증가치 설정에 사용
L 지정할 수 있는 범위의 마지막 값
W 월~금요일 또는 가장 가까운 월/금요일
# 몇 번째 무슨 요일 2#1 => 첫 번째 월요일

예제) 
Expression Meaning 
초분시일월주(년)
“0 0 12 * * ?” 아무 요일, 매월, 매일 12:00:00
“0 15 10 ? * *” 모든 요일, 매월, 아무 날이나 10:15:00 
“0 15 10 * * ?” 아무 요일, 매월, 매일 10:15:00 
“0 15 10 * * ? *” 모든 연도, 아무 요일, 매월, 매일 10:15 
“0 15 10 * * ? 2005″ 2005년 아무 요일이나 매월, 매일 10:15 
“0 * 14 * * ?” 아무 요일, 매월, 매일, 14시 매분 0초 
“0 0/5 14 * * ?” 아무 요일, 매월, 매일, 14시 매 5분마다 0초 
“0 0/5 14,18 * * ?” 아무 요일, 매월, 매일, 14시, 18시 매 5분마다 0초 
“0 0-5 14 * * ?” 아무 요일, 매월, 매일, 14:00 부터 매 14:05까지 매 분 0초 
“0 10,44 14 ? 3 WED” 3월의 매 주 수요일, 아무 날짜나 14:10:00, 14:44:00 
“0 15 10 ? * MON-FRI” 월~금, 매월, 아무 날이나 10:15:00 
“0 15 10 15 * ?” 아무 요일, 매월 15일 10:15:00 
“0 15 10 L * ?” 아무 요일, 매월 마지막 날 10:15:00 
“0 15 10 ? * 6L” 매월 마지막 금요일 아무 날이나 10:15:00 
“0 15 10 ? * 6L 2002-2005″
2002년부터 2005년까지 매월 마지막 금요일 아무 날이나 10:15:00 
“0 15 10 ? * 6#3″ 매월 3번째 금요일 아무 날이나 10:15:00

“?”와 “*”의 차이를 구분하기가 어렵습니다. 그냥 “?”는 한 개만 사용하는 것으로 생각하시면 됩니다
```
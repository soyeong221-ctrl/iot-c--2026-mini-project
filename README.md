# iot-c--2026-mini-project
## PlanMate 개발과정

## DAY1
### 프로젝트 제안서

## DAY2
### 프로젝트 초기 설정
- 목표: 사용자 입력 기반 프로그램 이해
    - Visual Studio에서 C++ 콘솔 프로젝트 생성
    - main.cpp, Task.h 파일 분리
    - 프로그램 실행 구조(메뉴 기반 CLI) 설계

### 일정 데이터 구조 설계
- 일정 관리를 위한 Task 클래스 정의
    - 데이터 구조화, 객체지향 기본 개념

```c
class Task {
public:
    string title;
    string date;
    int priority;
};
```

### 일정 저장 구조 구현(vector)
- 여러 개의 일정을 저장하기 위해 vector 사용
    - 동적 배열 사용, 여러 데이터를 효율적으로 관리하는 방법

```c
vector<Task> tasks;
```

### 일정 추가 기능 구현
- 사용자로부터 입력을 받아 일정을 생성하고 저장
    - 사용자 입력 처리
    - 데이터 생성 -> 저장 흐름 이해

```c
Task t;
cin >> t.title;
cin >> t.date;
cin >> t.priority;

tasks.push_back(t);
```

### 전체 일정 조회 기능 구현
- 반복문을 사용하여 모든 일정 출력
    - 반복문 활용
    - 배열/리스트 순회

```C
for (int i = 0; i < tasks.size(); i++) {
    cout << tasks[i].title << endl;
}
```

### 일정 삭제 기능 구현
- 사용자가 선택한 번호에 해당하는 일정 삭제
    - vector 데이터 삭제
    - 사용자 입력(1번)과 인덱스(0번)차이 이해
    - 예외 처리 필요성

```C
int index = num - 1;
tasks.erase(tasks.begin() + index);    
```
#### 예외처리
    - 잘못된 번호 입력 방지, 데이터가 없는 경우 처리

```C
if (tasks.empty()) {
    cout << "삭제할 일정이 없습니다." << endl;
}

if (index < 0 || index >= tasks.size()) {
    cout << "잘못된 번호입니다." << endl;
}
```    

### 일정 완료 상태 기능 추가
- 일정의 완료 여부를 관리하기 위해 상태 값 추가
    - 데이터에 상태 추가
    - 단순 저장 -> 관리 기능 확장

```C
bool isDone;  // 일정 생성 시 기본값 설정

t.isDone = false;  // 일정 생성 시 기본 상태는 TO-DO

if (tasks[i].isDone)
    cout << "[DONE]";
else
    cout << "[TO-DO]";

// 일정의 상태를 사용자가 직접 변경할 수 있도록 상태 토글 기능 구현
tasks[index].isDone = !tasks[index].isDone;
```

### 우선순위 기반 정렬 기능 구현
- sort() 사용하여 중요한 일정이 먼저 나오도록 정렬
    - 정렬 알고리즘 활용
    - 기준을 설정하여 데이터 정렬

```C
sort(tasks.begin(), tasks.end(), [](Task a, Task b) {
    return a.priority < b.priority;
});
```

### 오늘 할 일 필터링 기능 구현
- 사용자가 입력한 날짜와 일치하는 일정만 출력하도록 구현
    - 조건문 활용
    - 데이터 필터링 개념 이해

```C
if (tasks[i].date == today)
```

### 파일 저장 기능 구현(데이터 유지)
- 프로그램 종료 후에도 일정 데이터를 유지하기 위함
    - ofstream 사용 -> 파일 생성 및 데이터 저장
    - 각 일정 정보를 한 줄 단위로 기록
    - 프로그램 종료 후에도 데이터 유지 가능

    - 파일 입출력 기본 구조 이해
    - 프로그램 외부 저장소 활용 개념 학습

```C
void saveTasks(vector<Task>& tasks) {
    ofstream file("tasks.txt");

    for (int i = 0; i < tasks.size(); i++) {
        file << tasks[i].title << " "
             << tasks[i].date << " "
             << tasks[i].priority << " "
             << tasks[i].isDone << endl;
    }

    file.close();
}
```

### 파일 불러오기 기능 구현
- 프로그램 실행 시 기존 데이터를 자동으로 불러오도록 구현
    - ifstream 사용 -> 파일 데이터 읽어옴
    - 반복문 통해 파일 끝까지 데이터 읽어 리스트에 저장
    - 파일이 없을 경우 예외 처리 수행

    - 파일 읽기 구조 이해
    - 데이터 복원 개념 학습

```C
void loadTasks(vector<Task>& tasks) {
    ifstream file("tasks.txt");

    if (!file) return;

    Task t;

    while (file >> t.title >> t.date >> t.priority >> t.isDone) {
        tasks.push_back(t);
    }

    file.close();
}
```

### 프로그램 시작 시 데이터 자동 로드
- 프로그램 실행 시 저장된 데이터를 자동으로 불러옴
- 기존 일정 유지

- 프로그램 실행 흐름과 데이터 초기화 이해

```C
vector<Task> tasks;
loadTasks(tasks);
```

### 데이터 변경 시 자동 저장 처리
- 일정 추가 및 삭제 시 파일 자동 갱신 구현
    - 데이터 변경 시 즉시 파일에 반영
    - 최신 상태 유지

    - 데이터 일관성 유지 방법 이해
    - 실시간 저장 구조 학습

```C
tasks.push_back(t);
saveTasks(tasks);

tasks.erase(tasks.begin() + index);
saveTasks(tasks);
```

### 일정 완료 상태 변경 기능 구현
- 사용자가 직접 일정 완료 상태를 변경할 수 있도록 기능 추가
    - 단순 상태 표시 -> 사용자 제어 기능으로 확장
    - 데이터 수정 흐름 이해

    - 일정 목록에서 번호 선택하면 해당 일정 상태가 TO-DO -> DONE으로 변경 되도록 구현
```C
tasks[index].isDone = !tasks[index].isDone;
```

## DAY3

### 코드 구조 개선
- 출력 함수 분리
    - 반복 출력 코드 제거 -> 삭제/완료/수정 재사용 가능
```C
void printSimpleTasks(const std::vector<Task>& tasks)
```

### 날짜 형식 통일
- YYYY-MM-DD
- 오늘 날짜 자동 생성: std::string getToday()

### 할일 - [1] 오늘 [2] 전체
```C
// 오늘
if (tasks[i].date == today && tasks[i].type == 1)

// 전체
vector<Task> todos;
```

### waitEnter -> 입력 버퍼 문제 해결


### 지금까지 한 것
- day2
    - 프로그램 시작
    → 파일에서 데이터 불러오기
    → 메뉴 반복 실행
    → (추가 / 조회 / 삭제 / 완료변경 / 오늘할일)
    → 변경 시 저장

    - vector(자료구조), 클래스, 함수(기능 분리), 파일 입출력, for문(반복문), if문(조건문), 

- day3
    - cin() > getline() 바꿈
    - 일정 vs 할일 구분 (type 추가)
    - system("pause") 썼는데 이상해서 waitEnter() 사용
    - 2번 전체 일정: 날짜 기준 + 같은 날짜 안에서는 우선순위 정렬
    - waitEnter


### 앞으로 할 것
- 일정 수정
- 메뉴 순서 변경
- 구조 개선
- 달력 UI(최종)

```C
        [ 2026 April ]

Sun Mon Tue Wed Thu Fri Sat
        1   2   3   4
 5   6  [7]*  8   9   10  11
...

-------------------------------
📌 선택 날짜: 2026-04-07

[일정]
- 회의

[할일]
- C++ 공부 (1)
-------------------------------

[A] 추가  [E] 수정  [D] 삭제  [C] 완료
[S] 날짜 선택   [<] 이전달   [>] 다음달
[T] 오늘로 이동
[Q] 종료

입력: _
```

## DAY 4
### 오늘 한 것
- 메뉴 정리
- 수정 

### 앞으로 .. 
- GUI .. 일단 넣어두기
- 검색기능

### 무엇을 만들고자 하는가
- 달력을 보면서 일정 관리를 하고 싶음
- [일정] [할일] 같이 보고 싶음
- 화면 첫 시작은 오늘 날짜
- [할일] 안에 우선순위 1, 2, 3 이랑 오늘 해야 할 거는 (TO-DO) 표시가 되면 좋겠음
- 글자 하나 누르면 이동 되는 화면
- 지금 있는 메뉴들 다 포함하면 좋겠음

- 일단!
- ✅ 1단계: CLI(로직) 완성 (지금 코드)
    - ~~수정 기능 완성~~
    -~~ 날짜 선택해서 보기 ~~
    - 함수 분리 (addTask, deleteTask 등)
    - 메뉴 구조 안정화
    - 날짜 기반 조회 안정화

- ✅ 2단계: 콘솔 UI 업그레이드
    - 달력 출력 (ASCII)
    - 선택 날짜 기반 출력
    - 메뉴 → 키 입력 방식 변경 (A/E/D/C)

- ✅ 3단계: QT 적용(가능할지 몰겠지만 일단..)
    - 버튼 클릭
    - 달력 위젯
    - 리스트 UI

## DAY 5
### TO-DO
- try-catch 적용 (지금)
- 정렬 고도화 (날짜 + 타입 + 우선순위)
- 날짜별 출력 개선 (형식 맞추기)
- 달력 UI (ASCII)
- 구조 전환(Menu -> Calendar UI)

#### try-catch
- 입력 안정성 처리 (try-catch)
    - 사용자 입력을 문자열로 받은 뒤 숫자로 변환할 때 std::stoi() 사용
    - 잘못된 입력(문자 입력 등)이 들어오면 프로그램이 종료되는 문제 발생
    - 이를 방지하기 위해 try-catch로 예외 처리

```C++
std::string input;
std::getline(std::cin, input);

try {
    int num = std::stoi(input);
} catch (...) {
    std::cout << "숫자를 입력해주세요." << std::endl;
    return;
}
```
- try: 오류가 발생할 수 있는 코드 실행
- catch: 오류 발생 시 실행되는 코드
- 프로그램이 꺼지지 않고 계속 동작하도록 안정성 확보    

#### 구조 전환 (Menu → Calendar UI)
- 기존: 메뉴 기반 CLI 구조
- 변경: 달력 중심 UI 구조로 전환

- 변경 이유
    - 기능 중심 구조 → 사용자 경험 중심 구조로 개선
    - 실제 앱처럼 “화면 하나에서 모든 기능 제어” 구현

- 변경 사항
1. main 역할 축소
    - 기존: 메뉴 출력 + 기능 제어
    - 변경: showCalendar()만 실행하도록 단순화
```C++
while (true) {
    system("cls");
    manager.showCalendar();
}
```

2. showCalendar() 중심 구조 도입
    - 모든 기능을 하나의 UI에서 처리
    - 사용자 입력 → 함수 호출 구조
```C++
[A] 추가  [E] 수정  [D] 삭제  [C] 완료
```

3. 상태 개념 도입 (selectedDate)
    - 기존: 매번 날짜 입력
    - 변경: 선택된 날짜 유지
```C++
std::string selectedDate;
```

4. 함수 역할 분리
    - 입력 받는 함수 vs 출력 전용 함수 분리
```C++
showTasksByDate()           // 사용자 입력 포함
showTasksByDateInternal()   // 출력 전용
```

#### 고정
```C++
main
[ MAIN CALENDAR SCREEN ]
        ↓
   날짜 선택 (S or Enter)
        ↓
[ DATE DETAIL SCREEN ]
        ↓
  A / E / D / C / B / Q
```

### DAY 6

#### 오늘 할 거
- 기능 점검
- 구조 리팩토리
    1. 중복 제거
    2. 역할 분리
    3. 읽기 쉽게

    - 날짜 선택(5/30 날짜 입력 -> 오늘 날짜 안 맞음)
    - 수정: 새 내용, 새 날짜, 새 우선순위 문맥 이상
        - enter 계속 치면 오류
    - 추가/수정/삭제/ + enter -> 오류
    - 완료: enter 빠르게 넘어감, 우선순위별로 보여주면 좋겠음


### DAY 7
#### 오늘 할 거
- 구조 리팩토리(최종)
    - 

- 날짜 선택하면 바로 그 해당날짜 화면이 나오면 좋겠어. 
- 캘린더가 계속 보이면 좋을 것 같은데, 캘린더는 고정으로 하고 날짜 선택하면 밑으로 화면에 일정/할 일이 뜨게해줘.
- 글고 날짜입력하고 엔터치면 오류메시지 뜨는데 이런 것들은 어떻게 보완해?


```C++
[MAIN]
  ├─ 1. 날짜 선택
  │      ↓
  │   [DATE DETAIL]
  │      ├ add/edit/delete/done
  │      └ back → MAIN
  │
  ├─ 2. TODAY TO-DO
  │      └ back → MAIN
  │
  └─ Q 종료
```
- enter 눌렀을 때 이상함

[날짜 입력]
- 검색기능 추가(완료)
- 수정, 삭제 모두 현재 날짜 기준(완료)
[todo]
- 오늘 할 일이 없습니다 -> 엔터 눌러서 계속
- b 안눌러도 뒤로 가짐
- 완료 처리 오늘 기준


- 일정 수정할 때 우선순위 빼기
- to-do 완료 처리 되면 (done) * 표시 빼기
- 수정시 바뀐 내용 없으면?

- to - do enter 누르면 뒤로가짐

### day08

🔥 지금 코드 기준 “변경된 핵심 방향”
1. 단순 CLI → 달력 기반 UI 중심 구조
원래: 메뉴 선택 → 기능 실행
지금:
👉 showCalendar() 중심
👉 “화면 하나에서 모든 기능 제어”

✔ 이건 단순 기능 프로그램이 아니라
👉 UX 설계까지 들어간 프로젝트로 업그레이드됨

2. “일정 관리” → “일정 + 할일 분리 관리”
기존: Task 하나로 통합
지금:
type = 0 → 일정
type = 1 → 할일

✔ 이건 꽤 중요한 변화야
👉 실제 서비스 구조랑 비슷해짐

3. 우선순위 + 완료 상태 → 실제 사용 흐름 반영
TO-DO / DONE
오늘 기준 필터링
달력에 * 표시

✔ 단순 기능이 아니라
👉 “사용자 행동 유도 구조”까지 들어감

4. 입력 안정성 대폭 강화
getline + stoi + try-catch
safeInputInt()
Enter 유지 기능

✔ 이건 그냥 기능이 아니라
👉 프로그램 완성도 핵심 포인트

5. 데이터 구조 고도화
title / date / time / priority / isDone / type

✔ 초기보다 훨씬 실제 서비스 느낌

# 리눅스 - ubuntu
## 0417(금)
### Socket 통신
- 소켓통신: `클라이언트 ↔ 서버가 네트워크로 데이터를 주고받는 통신`
- 이더넷 기반 실시간 데이터 전송방식인 TCP 또는 UDP를 사용하는 양방향(말하면서 듣는 것 ex phone) 통신
    - 데이터를 요청하는 클라이언트와 데이터를 제공하는 서버의 주체가 필요하다.
- 네트워크: Net+Work의 합성어로 두 대의 기기를 연결하고 서로 통신할 수 있는 것
- 소켓: `프로그램 ↔ 네트워크 연결 통로`
전송 계층과 응용 프로그램 사이의 인터페이스 역할을 하며 떨어져 있는 두 호스트를 연결해 준다.
- IP: `“어디로 보낼지” (주소만 담당, 신뢰성 없음)`
    - 데이터를 정해진 목적지까지 전달만 하는 역할이며 비신뢰성과 비연결성의 특징을 가지고 있다. 따라서 온전한 데이터의 전달은 보장하지 않는다.(길 터주기)
- 프로토콜: `통신 규칙`
    - 컴퓨터간 원거리 통신 장비 사이에서 메시지를 주고 받을 수 있는 양식과 규칙의 체계이다.(약속)
- TCP: `느리지만 정확하게 전달 (신뢰성 O)`
    - 데이터를 온전하게 받을 수 있도록 도와주는 프로토콜이다.
- UDP: `빠르지만 유실 가능 (신뢰성 X)`
- HTTP: `웹에서 쓰는 통신 규칙`
    - 웹 브라우저와 웹 서버 간에 데이터를 주고 받기 위한 프로토콜이다.

서버 기준 6단계 - 순서대로 외우기 (소말리아)

1. socket → 소켓 생성
2. bind → 주소(IP, 포트) 연결
3. listen → 연결 대기
4. accept → 클라이언트 연결 수락
5. read/write → 데이터 송수신
6. close → 종료

![alt text](image-1.png) 

![alt text](image-2.png)

### C 저수준 파일 입출력
#### 파일
- 파일은 데이터의 집합으로, 디스크와 같은 저장 매체에서 정보를 저장하는 기본단위.
- `리눅스는 모든 것을 파일로 본다.`
    - 일반 파일 - 텍스트 파일, 바이너리 파일 등 사용자 데이터가 저장된 파일
    - 디렉토리 - 파일과 다른 디렉토리를 포함하는 파일로, 파일 시스템의 구조를 형성
    - 장치 파일 - 하드웨어 장치와의 인터페이스를 제공하는 파일로, /dev 디렉토리에 위치
    - FIFO - 프로세스 간 통신을 위한 특별한 파일
    - 소켓 - 네트워크 통신을 위한 파일로, 프로세스 간의 데이터 전송을 가능하게 함

#### 파일 디스크립터
- 파일 디스크립터: `파일을 다루는 번호`
    - 리눅스 및 UNIX 계열 운영 체제에서 프로세스가 파일이나 다른 I/O 자원(소켓, 파이프 등)에 접근하기 위해 사용하는 정수 값.
    - 운영 체제가 내부적으로 관리하는 자료구조인 파일 테이블의 인덱스로 작용한다.
- 정수 값이 할당된다.
- 기본적으로 생성되는 파일 테이블은 아래와 같다.
0	표준 입력 (stdin)
1	표준 출력 (stdout)
2	표준 에러 (stderr)

- 파일 디스크립터를 사용하여 read, write, close와 같은 시스템 콜을 통해 파일이나 I/O 자원에 접근할 수 있다. 예를 들어, 파일을 열면 운영 체제는 해당 파일에 대한 파일 디스크립터를 반환한다.
- 파일 디스크립터는 비유일성으로 열어 놓은 파일이나 자원에 대한 유일한 식별자 역할을 한다.
- 각 프로세스들은 독립적으로 파일 디스크립터를 관리한다.
- 파일 디스크립터도 열려있는 파일을 추적하고, 사용이 끝난 파일은 반드시 닫아야 한다. 그렇지 않으면 디스크립터도 누수로 이어진다.

#### 파일 입출력 관련 시스템콜

```ubuntu
open → fd 받기 → read/write → close
```

1. open(파일 열기)

```ubuntu

int fd = open("file.txt", O_RDWR | O_CREAT, 0644);

#include <fcntl.h>

int open(const char *path, int flag);
// path는 우리가 열 파일 경로, flag 는 파일을 열 때 줄 옵션

//example

// 1. 파일 열기 (읽기 및 쓰기 모드, 파일이 없으면 생성)
int fd = open("example.txt", O_RDWR | O_CREAT | O_TRUNC, S_IRUSR | S_IWUSR);
if (fd == -1) {
    perror("파일 열기 실패");
    return EXIT_FAILURE;
}
```
2. read(파일 읽기)
3. write(파일 쓰기)
4. close(파일 닫기)


## 명령어
    - `pwd: 현재 위치`
    - `cd(change directory): 이동`
    - `ls: 파일 목록(디렉토리 확인)`
        - 옵션
            - -a: 숨겨진 파일
            - -l: 자세히

    - clear: 화면 지우기        
    - ip a: ip 확인
    - mkdir: 폴더 생성/새로운 디렉토리 생성
    - .(점 하나): 현재 디렉토리
    - ..(점 두 개): 상위 디렉토리
    - rm -fr: 폴더와 안의 내용 삭제
    - gcc: 컴파일
        - gcc 파일명.c -o 실행파일 이름
        - gcc hello.c -o hello: hello라는 이름의 hello.c 파일 빌드
        - 이후 ./hello 실행

    - ~: 사용자 디렉토리
    - /: root 디렉토리
    - sudo: 관리자 권한 실행
        - 일반 사용자 → 제한된 권한, 관리자(root) → 모든 권한
    - chmod: 권한 변경       
    - u: 사용자
        - chmod u+x hello.c: hello.c 사용자의 권한에 x 할 수 있도록 권한 설정
    - mv: 파일 이동 
        - mv fd2.c ~/Work/socket : fd2.c 파일을 Work 안에 있는 socket으로 이동
    - cp: 파일 복사   

    - 위치 기준 뒤로가기 → cd -
    - 단계 위로 → cd ..
    - 명령어 기준 → ↑
    - 실행 취소 → Ctrl + C

## 0420(월)
### 1교시
#### 프로토콜
- 프로토콜: 약속 -> 이렇게 말하자~
    - 컴퓨터 상호간의 데이터 송수신에 필요한 통신규약
    - 소켓을 생성할 때 기본적인 프로토콜을 지정해야 한다.
    - 데이터 보내는 방법, 형식, 순서
- 프로토콜 체계
    - PF_INET: IPv4 인터넷 프로토콜 체계

#### 소켓 타입: 데이터를 어떻게 보낼지 방식 선택
- 소켓 = 프로토콜 + 전송방식(TCP/UDP)
- 2TYPE 소켓
    - TCP(연결형): 안정적, 순서 보장, 느림 (전화 통화 느낌)
    - UDP(비연결형): 데이터 유실 가능, 순서 보장X, 빠름(편지 던지기 느낌)
    ![alt text](image-3.png)

    ![alt text](image-4.png)

- 윈도우 운영체제의 socket 함수
    ![alt text](image-5.png)

#### IP주소 구조
- 네트워크 + 컴퓨터 위치 정보
- EX) 1.2.3.4 (앞: 네트워크 / 뒤: 호스트(컴퓨터))
- 옛날기준: A/B/C 클래스(첫 바이트로 구분)

- IP는 네트워크 주소와 호스트 주소의 경계 구분 가능
    - 첫번 째 바이트 정보만 참조해도 IP주소의 클래스 구분 가능
    ![alt text](image-6.png)

#### 포트 번호
- 한 컴퓨터 안에서 프로그램 구분
- EX) IP: 집 주소, PORT: 방 번호 / 80->웹, 443->HTTPS

- 소켓의 구분에 활용되는 PORT 번호
    ![alt text](image-7.png)

- IPv4 기반의 주소표현을 위한 구조체
    ![alt text](image-8.png)

#### sockaddr_in 구조체
- 주소 + 포트를 묶는 구조체
- 소켓에서 "누구랑 통신할지" 담는 그릇
```C
struct sockaddr_in {
    short sin_family;   // PF_INET
    unsigned short sin_port; // 포트 번호
    struct in_addr sin_addr; // IP 주소
};
```
- 구조체 sockaddr_in의 멤버에 대한 분석
    ![alt text](image-9.png)

#### 바이트 순서
- 컴퓨터마다 숫자 저장 방식이 다름
- 리틀 엔디안(Intel)
- 빅 엔디안(네트워크)
- 그대로 보내면 값이 뒤집힘 -> 변환 필요
- 무조건 네트워크 방식(빅 엔디안)으로 맞춤

```C
htons()  // short
htonl()  // long
ntohs()
ntohl()

// 내 컴퓨터 -> 네트워크용으로 변환
```

- 바이트 순서(Order)와 네트워크 바이트 순서
    ![alt text](image-10.png) 

- 바이트 순서 변환
    ![alt text](image-11.png)

### 1교시 수업 정리
- 소켓 만들 때
    - 프로토콜 선택 (PF_INET)
    - 타입 선택 (TCP/UDP)
    - IP + PORT 준비 (sockaddr_in)
    - 바이트 순서 변환
    - 통신 시작

- 어디(IP), 누구(PORT), 어떻게(TCP/UDP) 보낼지 정하는 게 소켓

### 2교시
- 인터넷 주소 초기화 과정
    ![alt text](image-14.png)

    ![alt text](image-13.png)

#### INADDR_ANY
- INADDR_ANY: 모든 IP에서 들어오는 연결 다 받겠다.
```C
addr.sin_addr.s_addr = htonl(INADDR_ANY);
```
- 사용이유
    - 서버는 보통 IP가 여러 개일 수 있음
    - 하나만 지정하면 -> 그 IP로만 접속 가능
    - INADDR_ANY 쓰면 -> 어떤 IP로 들어와도 다 받음

    ![alt text](image-15.png)   

- 서버 주소 설정 흐름
```C
struct sockaddr_in addr;

addr.sin_family = AF_INET;  // AF_INET: IPv4 쓰겠다
addr.sin_addr.s_addr = htonl(INADDR_ANY); //INADDR_ANY: 아무 IP나 OK
addr.sin_port = htons(9190);    // 9190: 포트 번호

// 이걸 bind()에 넣는 거
// 이대로 외우기
```
- ./hserver9190
    ![alt text](image-16.png)

- ./hclient 127.0.0.1 9190    
    ![alt text](image-18.png)

#### 소캣에 인터넷 주소 할당하기
- 
    ![alt text](image-19.png)

    ![alt text](image-20.png)

#### TCP/IP 프로토콜 스택
- TCP/IP 계층
    - 인터넷은 층으로 나뉨

```C
[응용]  → 우리가 만든 프로그램 (소켓 코드)
[전송]  → TCP / UDP
[네트워크] → IP
[링크] → 실제 전선/와이파이
```    

- 우리는 응용 계층
- TCP/UDP는 전달 방식 담당
- IP는 주소 담당

![alt text](image-21.png)

#### LINK & IP 계층

![alt text](image-22.png)

#### TCP/UDP 계층

- TCP/UDP 계층의 기능 및 역할
    - 실제 데이터의 송수신과 관련 있는 계층(전송 계층)
    - TCP: 데이터 전송 보장 - 신뢰성을 보장하기 때문에 UDP에 비해 복잡한 프로토콜
    - UDP: 데이터 전송 보장X

#### 연결요청 대기 상태로의 진입

![alt text](image-23.png)

![alt text](image-24.png)

#### 클라이언트의 연결 요청 수락

![alt text](image-27.png)

![alt text](image-25.png)

![alt text](image-28.png)

![alt text](image-29.png)

![alt text](image-30.png)

## 0421(화)

![alt text](image-31.png)

#### 에코 클라이언트

#### UDP 소켓의 특성과 동작 원리
![alt text](image-32.png)

- UDP 소켓은 연결이라는 개념 존재 X
![alt text](image-33.png)

- UDP 기반의 데이터 입출력 함수
![alt text](image-34.png)

- UDP 기반의 에코 서버와 클라이언트
![alt text](image-35.png)

- 윈도우 기반 sendto, recvfrom 함수
![alt text](image-36.png)

#### 일반적인 연결 종료의 문제점
- close 및 closesocket 함수의 기능
![alt text](image-37.png)

- 소켓의 Half-close
![alt text](image-38.png)

- 우아한 종료를 위한 Shut-down 함수와 그 필요성
![alt text](image-39.png)

- Half-close 기반 파일 전송 프로그램
![alt text](image-40.png)

#### 도메인 이름과 DNS 서버
![alt text](image-41.png)

#### 소켓의 옵션
![alt text](image-42.png)

#### 옵션정보의 설정에 사용되는 함수
![alt text](image-43.png)

#### 소켓의 타입 정보(TCP or UDP 확인)

#### 주소의 재할당
![alt text](image-44.png)

#### Nagle 알고리즘
![alt text](image-45.png)

- 중단
![alt text](image-46.png)

#### 소켓의 옵션정보를 확인하는 getsockopt 함수
![alt text](image-47.png)

## 0422(수)
### 다중접속서버(I/O Multiplexing) & Asyncronous I/O
#### 멀티프로세서
- 다중 접속 서버를 구현하는 방법
- 1. 멀티프로세스 기반 서버: 다수의 프로세스를 생성하는 방식
- 2. 멀티 플렉싱 기반 서버: 입출력 대상을 묶어서 관리하는 방식
- 3. 멀티 스레딩 기반 서버: 클라이언트 수만큼 스레드를 생성하는 방식

- 1. 멀티프로세서
    - 프로세서: 실행중인 프로그램

```c
#include <unistd.h>

pid t fork(void); // 성공 시 프로세서 ID, 실패 시 -1 반환

```
- 부모 프로세서는 fork() 함수를 호출하여 자식 프로세서 생성
    - 부모 프로세서: Fork 함수의 반환 값을 가지는 프로세서
    - 자식 프로세서: Fork 함수의 반환 값으로 0을 가지는 프로세서

- 좀비 프로세서
    ![alt text](image-48.png)    

- 2) 프로세서 종료    
    ![alt text](image-49.png)

    ![alt text](image-50.png)

- 3) 프로세서 종료 알림
    - 시그널이란 운영체제가 어떤 특정한 상황을 프로세서에게 알리는 것으로, 자식프로세서의 종료를 운영체제로부터 전달 받음.

```c
#include <signal.h>

void* signal(int signo, void (*handler)(int));
    // 시그널 발생 시 호출되도록 이전에 등록된 함수의 포인터 반환
```
- signo 상수

- 프로세서간 통신
    - 프로세서간 데이터 전달 불가능
    - 운영체제가 별도의 메모리공간을 마련하여 프로세서간 통신 지원.

```c
#include <unistd.h>

int pipe(int filedes[2]);
    // 성공 시 0, 실패 시 -1 반환
```    

![alt text](image-51.png)

- 파이프 생성과 디스크립터 복사
    ![alt text](image-52.png)

    ![alt text](image-53.png)

- 프로세서간 양방향 통신: 적절한 방식

#### 멀티스레드
- 프로세서
- 스레드

```c
#include <pthread.h>

int pthread_create ()
```
thread
attr
start_routine
arg

- 하나의 운영체제 내에서는 둘 이상의 프로세서가 생성되고, 하나의 프로세서 내에서는 둘 이상의 스레드가 생성된다.

## 0423(목)
### IO 멀티플렉싱 - select, poll, epoll
#### IO 멀티플렉싱
- 하나의 프로세스(또는 스레드)가 여러 클라이언트의 I/O를 동시에 처리하는 방식
- 여러 소켓을 감시하다가 이벤트가 발생한 소켓만 선택적으로 처리
- 멀티 프로세서의 단점
- 멀티 프로세스/스레드 방식의 단점 보완
    - 높은 메모리 사용
    - 컨텍스트 스위칭 비용 발생
- 적은 자원으로 다수의 클라이언트 처리 가능

#### select 기반 IO 멀티플렉싱
- fd_set 구조체: 여러 파일 디스크립터(fd)를 집합 형태로 관리하는 구조체
    - FD_ZERO : 집합 초기화
    - FD_SET : fd 추가 (감시 대상 등록)
    - FD_CLR : fd 제거
    - FD_ISSET : 해당 fd에 이벤트 발생 여부 확인

- 파일 디스크립터의 설정
    - 감시할 fd를 fd_set에 등록
    - select 호출 전 초기화 및 복사 필요(select 실행 시 값이 변경되기 때문)

#### select 함수
- 여러 fd를 동시에 감시하여 이벤트 발생 여부를 확인하는 함수
- 읽기/쓰기/예외 상황 감지 가능
- 이벤트 발생 시 해당 fd만 선택적으로 처리
- 호출 시까지 대기(blocking)

#### epoll


## 0424(금)
### UDP 소켓 특징
- UDP: 연결 지향X(비연결형) 그래서 listen(), accept() 필요 없음
    - 데이터 보낼 때마다 sendto()에서 목적지 주소를 직접 지정(= 보낼 때마다 주소 적어서 던진다)

- connect() 사용하는 경우(예외)
    - UDP도 connect() 가능, 이 경우:
        - 상대 주소를 한 번 저장
            - 이후 sendto()/recvfrom() x
            - write()/read() o
        - 즉, UDP인데 TCP처럼 쓰는 느낌

```C
ssize_t sendto(
    int sockfd,
    void *buff,
    size_t nbytes,
    int flags,
    struct sockaddr *to,
    socklen_t addrlen
);
```            
- 반환값
    - 성공: 보낸 바이트수(일반적으로 nbytes)
    - 실패: -1

#### 매개변수
1. sockfd: 소켓 번호(파일 디스크립터)
    - socket()으로 만든 것
2. buff: 보낼 데이터의 시작 주소
    - char msg[] "hello";
3. nbytes: 보낼 데이터 크기
    - strlen(msg)
4. flags: 옵션(보통 0 사용)
5. to: 목적지 주소 정보
    - struct_sockaddr_in을 캐스팅해서 넣음
6. addrlen: 주소 구조체 크기
    - sizeof(struct sockaddr_in)

#### 핵심
- UDP 송신 흐름:
```C
socket() 생성
→ 주소 구조체 설정 (IP + PORT)
→ sendto()로 전송
```

- UDP는 연결 없음
- sendto 하나로 끝
- 주소를 매번 넣어야 함
- 빠르지만 신뢰성 없음

- TCP: 연결하고 계속 대화
- UDP: 주소 적고 던지면 끝


### TCP vs UDP 소켓 정리
#### 소켓 타입 (생성 단계)
```C
socket(PF_INET, SOCK_STREAM, 0); // TCP
socket(PF_INET, SOCK_DGRAM, 0);  // UDP
```
- SOCK_STREAM → TCP (연결형)
- SOCK_DGRAM → UDP (비연결형)

#### 전체 동작 흐름
- TCP (SOCK_STREAM)
```C
// 서버
socket()
bind()
listen()
accept()
read() / write()
close()

// 클라이언트
socket()
connect()
read() / write()
close()
```

- UDP (SOCK_DGRAM)
```C
// 서버 / 클라이언트 공통
socket()
bind()      // 서버만 주로 사용    
sendto()
recvfrom()
close()
```

#### 멀티캐스트
- 멀티캐스트
    - 패킷의 무한 루프 방지
    - 멀티캐스트 전송 범위 제어

- ip_mreq
- 라우팅, TTL
- new_sender.c

## 0427(월)
### 라즈베리파이
#### 설치
1. sd카드를 카드리더에 넣어서 컴퓨터에 등록
2. Raspberry pi image 설치
3. RealVNC Viewer 설치
4. card reader에 sd카드 넣고 연결
5. putty 설정 rip 192.168.0.7
6. 고정 IP
    - wifi 설정 - IPv4 setting 192.168.0.100
- sudo apt update, sudo apt upgrade
- VNC 
    - sudo raspi-config
    - Interface Option
    - I3 VNC
- 한글 설치
    - sudo apt install fonts-nanum fonts-nanum-extra
    - sudo apt install fonts-unfonts-core
    - sudo apt install ibus
    - sudo apt install ibus-hangul
    - ibus-setup
        - inputMethod > Add > Korean 검색 > Korean - Hangul > Add > Key 등록

#### pinout
- GND = 0v(기준 전압) / 캐소드 타입
- 5v = Vcc = 1 / 애노드 타입

- 라즈베리파이는 왜 DC를 쓰는가
    - 라즈베리파이 같은 컴퓨터는 안정적인 전압과 일정한 전류가 필요하다.
    - 그래서: AC 직접 사용 하지 않고, DC로 변환해서 사용(보통 5V DC 사용)

- 애노드(Anode) / 캐소드(Cathode)
    - 애노드: 전류가 들어가는 쪽 (+)
    - 캐소드: 전류가 나가는 쪽 (-)
    - 기준: 전류(+) 흐름 기준

- LED 기준
    - 긴 다리 → 애노드(+)
    - 짧은 다리 → 캐소드(-)

- 연결 방식: + → 애노드 → LED → 캐소드 → -
- 반대로 연결하면: 전류 흐르지 않음, LED 안 켜짐

- `라즈베리 파이에서는 GPIO(+ 느낌) → 애노드 → 캐소드 → GND`
- 직렬: 전류 일정, 전압 증가
- 병렬: 전압 일정, 전류 증가
- KCL, KVL

- `긴 다리 3.3V면 = 공통 애노드 = on/off 반대로 쓰는 게 정석`

## 0428(화)
### 라즈베리 파이 2
- pinctrl set 14 op/dh/dl
    - op → output (출력 모드)
    - dh → drive high (3.3V 출력)
    - dl → drive low (0V 출력)

- pinout: 전자 회로보기

## 가상환경 생성

![alt text](image-54.png)

프로젝트 폴더에서:
python3 -m venv .venv
그러면 .venv 폴더가 생김

활성화 (여기서 핵심)
source .venv/bin/activate

- 온오프 스위치 만들기
- 이벤트 기반으로 바꾸기
- 풀업/풀다운 저항으로 바꾸기

## CDS
- CDS 조도센서 값 읽기
    - CDS 센서는 빛의 밝기에 따라 저항값이 변하는 센서이다.
    - 이 저항 변화는 전압 변화로 변환되고, ADC를 통해 0~255 범위의 값으로 변환된다.
    - 밝기가 강하면 값이 변하고, 빛을 가리면 반대로 값이 변화한다(회로 구성에 따라 증가/감소 방향은 달라질 수 있음).
    - 즉, 출력값(val)은 주변 밝기 정도를 숫자로 표현한 값이다.

- vcc, gnd 마지막에 꽂기
- 140 0 160 1

#### 실질적인 값을 또다른 형태로 변환(범위 바꾸기)
- 원래 값(raw)을 0 ~ 100으로 변환

```python
import smbus
import time
from gpiozero import LED

bus = smbus.SMBus(1)
ADDR = 0x48

A0 = 0x40

led = LED(17)

# 👉 여기 기준값 (나중에 조절)
MIN_VAL = 100
MAX_VAL = 170

def read_cds():
    bus.write_byte(ADDR, A0)
    bus.read_byte(ADDR)
    return bus.read_byte(ADDR)

try:
    while True:
        val = read_cds()

        # 🔥 0~100 변환
        scaled = int((val - MIN_VAL) * 100 / (MAX_VAL - MIN_VAL))

        # 범위 보정 (0~100 유지)
        if scaled < 0:
            scaled = 0
        if scaled > 100:
            scaled = 100

        print("RAW:", val, "SCALED:", scaled)

        # LED 기준도 스케일 기준으로 변경
        if scaled < 30:
            led.on()
        else:
            led.off()

        time.sleep(0.5)

except KeyboardInterrupt:
    print("종료")
```

#### CDS 값 스케일링
- CDS 센서 값은 보통 특정 범위(예: 120~170)에서만 변화해서 직관적으로 보기 어렵다.
- 이를 0~100 범위로 변환(스케일링)하면 밝기 변화를 더 쉽게 파악할 수 있다.
```python
scaled = int((val - MIN_VAL) * 100 / (MAX_VAL - MIN_VAL))
```
- MIN_VAL: 어두울 때 값
- MAX_VAL: 밝을 때 값

- 결과적으로
- RAW 값 → 센서 원본 데이터
- SCALED 값 → 사람이 이해하기 쉬운 0~100 기준 값

#### 후방감지 센서
- 30cm 거리부터 소리 나게.
- 가까워질수록 소리 빠르게.
- 라즈베리 파이로 만들고 있음. 부저, HC-SR04 사용

#### lcd
- lcd에 구구단 출력(2-9단)
- 초음파 센서 거리 lcd에 표시
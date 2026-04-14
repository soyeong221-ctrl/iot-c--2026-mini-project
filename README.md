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
 └── showCalendar()  ← 메인 화면 (절대 고정)
       ├── 달력 출력
       ├── 선택 날짜
       ├── 일정 출력
       └── 입력 처리 (A/E/D/C/S)
```

### DAY 6

#### 오늘 할 거
- 기능 점검
- 구조 리팩토리
- 달력 UI 완성


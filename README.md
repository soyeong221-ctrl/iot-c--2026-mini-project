# iot-c--2026-mini-project
## PlanMate 개발과정

## DAY1
### 프로젝트 제안서

## DAY2
### 1. 프로젝트 초기 설정
- 목표: 사용자 입력 기반 프로그램 이해
    - Visual Studio에서 C++ 콘솔 프로젝트 생성
    - main.cpp, Task.h 파일 분리
    - 프로그램 실행 구조(메뉴 기반 CLI) 설계

### 2. 일정 데이터 구조 설계
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

### 3. 일정 저장 구조 구현(vector)
- 여러 개의 일정을 저장하기 위해 vector 사용
    - 동적 배열 사용, 여러 데이터를 효율적으로 관리하는 방법

```c
vector<Task> tasks;
```

### 4. 일정 추가 기능 구현
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

### 5. 전체 일정 조회 기능 구현
- 반복문을 사용하여 모든 일정 출력
    - 반복문 활용
    - 배열/리스트 순회

```C
for (int i = 0; i < tasks.size(); i++) {
    cout << tasks[i].title << endl;
}
```

### 6. 일정 삭제 기능 구현
- 사용자가 선택한 번호에 해당하는 일정 삭제
    - vector 데이터 삭제
    - 사용자 입력(1번)과 인덱스(0번)차이 이해
    - 예외 처리 필요성

```C
int index = num - 1;
tasks.erase(tasks.begin() + index);    
```

### 7. 일정 완료 상태 기능 추가
- 일정의 완료 여부를 관리하기 위해 상태 값 추가
    - 데이터에 상태 추가
    - 단순 저장 -> 관리 기능 확장

```C
bool isDone;  // 일정 생성시 기본값 설정

t.isDone = false;  // 출력 시 상태 표시

if (tasks[i].isDone)
    cout << "[DONE]";
else
    cout << "[TO-DO]";
```

### 8. 우선순위 기반 정렬 기능 구현
- sort() 사용하여 중요한 일정이 먼저 나오도록 정렬
    - 정렬 알고리즘 활용
    - 기준을 설정하여 데이터 정렬

```C
sort(tasks.begin(), tasks.end(), [](Task a, Task b) {
    return a.priority < b.priority;
});
```

### 9. 오늘 할 일 필터링 기능 구현
- 사용자가 입력한 날짜와 일치하는 일정만 출력하도록 구현
    - 조건문 활용
    - 데이터 필터링 개념 이해

```C
if (tasks[i].date == today)
```

### 10. 파일 저장 기능 구현(데이터 유지)
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

### 11. 파일 불러오기 기능 구현
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

### 12. 프로그램 시작 시 데이터 자동 로드
- 프로그램 실행 시 저장된 데이터를 자동으로 불러옴
- 기존 일정 유지

- 프로그램 실행 흐름과 데이터 초기화 이해

```C
vector<Task> tasks;
loadTasks(tasks);
```

### 13. 데이터 변경 시 자동 저장 처리
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

### 14. 일정 완료 상태 변경 기능 구현
- 사용자가 직접 일정 완료 상태를 변경할 수 있도록 기능 추가
    - 단순 상태 표시 -> 사용자 제어 기능으로 확장
    - 데이터 수정 흐름 이해

    - 일정 목록에서 번호 선택하면 해당 일정 상태가 TO-DO -> DONE으로 변경 되도록 구현
```C
tasks[index].isDone = !tasks[index].isDone;
```

## DAY3

- 일정/할일 구분
- 수정 기능

+ 달력 넣기
- 달력 날짜 연계 되는지
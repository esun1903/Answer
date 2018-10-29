# 면접 질문 리스트

[TOC]

## DB

### ORM(Object Relational Mapping)

* 객체와 관계와의 설정
* 객체와 관계형 DB를 Mapping해준다.
* 객체와 테이블을 Mapping하기 때문에 SQL을 직접 날리는 것이 아니라 마치 자바에서 라이브러리 사용하듯이 사용하면 된다.
* 객체와 관계형 데이터에비스와의 설정을 자동으로 해준다.
* 관계형 데이터베이스의 데이터를 객체형 데이터처럼 사용할 수 있다.
* JPA, Hibernate, MyBatis
* 장점
  * 객체 지향적인 코드로 인해 더 직관적이고 비즈니스 로직에 집중할 수 있게 도와준다.
  * 선언, 할당, 종료 같은 부수적인 코드가 줄어든다.
  * 재사용 및 유지 보수의 편리성이 증가한다.
  * DBMS에 대한 종속성이 줄어든다.
  * 절차적, 순차적 접근이 아닌 객체 지향적 접근으로 인해 생산성이 증가한다.
* 단점
  * 모든 기능을 ORM으로만 작성하기에는 쿼리가 복잡해지면 쓰기 어렵다.
  * 많은 수의 레코드를 잦은 빈도로 벌크 수행
  * ORM으로만 완벽하게 서비스를 구현하기가 어렵다.
  * 프로시저가 많은 시스템에선 ORM의 객체 지향적인 장점을 활용하기 어렵다.

### MySQL 엔진

* InnoDB
  * 기본값 스토리지 엔진
  * 트랜잭션 safe, 커밋과 롤백, 데이터 복구 기능을 제공한다.
  * row-level locking을 제공한다.
  * 데이터를 clusterd index에 저장하여 pk기반 query의 I/O 비용을 줄인다.
  * fk제약을 제공하여 데이터 무결성을 보장한다.
* MyISAM
  * 트랜잭션을 지원하지 않는다.
  * 테이블 단위의 locking을 제공한다.
  * 특정 세션이 테이블을 변경하는 동안 테이블 단위로 lock이 잡힌다.
* Archive
  * 로그 수집에 적합한 엔진
  * 데이터가 메모리 상에서 압축되고 압축된 상태로 디스크에 저장된다.
  * row-level locking이 가능하다.
  * 한번 insert된 데이터는 update/delete가 불가능하다.
  * 인덱스를 지원하지 않는다.
  * 거의 가공하지 않을 윈시 로그 데이터를 관리하는데 효율적이고, 테이블 파티셔닝도 지원한다.
  * 트랜잭션은 지원하지 않는다.

### 인덱스

* RDBMS에서 검색 속도를 높이기 위해 사용하는 기술
* 색인
* 해당 테이블의 컬럼을 색인화(따로 파일로 저장)하여 검색시 테이블 전체를 full scan 하는 것이 아니라 색인화 되어있는 index 파일을 검색하여 검색 속도를 빠르게 한다.
* B+ 트리로 저장된다.
* index로 설정한 컬럼 값이 변경되거나 추가되면, 인덱스 역시 변경된다. 따라서 적절하게 인덱스를 설정해야 한다.
* 데이터의 삽입, 삭제가 빈번한 경우 index의 성능이 떨어진다. 매번 B+트리를 수정해야 하기 때문이다.
* index가 데이터베이스 공간을 차지하기 때문에 추가적인 공간이 필요하다.(10%)
* index를 생성하는데 시간이 많이 소요될 수 있다.
* SELECT의 WHERE / JOIN시에만 인덱스가 사용되며 SELECT의 검색 속도를 빠르게 하는것이 목적이다.
* WHERE의 타겟이 되는 컬럼을 인덱스로 만드는 것이 좋다.
* 데이터 중복도가 높은 컬럼은 인덱스로 만들어도 효과가 없다.
* 외래키가 사용되는 컬럼은 인덱스를 생성해 주는 것이 좋다.
* 사용하지 않는 인덱스는 제거하는 것이 좋다.

### 정규화

* 관계형 데이터베이스의 설계에서 중복을 최소화하게 데이터를 구조화 하는 프로세스이다.
* 관계를 재구성하여 작고 잘 조직된 관계를 생성하는 것에 있다.
* 성능상의 이유로 비정규화 될 수 있다.

### 트랜잭션

* 작업의 수행 단위
* 트랜잭션이 안전하게 수행된다는 것을 보장하기 위한 약어 : ACID
* Atomicity : 원자성
* Consistency : 일관성
* Isolation : 고립성
* Durability : 지속성

### Partitioning(파티셔닝)

* 수직 파티셔닝
* 큰 테이블이나 인덱스를 관리하기 쉬운 단위로 분리하는 방법을 의미한다.
* 물리적인 파티셔닝으로 인해 전체 데이터의 훼손 가능성이 줄어들고 데이터 가용성이 향상된다.
* 큰 테이블을 제거하여 관리를 쉽게 해준다.
* 특정 DML과 쿼리의 성능을 향상시킨다.
* 주로 대용량 데이터 Write 환경에서 효율적이다.
* 많은 데이터 삽입이 있는 OLTP(Online Transaction Processing) 시스템에서 삽입 작업들을 분리된 파티션들로 분산시켜 경합을 줄인다.
* 테이블간 join에 대한 비용이 증가한다.
* 테이블과 인덱스를 별도 파티션 할 수는 없다. 테이블과 인덱스를 같이 파티셔닝해야 한다.

### Sharding(샤딩)

* 수평 파티셔닝
* 대용량의 데이터를 처리하기 위해 테이블을 수평 분할하여 데이터를 분산 저장하고 처리하는 것을 의미한다.
* 같은 타입의 데이터를 다수의 데이터베이스에 쪼개서 저장하는 것
* 샤딩 알고리즘이 매우 쉽게 일반화가 가능하기에 애플리케이션 레벨이나 데이터베이스 레벨이서 구현이 가능하다.

### Replication(리플리케이션)

* 두 개 이상의 DBMS 시스템을 Master/Slave로 나눠서 동일한 데이터를 저장하는 방식
* Master에는 데이터의 수정 사항을 반영만 하고 Replication을 하여 Salve에 실제 데이터를 복사한다.
* 삽입/수정/삭제는 Master가 담당하고 조회는 Slave가 담당해서 성능 향상 효과를 얻을 수 있다.
* 로그 기반 복제
  * Statement Based : SQL을 복사하여 진행
  * Row Based : SQL에 따라 변경된 Row만 기록하는 방식
  * Mixed : 기본적으로 statement Based로 진행하면서 필요에 따라 Row Based를 사용한다.

### MongoDB

* 도큐먼트 지향 데이터베이스 시스템이다.
* NoSQL
* 똑같은 조건으로 설계했을 시 RDBMS보다 빠르다.
* 트랜잭션을 지원하지 않는다.
* 도큐먼트의 집합을 컬렉션이라고 한다. = RDBMS에서 테이블
* Join을 지원하지 않는다.
* 데이터 갱신 및 입력이 바로 디스크에 쓰이지 않는다.(비동기식) 따라서 데이터가 유실될 가능성이 있다.
* 자체적으로 스케일 아웃을 지원한다.

### Redis

* 인 메모리 데이터베이스
* NoSQL
* 키-값 구조
* C 코드로 개발되었다.
* 데이터를 디스크에 저장하기 때문에 데이터 유실 위험이 memcached에 비해 적다.
* 클러스터링을 지원하지 않는다.
* 샤딩을 통해 확장한다.
* 데이터를 저장하는 방식
  * snapshotting
    * save
      * 순간적으로 redis를 정지시키고, 그 때의 스냅샷을 디스크에 저장한다.
    * blocking
      * 별도의 프로세스를 띄운후, 그 당시의 메모리 스냅샷을 디스크에 저장하며 redis는 정상적으로 작동한다.
  * AOF(Append On File)
    * redis의 모든 write/update 연산 자체를 모두 log 파일에 기록해 두었다가 서버가 재 시작될 때 기록된 로그를 통해 데이터를 복구한다.
    * 기본적으로 non-blocking call이다.

### Datasocurce

* 커넥션 풀의 커넥션을 관리하기 위한 객체이다.
* 데이터소스 객체를 통해서 필요한 커넥션을 획득, 반납한다.
* JNDI(Java Naming and Directory Interface) Server를 통해 이용된다.

### HikariCP

* 스프링 부트 2.0의 기본 jdbc cp이다.

## Java

### Java 8 변경 사항

* 인터페이스에 디폴트 메소드 추가
* 스트림 api
* 람다
* 함수형 프로그래밍

### Interface(인터페이스)

* 클래스들이 구현해햐 하는 동작을 지정하는데 사용되는 추상형
* 규약, 규제
* 추상 클래스의 극단적인 경우
* 클래스와 달리 다중 상속(다중 구현)이 가능하다.
* 객체의 내부 구조를 알 필요 없이 인터페이스의 메소드만 알고 있으면 된다는 장점
* 다형성을 이용한 인터페이스의 구현 객체를 손쉽게 교체할 수 있다.
* 자바 8에서 인터페이스가 가질 수 있는 것
  * 상수 필드(public static final) : 기존, 생략 가능
  * 추상 메소드(public abstract) : 기존, 생략 가능
  * 디폴드 메소드(public default) : 새로 추가됨
    * 이전에 개발한 구현 클래스를 그대로 사용하지 않고 변경하지 않으면서, 새롭게 개발하는 클래스는 디폴트 메소드를 활용해 새로운 기능을 만들 수 있다. 

### 다형성

* 하나의 메소드나 클래스가 상황에 따라 다양한 방법으로 동작하는 것을 의미한다.
* 오버로딩(Overloading)은 다형성의 가장 대표적인 예
* 다형성의 조건
  * 공통의 부모
  * 공통의 메소드 재정의
  * 부모 타입의 변수로 호출

### Generic(제네릭)

* 다양한 타입의 객체들을 다루는 메소드나 컬렉션 클래스에 컴파일 시의 타입 체크를 해 주는 기능이다.
* 클래스 내부에서 사용할 데이터 타입을 나중에 인스턴스를 생성할 때 확정하는 것을 말한다.
* 타입 안정성을 제공한다.
* 타입 체크와 형 변환을 생략할 수 있으므로 코드가 간결해진다.

### mutable & immutable

* mutable
  * 변경 가능 객체
  * 상태를 바꿀 수 있는 객체
  * 최초 생성 이후에도 자유롭게 값 변겨 가능
* immutable
  * 변경 불가 객체
  * 상태를 바꿀 수 없는 객체
  * 최초 생성 이후로는 값 변경 불가

### OOP(Object-Oriented Programming, 객체 지향 프로그래밍)

* 프로그램을 수 많은 객체라는 기본 단위로 나누고 이 객체들의 상호 작용으로 프로그램을 서술하는 방식
* 객체들을 데이터 묶음 뿐만 아니라 하나의 역할을 수행하는 메소드와 데이터의 묶음
* 큰 문제를 작게 쪼개는 것이 아니라, 먼저 작은 문제들을 해결할 수 있는 객체들로 만든 뒤, 이 객체들을 조합해서 큰 문제를 해결하난 상향식(Buttom up) 해결법 도입
* 캡슐화(Encapsulation)
  * 프로그램의 세부 구현을 외부로 드러나지 않도록 특정 모듈(클래스) 내부로 감추는 것이다.
  * 접근 제어 지시자를 통해 외부에서 접근을 제어한다.
* 상속(Inheritance)
  * 자식 클래스가 부모 클래스의 특성과 기능을 그대로 물려받는 것을 말한다.
  * 재정의를 통해 자식 클래스에서 상속받은 기능만들 재 정의해 사용이 가능하다.
* 다형성(Polymorphism)
  * 하나의 변수, 함수 등이 상황에 따라 다른 의미로 해석될 수 있다.
  * 오버로딩

### Reflection

* 객체를 통해 클래스의 정보를 분석해 내는 프로그램 기법
* 자바는 동적으로 객체를 생성하는 기술이 없다. 그래서 동적으로 인스턴스를 생성하는 Reflection으로 그 역할을 대신하게 된다.
* Reflection은 구체적인 클래스 타입을 알지 못해도, 그 클래스의 메소드, 타입, 변수들을 접근할 수 있도록 해주는 자바 API이다.

### Annotation

* 자바 소스 코드에 추가하여 사용할 수 있는 메타데이터의 일종이다.
* @기호를 붙여서 사용한다.
* JDK 1.5버전 이상에서 사용 가능하다.
* CompileTime, Runtime시에 해석될 수 있다.
* 클래스 파일에 포함되어 컴파일러에 의해 생성된 후 JVM에 포함되어 작동한다.
* @Target으로 어노테이션이 적용할 위치를 선택한다.
* @Retention으로 컴파일러가 어노테이션을 다루는 방법을 설정한다.

### Enum

* 열거 타입, 변수를 선언할 때 변수 타입으로 사용할 수 있다.
* IDE의 지원을 받을 수 있다.(자동 완성, 오타, 텍스트 리팩토링)
* 허용 가능한 값들을 제한할 수 있다.
* 리팩토링시 변경 범위가 최소화된다. 내용의 추가가 필요하더라도, Enum 코드외에 수정할 필요가 없다.
* 완전한 기능을 갖춘 클래스이다.

### 접근 제어 지시자

| 지시자    | 클래스 내부 | 동일 패키지 | 상속받은 클래스 | 이외의 영역 |
| --------- | ----------- | ----------- | --------------- | ----------- |
| private   | :o:         | :x:         | ❌               | ❌           |
| default   | :o:         | :o:         | :x:             | ❌           |
| protected | :o:         | :o:         | :o:             | ❌           |
| public    | :o:         | :o:         | :o:             | ⭕️           |

* private : 클래스 내부(메소드)에서만 접근을 허용한다.
* default : 클래스 내부와 동일 패키지에서만 접근을 허용한다.
* protected : 클래스 내부와 동일 패키지 상속받은 클래스에서만 접근이 가능하다.
* public : 어디서든 접근이 가능하다.

### String, StringBuilder, StringBuffer

* String은 immutable, private final char[] 형태
* immutable인 이유 : 퍼포먼스, 동시성, GC
* StringBuilder, StringBuffer은 mutable
* StringBuffer는 Thread safe, StringBuilder는 Thread safe하지 않다. 따라서 Multi Thread 환경에서는 StringBuffer를 사용해야 한다.

## JVM(Java Virtual Machine)

- 자바 가상 머신
- 자바 애플리케션을 클래스 로더를 통해 읽어 들어 자바 API와 함께 실행하는 것이다.
- 자바 바이트 코드를 실행할 수 있는 주체이다.
- 인터프리터나 JIT컴파일 방식으로 다른 컴퓨터 위에서 자바 바이트 코드를 실행할 수 있도록 구현한다.
- 자바 바이트 코드는 플랫폼에 독립적이다.
- 이론적으로 모든 자바 프로그램은 CPU나 운영체제의 종류와 무관하게 동작할 것을 보장한다.
- 스택 기반, 대다수의 명령어가 스택 선두에서 피연산자를 택하고 다시 스택에 넣는다.
- 포인터를 지원하지만, C처럼 주소 값을 임의로 조작이 가능한 포인터 연산을 지원하지 않는다.
- GC를 사용해 자원을 관리한다.
- 메모리 관리가 가능하다.

### Class Loader

* 각 소스코드 파일을 오브젝트 파일로 변환시켜주는 역할을 한다.
* Loader(로더)
  * 목적 프로그램(기계어 파일)을 실행 가능한 파일로 변환하기 위해 컴퓨터의 주 기억 장소를 할당(Allocation)하거나, 여러개의 목적 프로그램을 연계 편집하여 CPU가 처리할 수 있는 프로그램으로 변환시킨다.
  * 프로그램을 실행하기 위하여 프로그램을 보조 기억 장치로부터 컴퓨터의 주 기억 장치에 올려 놓는 작업을 수행
  * 목적 프로그램들 끼지 연결(Linking)시키거나, 주 기억 장치를 재배치(Relocation)하는 증 포괄적인 작업을 수행
  * 할당(Allocation) -> 연결(Linking) -> 재배치(Relocation) -> 적재(Loading) 순으로 진행
* Linker : 모든 오브젝트 파일들을 하나의 오브젝트 파일로 합친다.
* JVM내로 Class(.class)를 로드하고, 링크를 통해 배치하는 작업을 수행하는 모듈이다.
* Runtime 시점에 Dynamic으로 Class를 Load한다.
* jar 파일 내 저장된 Class들을 JVM 위에 올리고, 사용하지 않는 Class들은 메모리에서 삭제한다.
* class의 instance를 생성하면 Class Loader를 통해 메모리에 Load하게 된다.
* Java는 Dynamic Code
* Compiletime이 아니라 Runtime시에 참조한다. 즉 Class를 처음 참조할 때, 해당 Class를 Load하고 Link한다.

### Runtime Data Areas

* JVM이 프로그램을 수행하기 위해 OS로부터 별도로 할당 받은 메모리 공간을 의미한다.
* PC Register
  * CPU는 명령어를 수행하는 동안 필요한 정보를 Register라고 하는 CPU내의 기억장치를 사용한다.
  * Stack - Base로 동작한다.
  * 각 쓰레드별로 하나씩 존재하며, 현재 수행중인 JVM 명령어의 주소를 가지게 된다.
  * 만약 Native Method를 수행한다면 PC Register는 undefined 상태가 된다.
  * PC Register에 저장되는 명령어의 주소는 Native Pointer 일 수도 있고, Method ByteCode일 수도 있다.
  * Native Method를 수행할 때에서는 JVM을 거치지 않고 API를 통해 바로 수행하게 된다.
* JVM Stack
  * JVM Stack은 Thread의 수행 정보를 Frame를 통해서 저장하게 된다.
  * Thread가 시작될 때 생성되며, 각 쓰레드별로 생성이 되기 때문에 다른 쓰레드에는 접근할 수 없다.
  * 메소드가 호출되면 메소드와 메소드 정보는 스택에 쌓이게 되며 메소드 호출이 종료될 때 스택 point에서 제거된다.
  * 메소드 정보는 해당 메소드의 매개변수, 지역변수, 임시변수, 주소(메소드 호출한 주소) 등을 저장하고, 메소드 종료시 메모리 공간이 사라진다.
* Native Method Stack
  * Java외의 언어로 작성된 Native Code들을 위한 스택, 즉 JNI(Java Native Interface)를 통해 호출되는 C/C++ 등의 코드를 수행하기 위한 Stack
* Method Area(Class Area, Static Area)

  * 모든 쓰레드가 공유하는 메모리 영역이다.
  * Method Area는 Class, Interface, Method, Field, Static 변수 등의 Byte Code 등을 보관한다.

* Heap

  * 인스턴스 또는 객체를 저장하는 공간이다.
  * 프로그램상에서 런타임시 다이나믹으로 할당하여 사용하는 공간이다.
  * .class를 이용해 인스턴스를 생성하면 Heap에 저장된다.
  * new 연산자로 생성된 인스턴스를 저장한다.
  * 초기 heap size는 64m이고 최대 heap size는 256m이다.
  * GC의 대상이 된다.
  * 성능 이슈를 일으키는 공간이다.
    * Permanent Generation
      * 생성된 인스턴스의 주소값이 저장된 공간이다.
    * New/Young Area
      * eden : 인스턴스들이 최초로 생성되는 공간
      * survivor 0/1 : eden에서 참조되는 인스턴스들이 저장되는 공간
    * Old
      * new Area에서 일정 시간 참조되고 살아남은 객체들이 저장되는 공간
      * eden 영역에서 인스턴스가 가득차게 되면 첫번째 GC가 발생한다.
      * eden 영역에 있는 값들을 Survivor 1영역에 복사하고, 이 영역을 제외한 나머지 영역의 객체를 삭제한다.
* Runtime Constant Pool

  * 각 클래스와 인터페이스의 상수, 메소드와 필드에 대한 모든 레퍼런스를 담고 있는 테이블이다.
  * 메소드나 필드의 실제 메모리상 주소를 찾을땐 해당 풀을 참조한다.

### Execution Engine

* Load된 Class의 바이트코드를 실행하는 Runtime Module이다.
* Class Loader를 통해 JVM내의 Runtime Data Areas에 배치된 ByteCode는 Execution Engine에 의해 실행되며, 실행 엔진은 자바 바이트코드를 명령어 단위로 읽어서 실행한다.
* 최초의 JVM은 인터프리터 방식이기 때문에 속도가 느린 단점이 있었지만, JIT 컴파일러 방식을 통해 이 점을 보완했다.
* JVM은 모든 코드를 JIT 컴파일러 방식으로 실행하지 않고, 인터프리터 방식을 사용하다가 일정한 기준을 넘어서면 JIT 컴파일러 방식으로 실행한다.

### 실행 과정

1. 프로그램이 실행되면 JVM은 OS로부터 이 프로그램이 필요로 하는 메모리를 할당받는다. JVM은 이 메모리를 용도에 따라 여러 영역(Rumtime Data Areas)으로 나누어 관리한다.
2. Java Compiler(Javac)가 Java SourceCode(.java)를 읽어들여 Java ByteCode(.class)로 변환한다.
3. Class Loader를 통해 class 파일들을 JVM으로 Loading한다.
4. Loading된 class 파일들은 Execution Engine을 통해 해석된다.
5. 해석된 ByteCode는 Runtime Data Areas에 배치되며 실질적인 수행이 이루어지게 된다. 이러한 과정속에서 필요에 따라 Thread Synchronization과 같은 GC 작업을 수행한다.

### JIT(Just In Time) Compiler

* Interpreter 방식의 단접을 보완하기 위해 도입된 컴파일러

* 인터프리터 방식으로 실행하다가 일정한 기준을 넘어서면 바이트코드 전체를 컴파일하여 네이티브 코드로 변환한다.

* 이후에는 더이상 인터프리터 방식으로 컴파일하지 않고, 네이티브 코드로 직접 실행한다.

* NativeCode는 Cache에 보관하기 때문에 한번 Compile된 Code는 빠르게 수행된다.

* JIT Compiler가 Compile하는 과정은 ByteCode를 Interpreting하는 방식보다 훨씬 오래 걸리므로 한 번만 실행되는 Code라면 Interpreter방식이 적절하다.

## GC

## Spring

### Spring

* 스스로 발전하는 프레임워크
* 스프링 개발 철학 중 하나는 "항상 프레임워크 기반의 접근 방법을 사용하라"
* 스프링 기능의 대부분은 핵심 기능을 확장해서 발전시킨 결과물이다.
* 단순함과 유연성을 중요 가치로 생각한다.
* 자바 엔터프라이즈 개발을 편하게 해주는 오픈소스 경량급 애플리케이션 프레임워크
* 필요한 부분만 가져다 사용할 수 있도록 모듈화 되어 있다.
* 각 모듈은 독립적으로 분리되어 있고, 재사용이 가능하다.
* IoC/DI/AOP를 지원한다.

### Spring Boot

* Spring Project의 하나
* 초기 수작업의 셋팅을 자동으로 해준다.
* 프로젝트마다 기본적으로 설정하게 되는 부분들을 이미 내부적으로 가지고 있다.
* Servlet Contatiner를 기본 내장하고 있다.(Tomcat, Jetty)
* Pom.xml에서 의존 라이브러리의 버전을 자동으로 관리해준다.
* 자주 사용하는 프로젝트 조합을 미리 만들어 놓고 스프링을 더욱 쉽고 간단하게 사용하기 위해 만들어졌다.

### Spring Bean Life Cycle

1. Bean 인스턴스화 및 DI
   1. XML파일 / Java Config / Annotation에서 bean 정의를 스캔
   2. bean 인스턴스 생성
   3. bean property에 의존성 주입
2. 스프링인지 여부 검사
   1. bean이 BeanNameAware 인터페이스 구현 시 setBeanName() 호출
   2. bean이 BeanClassLoaderAware 인터페이스 구현 시 setBeanClassLoader() 호출
   3. bean이 ApplicationContextAware 인터페이스 구현 시 setApplicationContext() 호출
3. Bean 생성 생명주기 Callback
   1. @PostConstruct Annotation 적용 메소드 호출
   2. bean이 initializingBean 인터페이스 구현시 afrerPropertiesSet() 호출
   3. bean이 init-method 정의하면 지정한 메소드 호출
4. Bean 소멸 생명주기 Callback
   1. @PreDestory Annotation 적용 메소드 호출
   2. bean이 DispoableBean 인터페이스 구현시 destroy() 호출
   3. bean이 destroy-method 정의하면 지정한 메소드 호출

### IoC Container

* 컨테이너는 보통 인스턴스의 생명 주기를 관리하며, 생성된 인스턴스들에게 추가적인 기능을 제공하도록 한다.
* 작성한 코드의 처리과정을 위임받은 독립받은 존재이다.
* 적절한 설정만 되어 있다면 누구의 도움 없이도 프로그래머가 작성한 코드를 스스로 참조한 뒤 알아서 객체의 생성과 소멸을 컨트롤해준다.
* 스프링 컨테이너는 IoC를 이숑해 애플리케이션을 구성하는 빈/컴포넌트들을 관리한다.
* 스프링 컨테이너 = IoC컨테이너 = DI컨테이너
* 스프링 컨테이너는 두 종류가 있다.
  * BeanFactory
    * DI의 기본사항을 제공하는 가장 단순한 컨테이너
    * 팩토리 패턴을 구현한 것
    * Bean을 생성하고 분배하는 책임을 지는 클래스
    * Bean의 정의는 즉시 로딩하지만, 빈 인스턴스 생성은 Lazy Loading한다.
    * 처음으로 getBean()이 호출된 시점에서야 해당 빈을 생성한다.(Lazy Loading)
  * ApplicationContext
    * BeanFactory 인터페이스를 상속 받은 하위 인터페이스이다.
    * 하지만 내부적으로 별도의 BeanFactory를 유지하고 있다.
    * 즉시 인스턴스를 만든다.
    * BeanFactory와 유사한 기능을 제공하지만 좀 더 많은 기능을 제공한다.
    * 국제화가 지원되는 텍스트 메시지를 관리해준다.
    * 이미지 같은 파일 자원을 로드 할 수 있는 포괄적인 방법을 제공한다.
    * Listener로 등록된 Bean에게 이벤트 발생을 알려준다.
    * Context초기화 시점에서 모든 싱글톤 Bean을 미리 로드한 후 애플리케이션을 기동한다.

### IoC(Inversion of Control, 제어의 역전)

* 프로그램의 제어 흐름 구조가 바뀌는 것
* 사용자가 객체를 생성하고 소멸시키는 것이 컨테이너가 대신 하게 된다.(제어의 역전)
* 이 제어권이 스프링 컨테이너로 넘어가는 것이 Spring IoC이다.
* 제어권이 컨테이너로 넘어감으로써 DI, AOP가 가능해졌다.
* 인스턴스의 생성부터 소멸까지의 객체(Bean) 생명주기를 컨테이서가 관리하게 된다.
* 스프링에서 객체가 만들어지는 순서
  1. 객체 생성
  2. 의존성 객체 주입(스스로 만드는 것이 아니라 제어권을 가진 스프링에게 위임하여 스프링이 만드러 놓은 객체를 주입한다.)
  3. 의존성 객체 메소드 호출

### DI(Dependency Injection, 의존성 주입)

* 인스턴스를 자신이 아닌 IoC 컨테이너에서 생성 후 주입한다.
* 내부적으로 new 키워드를 사용하지 않고 setter나 생성자를 이용한다.
* 기능이 변경 될 때 마다 코드를 변경하는 것은 비용이 많이 들게 되므로 가급적 코드의 변화가 적어지도록 프로그램을 작성하기 위해 탄생
* 모듈 간 결합도를 낮춰서 유연한 변경을 가능하도록 한다.
* 불필요한 의존 관계를 없애거나 줄일 수 있다.
* 각 객체를 bean 컨테이너로 관리한다.
* IoC를 구현하는 한 가지 방법이 DI이다.

### AOP(Aspect Oriented Programming, 관점 지향 프로그래밍)

* 애플리케이션 전체에 걸쳐 사용되는 기능들 재사용하도록 지원하는 것이다.
* 가로(횡단) 영역의 공통된 부분을 잘라냈다고 하여 크로스 컷팅(Cross-Cutting)이라고도 불린다.
* 로깅, 트랜잭션, 보안 등
* 로직 주입
* 프록시 패턴과 유사

### Spring MVC

1. 클라이언트의 요청에 대한 최초 진입 지점은 DispatcherServlet이 담당하게 된다. 일종의 front controller이다. 이 servlet이 다음 작업을 처리하게 된다.
2. DispatcherServlet은 Spring Bean Definition에 설정되어 있는 Handler Mapping 정보를 참조하여 해당 요청을 처리하기 위한 Controller를 찾는다.
3.  DispatcherServlet은 선택된 Controller를 호출하여 클라이언트가 요청한 작업을 처리한다.
4. Controller는 Business Layer와 통신하여 원하는 작업을 처리한 다음 요청에 대한 성공 유무에 따라 ModelAndView 인스턴스를 반환한다. ModelAndView 클래스에는 UI Layer에서 사용할 Model 데이터와 UI Layer로 사용할 View에 대한 정보가 포함되어 있다.
5. DispatcherServlet은 ModelAndView의 View의 이름이 논리적인 View 정보이면 ViewResolver를 참조하여 이 논리적인 View 정보를 실질적으로 처리해야 할 View를 생성하게 된다.
6. DispatcherServlert은 ViewResolver를 통하여 전달된 View에게 ModelAndView를 전달하여 마지막으로 클라이언트에게 원하는 UI를 제공할 수 있도록 한다. 마지막으로 클라이언트에게 UI를 제공할 책임은 View 클래스가 담당하게 된다.

### Spring Data JPA

* JPA(Java Persistence API) : 자바 영속성
* 도메인 주도 개발이 가능하다.
* 애플리케이션 코드가 SQL 데이터베이스 관련 코드에 잠식 당하는 것을 방지하고 도메인 기반의 프로그래밍으로 비즈니스 로직을 구현하는데 집중할 수 있다.
* 개발 생산성이 좋으며, 데이터베이스에 독립적인 프로그래밍이 가능하다.
* 타입 안정적인 쿼리 작성, Persistent Context가 제공하는 캐시 기능으로 성능 최적화까지 가능하다.
* 영속성 관리와 ORM을 위한 표준 기술이다.
* ORM 표준 기술로 Hibernate, OpenJPA, EclipseLink, TopLink Essentails과 같은 구현체가 있고 이에 표준 인터페이스가 JPA이다.

### MyBatis

* 개발자가 지정한 SQL, 저장프로시저, 그리고 몇가지 고급 매핑을 지원하는 Persistent 프레임워크다.
* JDBC로 처리한는 상당 부분의 코드와 파라미터 설정 및 결과 매핑을 대신해준다.
* 데이터베이스 결과에 원시 타입과 Map 인터페이스 그리고 POJO를 설정해서 매핑하기 위해 XML과 어노테이션을 사용할 수 있다.
* SqlSessionFactory을 사용한다. 실제 SQL를 호출해주는 역할을 한다.
* Java 소스에서 SQL을 분리해준다.

### 생성자 의존성 주입

* Spring 4.3+부터 생성자가 1개일 경우 @Autowired없이 생성자 의존성 주입이 가능하다.
* 단일 책임의 원칙
  * 생성자의 인자가 많을 경우 코드량도 많아지고, 의존 관계도 많아져 단일 책임의 원칙에 위배된다.
  * 의존관계, 복잡성을 쉽게 알수 있어 리팩토링의 단초를 제공하게 된다.
* 테스트 용이성
  * 특정 DI 컨테이너에 의존하지 않고, POJO여야 한다.
  * DI 컨테이너를 사용하지 않고도 인스턴스화 할 수 있고, 단위 테스트도 가능하며 다른 DI 프레임워크로 전환할 수 있게 된다.
* immutability
  * 필드가 final이 가능해 객체가 변경 불가능 상태가 된다.
* 순환 의존성
* 의존성 명시

### Hystrix

* Netflix OSS의 하나
* 분산 환경(MSA)에서 장애 내성과 지연 내성을 위한 라이브러리이다.
* 최소한의 부하로 운영이 가능하다.
* Circuit Breaker, DashBoard기능이 있다.
* 내부적으로 RxJava를 사용하고 있다.
* Circuit Breaker
  * 서비스간 의존성이 발생하는 접근 포인트를 분리시켜서 장애 전파를 막는다.
  * Fallback를 제공하여 시스템 장애로부터 복구를 유연하게 한다.
  * 쓰레드 풀 방식과 세마포어 방식이 있다.
  * 동기 방식과 비동기 방식으로 구성할 수 있다.
* 발동 조건(기본값)
  * 20번의 메소드 실행중, 10번 이상 실패 시 서킷브레이커 발동
  * 19번의 메소드가 실행됬다면 기본 충족수 미달로 서킷브레이커가 발동하지 않는다.
* 해제 조건(기본값)
  * 5초 이내 단 하나의 메소드 다시 실행 후 성공 시 서킷브레이커 닫힘, 실패 시 열림 유지
* 생명주기
  1. HystrixCommand, HystrixObservableCommand 객체 생성
  2. Command 실행
  3. 캐시 상태 확인
  4. 회로 상태 확인
  5. 사용 가능한 Thread Pool / Semaphore가 있는지 확인
  6. HystrixCommand.run() / HystrixObservableCommand.construc() 실행
  7. Calculate Circuit Health 확인
  8. Fallback 실행
  9. 응답 반환

## Web

### WAS(Web Application Server)

* 인터넷 상에서 HTTP를 통해 사용자 컴퓨터나 장치에 애플리케이션을 수행해 주는 미들웨어
* 동적 서버 컨턴츠를 수행하는 것으로 주로 DB서버와 같이 수행된다.
* 대부분 자바 기반이다.
* 일반적으로 Web Server의 기능을 포함하고 있어 Web Server가 없어도 서비스가 가능하다.
* 여러개의 트랜잭션을 관리한다.
* Servlet 페이지를 HTML 형태로 변환한다.
* JSP의 경우 java class 파일로 컴파일 후 print 형식으로 페이지를 사용자에게 전달한다.
* 사용자의 요청을 스레드 방시식으로 처리한다. 따라서 멀티 쓰레드 기반이다.

### WAS 생명주기

1. Web Server/클라이언트로부터 요청이 들어오면 제일 먼저 컨테이너가 이를 알맞게 처리한다.
2. 컨테이너는 web.xml를 참조하여 해당 서블릿에 대한 스레드를 생성하고 httpServletRequest / httpServletResponse 객체를 생성하여 전달한다.
3. 다음으로 컨테이너는 서블릿을 호출한다.
4. 호출된 서블릿의 작업을 담당하게 된 스레드는 요청에 따라 doPost(), doGet()을 호출한다.
5. 호출된 doPost(), doGet() 메소드는 생성된 동적 페이지를 Response 객체에 실어서 컨테이너에 전달한다.
6. 컨테이너는 전달받은 Response 객체를 HttpResponse 형태로 전환하여 웹 서버에 전달하고 생성되었던 스레드를 종료하고, httpServletRequest / httpServletResponse 객체를 소멸시킨다.

### Tomcat

* Apache 소프트웨어 재단에서 개발한 Servlet Container(Web Container)만 있는 Web Application Server이다.
* Web Server와 연동하여 실행할 수 있는 자바 환경을 제공한다.
* Servlet이나 JSP를 실행하기 위한 Servlet Container을 제공한다.
* 정적 컨텐츠를 로딩하는데 웹 서버보다 수행 속도가 느리다.
* Tomcat 8.5기준
  * MaxThreadPool : 200
  * minSpareThreads : 25
  * MaxQueueSize : Integer.MAX_VALUE

### Servlet

- Java를 사용하여 웹 페이지를 동적으로 생성하는 서버측 프로그램을 말한다.
- 웹 서버의 성능을 향상하기 위해 사용되는 자바 클래스의 일종이다.
- JSP와 비슷한 점이 있지만, JSP가 HTML에 Java코드를 포함하고 있는 반면, Servlet은 Java코드에 HTML을 포함하고 있다.
- 외부 요청마다 Thread로 응답한다.
- Java로 구현되기 때문에 다양한 플랫폼에서 동작한다.

### Web Server

* 클라이언트의 요청을 받아 html이나 오브젝트를 http 프로토콜을 이용해 전송하는 것이다.
* 정적 컨텐츠를 관리한다.(html, css, js, 이미지등)
* Apache, Nginx

### URL 입력시 동작 방식

### OAuth

* 인터넷 사용자들이 비밀번호를 제공하지 않고 다른 웹 사이트의 자신들의 정보에 대해 웹 사이트나 애플리케이션의 접근 권한을 부여할 수 잇는 공통적인 수단으로 사용된다.
* 접근 위임을 위한 개방형 표준이다.
* OAuth를 이용하면 이 인증을 공유하는 애플리케이션끼리는 별도의 인증이 필요없다.
* 여러 애플리케이션을 통합하여 사용하는 것이 가능하다.
* 용어
  * 사용자(Client) : 서비스 공급자와 소비자를 사용하는 계정을 가지고 있는 개인
  * 소비자(App) : Open API를 이용하여 개발된 OAuth를 사용하여 서비스 제공자에게 접근하는 웹 사이트 또는 애플리케이션
  * 서비스 제공자(Facebook) : OAuth를 통해 접근을 지원하는 웹 애플리케이션(Open API를 제공하는 서비스)
  * 소비자 비밀번호 : 서비스 제공자에서 소비자가 자신임을 인증하기 위한 키
  * 요청 토큰(Request Token) : 소비자가 사용자에게 접근 권한을 인증받기 위해 필요한 정보가 담겨있으며 후헤 접근 토큰으로 변환된다.
  * 접근 토큰(Access Token) : 인증 후에 사용자가 서비스 제공자가 아닌 소비자를 통해서 보호된 자원에 접근하기 위한 키를 포함한 값이다.
* 인증 방식
  1. 소비자(App)가 서비스 제공자(Facebook)에게 요청 토큰(Request Token)을 요청한다.
  2. 서비스 제공자(Facebook)가 소비자(App)에게 요청 토큰(Request Token)을 발급해준다.
  3. 소비자(App)가 사용자(Clinet)를 서비스 제공자(Facebook)로 이동시킨다. 여기서 사용자 인증이 수행된다.
  4. 서비스 제공자(Facebook)가 사용자(Clinet)를 소비자(App)로 이동시킨다.
  5. 소비자(App)가 접근 토큰(Access Token)을 요청한다.
  6. 서비스 제공자(Facebook)가 접근 토큰(Access Token)을 발급한다.
  7. 발급된 접근 토큰(Access Token)을 이용하여 소비자(App)에서 사용자 정보(App이 원하는 진짜 정보)에 접근한다.

### CORS(Cross Origin Resource Sharing)

* Cross-Site Http Request를 가능하게 하는 표준 규약이다.
* 다른 도메인으로부터 리소스가 필요할 경우 cross-site http request가 필요하다.
* 이미지, 링크, 스크립트 태그는 작동 한다.
* 하지만 스크립트 태그로 둘러싸여 있는 스크립트에서 생성된 cross-site http request는 same origin policy를 적용 받기 때문에 cross-site http request가 불가능하다.
* 기존에는 XMLHtppRequest는 보안상의 이유로 자신과 동일한 도메인만 HTTP 요청을 보내도록 제한했다. 즉 cross-origin http 요청을 제한했다.
* Simple/Preflight, Credential/Non-Credential의 조합으로 4가지가 존재한다.
* Simple Request
  * 서버에 1번 요청하고, 서버도 1번 회신하는 것으로 처리가 종료된다.
  * GET, HEAD, POST 중의 한 가지 방식을 사용해야 한다.
  * POST 방식일 경우 Content-type이 셋 중 하나여야 한다.
    * application/x-www-form-urlencoded
    * multipart/form-data
    * text/plain
  * 커스텀 헤더를 전송하지 말아야 한다.
* Preflight Request
  * Simple Request 조건에 해당하지 않으면 브라우저는 Preflight Request 방식으로 요청한다.
  * GET, HEAD, POST외의 다른 방식으로도 요청을 보낼 수 있다.
  * Application/xml처럼 다른 Content-type으로 요청을 보낼 수 있다.
  * 커스텀 해더도 사용할 수 있다.
  * 예비 요청과 본 요청으로 나뉘어 전송된다.
  * 예비 요청과 본 요청에 대한 서버의 응답을 프로그래머가 프로그램 내에서 구분하여 처리하는 것은 아니다.
* Request with Credential
  * HTTP Cookie와 HTTP Authentication 정보를 인식할 수 있게 해주는 요청
  * 요청 시 xhr.withCredentials = true를 지정해서 Credential 요청을 보낼 수 있다.
  * 서버는 Response Header에 반드시 Access-Control-Allow-Credentials: true를 포함해야 하고, Access-Control-Allow-Origin 헤더값에는 * 가 오면 안되며, 구체적인 도메인이 와야한다.
* Request without Credential
  * CORS 요청은 기본적으로 Non-Credential 요청이다.
  * xhr.withCredentials = true를 지정하지 않으면 Non-Credential 요청이다.

### CSRF(Cross-Site Request Forgery, XSRF, 사이트 간 요청 위조)

* 웹 사이트 취약점 공격의 하나이다.
* 사용자가 자신의 의지와는 무관하게 공격자가 의도한 행위를 특정 웹 사이트에 요청하게 하는 공격을 말한다.
* 특정 웹 사이트가 사용자의 웹 브라우저를 신용하는 상태를 노린 것이다.
* 사용자가 웹 사이트에 로그인한 상태에서 CSRF 코드가 삽입된 페이지를 열면, 공격 대상이 되는 웹 사이트는 위조된 공격 명령이 믿을 수 있는 사용자로부터 발송된 것으로 판단하게 되어 공격에 노출된다.
* **특정 웹 사이트가 사용자의 웹 브라우저를 신용하는 상태를 노린 점이다.**
* 공격 과정
  1. 이용자는 웹 사이트에 로그인해 정상적인 쿠키를 발급받는다.
  2. 공격자는 악성 링크를 이메일이나 게시판들의 경로를 통해 이용자에게 전달한다.
  3. 이용자가 공격용 페이지를 열면, 브라우저는 이미지 파일을 받아오기 위해 공격용 URL을 연다.
  4. 이용자의 승인이나 인지 없이 출발지와 도착지가 등록도미으로써 공격이 완료된다.

### XSS(Cross-Site Scripting, 사이트 간 스크립팅, 크로스 사이트 스크립팅)

* 웹 애플리케이션에서 많이 나타나는 취약점의 하나이다.
* 웹 사이트 관리자가 아닌이가 웹 페이지에 악성 스크립트를 삽입할 수 있는 취약점이다.
* 주로 여러 사용자가 보게되는 게시판에 악성 스크립트가 담긴 글을 올리는 형태로 이루어진다.
* 웹 애플리케션이 사용자로부터 입력 받은 값을 제대로 검사하지 않고 사용할 경우 나타난다.
* 이 취약점으로 해커가 사용자의 정보(쿠키, 세션)을 탈취하거나 자동으로 비정상적인 기능을 수행하게 할 수 있다.
* 주로 다른 웹 사이트와 정보를 교환하는 식으로 작동하므로 사이트 간 스크립팅이라고 한다.
* **사용자가 특정 웹 사이트를 신용하는 점을 노린 것이다.**

### Cookie(쿠키)

* 클라이언트 로컬에 저장되는 키와 값이 들어있는 작은 데이터 파일이다.
* 이름, 값, 만료날짜(쿠키 저장기간), 경로 정보가 들어있다.
* 일정 시간동안 데이터를 저장할 수 있다.(로그인 상태 유지에 활용)
* 클라이언트의 상태 정보를 로컬에 저장했다가 참조한다.
* 쿠키는 사용자가 따로 요청하지 않아도 브라우저가 요청시에 헤더를 넣어서 자동으로 서버에 전송한다.
* 프로세스
  * 브라우저에서 웹 페이지 접속
  * 클라이언트가 요청한 웹 페이지를 받으면서 쿠키를 클라이언트(로컬 스토리지)에 저장
  * 클라이언트가 재 요청시 요청과 함께 쿠키값도 전송
  * 지속적으로 로그인 정보를 가지고 있는 것 처럼 사용

### Session(세션)

* 일정 시간동안 같은 브라우저로부터 들어오는 일련의 요구를 하나의 상태로 보고 그 상태를 유지하는 기술
* 웹 브라우저를 통해 웹 서버에 접속한 이루호 브라우저를 종료할 때 까지 유지되는 상태
* 클라이언트가 요청을 보내면 해당 서버의 엔진이 클라이언트에게 유일한 ID를 부여하는데 이것이 세션 ID이다.
* 세션을 구분하기 위해 ID가 필요하고 그 ID만 쿠키를 이용해 저장해놓는다.
* 프로세스
  * 클라이언트가 서버에 접속시 세션 ID를 발급한다.
  * 서버에서는 클라이언트로 발급해준 세션 ID를 쿠키를 사용해 저장한다.(JSESSIONID)
  * 클라이언트는 다시 접속할 때 이 쿠키를 이용해서 세션 ID값을 서버에 전달한다.

### Token(토큰)

* Statelss방식이다. 상태유지를 하지 않는다.
* 사용자의 인증 정보를 서버나 세션에 담아두지 않는다.
* 세션이 존재하지 않으니 로그인 되어있는지 안 되어있는지 신경도 쓰지 않으면서 서버를 손쉽게 확장할 수 있다.
* 모바일 애플리케션에 적합하다.
* 인증 정보를 다른 애플리케이션에 전달할 수 있다.(OAuth)
* 서버 기반 인증의 문제점 - 세션, 확정성, CORS의 문제점을 보완할 수 있다.

### HTTP(Hyper Text Transfer Protocol)

* WWW 상에서 정보를 주고 받을 수 있는 프로토콜
* TCP, UDP, IP 프로토콜을 사용한다.
* 80번 포트를 사용한다.
* 연결 상태를 유지하지 않는 프로토콜이다.
  * 서버에 접속하여 클라이언트의 요청에 대한 응답을 전송 후 연결 종료
  * 전산 자원이 적게 들지만, 클라이언트 구분이 힘들다.
  * 요청이 많아 질 경우 또 다른 문제 발생
  * Cookie, Session, Token등을 사용해 단점 해소
* 클라이언트와 서버 사이에 이루어지는 요청(Request) / 응답(Response) 프로토콜이다.

### HTTP Message

* 서버와 클라이언트 간에 데이터가 교환되는 방식이다.
* Request : 클라이언트에 의해 전달되어 서버의 동작을 일으킨다. 요청 메시지
* Response : Request에 대한 서버의 회신이다. 응답 메시지
* ASCII로 인코딩 된 텍스트 정보로 구성되고 여러 줄에 걸쳐 만들어진다.
* 시작줄 : Http Method, Request Target(요청 대상, URL, 도메인), Http 버전 정보, Http Status Code등을 포함한다.
* 헤더(Header)
* 본문(body, payload) : 전송하고, 전송 받은 데이터를 포함한다

### HTTP2

* Http 1 에서는 앞의 요청의 응답을 받아야 다음 요청이 처리될 수 있다.
* Http 1 에서는 Request를 날릴 때 마다 새로 Connection을 생성했다.
* Http 1.1 에서는 지속 커넥션이라는 개념과 Http 파이프라이닝이라는 개념이 포함됬다. 커넥션을 재사용 할 수 있고, 서버에 여러 개의 Request를 날릴 수 있게 됬다. 그러나 Request를 날린 순새대로 Response를 받을 수 있다는 문제점이 있다. 블록킹이 발생하면 뒤의 요청이 블록킹된다.
* Http 1.1 에서는 1 Request당 1개의 Resource를 받아올 수 있다.(한 커넥션에서 한 번에 하나의 Response를 받을 수 있다.)
* Http 1.1에서는 Plain Text로 구성되어 있다.
* Http 2에서는 Multiplexing 개념이 도입되었다.
* 하나의 Request당 동시에 여러 Resources를 받아올 수 있다.
* 데이터를 전송할 때 Binary로 인코딩하여 전송한다.
* Frame, Stream이라는 개념이 추가됬다.
* Stream : 클라이언트와 서버 사이에 맺어진 연결을 통해 양방향으로 데이터를 주고 받는 한개 이상의 메시지를 의미

### HTTPS

* HTTP의 보안이 강화된 버전이다.
* S는 Over Secure Socket Layer의 약자이다.
* 소켓 통신(TCP)에서 일반 텍스트 대신에 SSL이나 TLS 프로토콜을 통해 세션 데이터를 암호화한다.
* 데이터의 적절한 보호를 보장한다.
* 클라이언트 요청시 SSL에 필요한 통신이 추가되기 때문에 통신이 느려진다.
* 암호화 복화화 계산을 하기 때문에 서버나 클라이언트의 자원을 추가적으로 소비한다.
* 증명서를 구입해야 한다.

### URI(Unifrom Resource Identifier, 통합 자원 식별자)

* 인터넷에 있는 자원을 나타내는 유일한 주소이다.
* 자원을 식별할 수 있는 문자열이다.
* 하위 개념으로 URL, URN이 있다.
* 127.0.0.1:8080/users?name=배다슬 은 URI, name=배다슬 이라는 식별자가 있다. 

### URL(Uniform Resource Locator, 유일 자원 지시기)

* 네트워크 상에서 자원이 어디있는지를 알려주기 위한 규약이다.
* 127.0.0.1:8080/users/image.jpg

### URN(Uniform Resource Name, 통합 자원 이름)

* 특정 자원에 대해 그 자원의 위치에 영향을 받지 않는 유일무이한 이름 역할을 한다.
* 자원을 여기저기로 옮기더라도 문제없이 동작한다.
* DNS

### REST(REpresentational State Transfer)

### AJAX(Asychronous JavaScript And XML, 비동기식 자바스크립트와 xml)

* 자바스크립트를 통해서 서버에 비동기 식으로 요청하는 것이다.
* 동작 과정
  1. XMLHTTP Request Oject를 만든다.
  2. Callback 함수를 만든다.
  3. Open a Request
  4. Send the Request

### Authorization(어쏠라이제이션, 인가)

### Authentation(어썬티케이션, 인증)

## 자료구조

### Stack

### Queue

### List

### ArrayList

### LinkedList

### Heap

### HashTable

### HashMap

### Tree

### Binary Tree

### B Tree

### B+ Tree

## 알고리즘

### 정렬 알고리즘

### 바이너리 서치

### 캐시 알고리즘

### DFS & BFS

### 암호화 알고리즘

## Network

### OSI 7계층

### L4, L7

### TCP

### UDP

### 로드밸런싱

### 클러스터링

### 게이트웨이

## OS

### 데드락

### 뮤텍스

### 세마포어

### 페이징

### 스케쥴링

### 교착상태

### 경쟁상태

### fork

### 프로세스

### 쓰레드

## Others

### Git

### IO

### NIO

### 동기

### 비동기

### Blocking

### NonBlocking

### Node

### V8

### ECMA 6

### Docker

### CI

### 함수형 프로그래밍

### 반응형 프로그래밍
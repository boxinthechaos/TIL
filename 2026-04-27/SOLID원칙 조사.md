## SRP (단일 책임 원칙)
1) **개념 설명**
   - 하나의 클래스는 하나의 책임(기능)만 가져야 한다는 원칙이다 즉 클래스는 변경될 이유가 하나만 있어야 한다
2) **필요한 이유**
    - 하나의 클래스에 여러 기능이 포함되면, 한 기능을 수정할 때 다른 기능에도 영향을 줄 수 있다
    - 유지보수가 어렵고, 코드복잡도가 증가한다
    - 역할을 분리하면 코드의 가독성 및 재사용성이 높아진다
3) **예시**
    - 잘못된 예: 잘못된 예: 하나의 클래스(Report)가 데이터를 생성하는 책임과 화면에 출력하는 책임 두 가지를 모두 가짐.
        ``` 
        class Report:
            def generate_data(self):
                print("보고서 데이터 생성")

            def print_to_screen(self):
                print("화면에 보고서 출력") ```

    - 개선된 예: 데이터 관리는 Report가, 화면 출력은 ReportPrinter가 각각 하나의 책임만 맡도록 분리.
        ```
        class Report:
            def generate_data(self):
                print("보고서 데이터 생성")

        class ReportPrinter:
            def print_to_screen(self, report):
                print("화면에 보고서 출력")
        ```


## OCP (개방/폐쇄 원칙)
1) **개념 설명**
    -  클래스는 확장에는 열려 있고, 수정에는 닫혀 있어야 한다
2) **필요한 이유**
    - 새로운 기능을 추가할 때 기존에 잘 작동하던 코드를 망가뜨리지 않기 위해서 필요하다
    - 요구사항이 변경되거나 새로운 기능이 필요할 때 시스템 전체를 뜯어고칠 필요가 없습니다
    - 모듈 간의 결합도를 낮춰주어, 코드가 스파게티처럼 얽히는 것을 방지하고 각 모듈이 독립적으로 자신의 역할만 수행할 수 있도록 만들어 줍니다
3) **예시**
    - 잘못된 점: 결제 수단(카카오페이 등)이 추가될 때마다 기존 Payment 클래스의 코드를 직접 수정해야 함.
        ``` 
        class Payment:
            def pay(self, type):
                if type == "card":
                    print("신용카드 결제")
        elif type == "cash":
            print("현금 결제")
        ```
    - 개선점 : 기존 코드를 수정하지 않고, 새로운 클래스(KakaoPayment 등)를 추가하는 것만으로 기능 확장 가능.
    ``` 
    class PaymentType:
        def pay(self): pass

    class CardPayment(PaymentType):
        def pay(self): print("신용카드 결제")

    class CashPayment(PaymentType):
        def pay(self): print("현금 결제")
    ```

## LSP (리스코프 치환 원칙)
1) **개념 설명**
   - 자식 클래스는 언제나 자신의 부모 클래스를 대체할 수 있어야 한다는 원칙이다. 즉, 부모 클래스가 들어갈 자리에 자식 클래스를 넣어도 프로그램은 논리적으로 문제없이 작동해야 한다.
2) **필요한 이유**
    - 상속을 잘못 사용하면 부모 클래스의 의도를 자식 클래스가 망가뜨려, 다형성(Polymorphism)을 활용한 코드가 제대로 동작하지 않게 된다.

    - 이 원칙을 지켜야 상속 계층이 올바르게 설계되며 예측 가능한 코드를 작성할 수 있다.
3) **예시**
    - 잘못 : 펭귄은 새를 상속받았지만 날 수 없어 에러를 발생시킴. (부모 클래스를 온전히 대체 불가)
    ``` 
    class Bird:
        def fly(self):
        print("파닥파닥")

    class Penguin(Bird):
        def fly(self):
            raise Exception("펭귄은 날 수 없음!") 
    ```
    - 개선 : '새'와 '나는 새'를 분리하여 상속 구조를 올바르게 재설계.
    ``` 
    class Bird:
        pass

    class FlyingBird(Bird):
        def fly(self): print("파닥파닥")

    class Penguin(Bird):
        def swim(self): print("헤엄치기")
    ``` 
    
## ISP (인터페이스 분리 원칙, Interface Segregation Principle)

1) 개념 설명

   - 자신이 사용하지 않는 메서드(기능)에 의존하도록 강제되어서는 안 된다는 원칙이다. 즉, 범용적인 큰 인터페이스 하나보다, 특정 클라이언트를 위한 여러 개의 구체적인 인터페이스로 쪼개는 것이 좋다.

2) 필요한 이유

    - 덩치가 큰 인터페이스를 구현하면, 해당 클래스에서 필요 없는 기능까지 억지로 구현해야 하는 낭비가 발생한다.

    - 인터페이스를 작게 분리하면 한쪽 기능이 변경되더라도 다른 쪽에 미치는 영향을 최소화할 수 있다.

3) 예시 (일상 사례 결합)
    - 잘못 : 복합기 인터페이스에 팩스 기능이 묶여 있어, 팩스 기능이 없는 기본 프린터가 억지로 에러 처리를 해야 함.
    ``` 
    class SmartDevice:
        def print(self): pass
        def fax(self): pass

    class BasicPrinter(SmartDevice):
        def print(self): print("출력")
        def fax(self): 
            raise Exception("팩스 기능 없음")
    ```
    - 개선 :프린터와 팩스 인터페이스를 작게 분리하여, 필요한 기능(Printer)만 구현하도록 함.
    ``` 
    class Printer:
        def print(self): pass

    class Fax:
        def fax(self): pass

    class BasicPrinter(Printer):
        ef print(self): print("출력")
    ```

## DIP (의존 역전 원칙, Dependency Inversion Principle)

1) 개념 설명

    - 고수준 모듈(핵심 비즈니스 로직)은 저수준 모듈(데이터베이스 연결 등 구체적인 기술)에 의존해서는 안 되며, 둘 다 '추상화(인터페이스)'에 의존해야 한다는 원칙이다.

2) 필요한 이유

    - 핵심 로직이 구체적인 데이터베이스나 외부 라이브러리에 직접 연결되어 있으면, 해당 기술이 변경될 때 핵심 로직까지 전부 뜯어고쳐야 한다.

    - 추상화에 의존하면 모듈 간의 결합도가 낮아져 기술 스택의 변경이나 테스트가 매우 쉬워진다.

3) 예시 (코드 사례)
    - 잘못 : App이 MySQLDatabase라는 구체적인 기술에 직접 의존하여 DB 변경이 어려움.
    ``` 
    class MySQLDatabase:
        def connect(self): print("MySQL 연결")

    class App:
        def __init__(self):
            self.db = MySQLDatabase()
    ```
    - 개선 : App이 Database라는 추상화된 인터페이스에 의존하게 하여, 어떤 DB로든 쉽게 교체 가능.
    ```
    class Database:
        def connect(self): pass

    class MySQLDatabase(Database):
        def connect(self): print("MySQL 연결")

    class App:
        def __init__(self, db: Database):
            self.db = db
    ```


## 느낀점
이번 수행평가를 통해 SOLID 원칙을 조사하면서 어떤 방식으로 SOLID를 사용해야하고 SOLID 원칙을 사용해야 하는 이유에 대해 더욱 자세히 알 수 있게 되었고 SOLID 원칙을 단순히 외워야 하는 원칙이 아닌 미래의 동료와 협업을 하기 위해서는 이것을 잘 다뤄야 한다 생각이 들었다

# Java

## main(메인)
main을 치면 
```
public class void main(String[] args){}
```
위에 것 처럼 만들어 준다

## sout(프린트)
sout을 치면
```
 System.out.println();
```
위에 처럼 만들어 준다 근데 이것은 Intellij,Eclipse의 기능이다

## 데이터 타입
short, int, long, float, double, char, boolean, byte등이 있다

1. 논리형(boolean):조건식과 논리 연산 에 쓰이며 참과 거짓을 나타낼 때 사용한다


2.정수형
| 자료형 | 크기(byte) | 최솟값 | 최값 |
|:-----------------|:------:|:--------:|:--------:|
| byte | 1byte | -128 | 127 |
| short | 2byte | -32,768 |	32,767 |
|  int | 4byte | -2,147,483,648 | 2,147,483,647 |
| long |  8byte | -9,223,372,036,854,775,808 | 9,223,372,036,854,775,807 |


3.실수형
| 자료형 | 크기(byte) | 최솟값 | 최값 | 정밀도 |
|:-----------------|:------:|--------:|:--------:|:--------:|
| float | 4byte | 1.4E-45 | 3.4E38 | 7자리 |
| double | 8byte | 	4.9E-324 |	1.8E308 | 15자리 |


4. 문자형
- 변수에 문자열이 아닌 하나의 문자만 저장할 수 있음.
(문자열 저장을 위해서는 객체형 String을 써야한다.)

- 자바에서는 Unicode를 지원하기 때문에 문자 하나가 16비트로 표현됨.(규격은 UTF-16을 사용함.)

## 변수
변수 명 앞에 데이터 타입을 입력한 후 이름을 적고 사용하면 사용 가능하다
```
public class _03_Variables {
    public static void main(String[] args) {
        String name = "나도코딩";
        int hour = 15;


        System.out.println(name + "님 배송이 시작 됩니다. " + hour + "시에 방문 예정입니다.");
        System.out.println(name + "님 배송이 완료되었습니다.");

        double score = 90.5;
        char grade = 'A';
        name = "키라키라프리큐어아라모드쿠루쿠루차지캔디로드";
        System.out.println(name + "님의 점수는 " + score + "점으로 학점은 " + grade + "입니다");

        boolean pass = false;
        System.out.println("이번 시험에 합격 했을까요? " + pass);

        double d = 3.14123456789;
        float f = 3.14123456789f;
        System.out.println(d);
        System.out.println(f);

        long l = 1000000000000L;
        l = 1_000_000_000_000l;
        System.out.println(l);
    }
}
```

## 주석
주석은 IDE에서 사용할 수 있는 코드로 소스코드를 더 쉽게 이해 시키기 위해서 사용된다 실제 코드에 영향을 주진 않는다
```
System.out.println("(10분 전) 잠시후 결혼식이 시작 예정이오니 자리에 착석 부탁드립니다.");
//System.out.println("(5분 전) 잠시후 결혼식이 시작 예정이오니 자리에 착석 부탁드립니다.");
System.out.println("지금부터 결혼식을 시작하겠습니다.");
한줄 주석은 ctrl + /
여러줄 주석은 ctrl + shift + /
```

## 변수이름 짓는 법
변수이름 짓는 법
1. 저장할 값에 어울리는 이름
2. 밑줄(_) 문자(abc), 숫자(1234) 사용 가능 (공백 사용 불가)
3. 밑줄 또는 문자로 시작 가능함
4. 한 단어 또는 두개 이상 단어로 한다
5. 변수는 소문자로 시작, 각 단어의 시작 글자는 대문자 (첫 단어 제)
6. 예약어는 사용 불가( public, static, void, int ,double, float)

## 상수
상수는 변하지 않는 것을 말하며 사용방법은 변수 맨 앞에 final을 쓰면 된다
```
public class _06_Constants {
    public static void main(String[] args) {
        final String KR_COUNTRY_CODE = "+82"; // 국가번호 (빨리)
        //KR_COUNTRY_CODE = "+8282";
        System.out.println(KR_COUNTRY_CODE);

        final double PI = 3.141592; //원주율
        final String DATE_OF_BIRTH = "2008-03-03"; // 생년 월일
    }
}
```

## 형변환
말 그대로 형을 변환시키는 것이다 사용 방법은 형 변화을 할 것 앞에 (float)등 괄호 열고 데이터 타입을 입력하면 된다
```
public class _07_TypeCasting {
    public static void main(String[] args) {
        //형변환
        //정수형 -> 실수형
        // 실수형 -> 정수형

        // int to float, double
        int score = 93;
        System.out.println(score);//93
        System.out.println((float)score);//93.0
        System.out.println((double)score);//93.0

        // float, double to int
        float score_f = 93.3f;
        double score_d = 98.8;
        System.out.println((int)score_f);//93
        System.out.println((int)score_d);//98

        // 정수 + 실수 연산
        score = 93+(int)98.8; // 93+98
        System.out.println(score);

        score_d = (double) 93+98.8; // 93.0 + 98.8
        System.out.println(score_d); // 191.8

        // 변수에 형 변환된 데이터 집어 넣기
        double convertedScoreDouble = score; // 191 - > 191.0
        // int -> long -> float -> double (자동 형변환)

        int convertedScoreInt = (int)score_d; // 191.8 -> 191
        //double -> float -> llong -> int (수동 형변환)

        // 숫자데이터를 문자 열로
        String s1 = String.valueOf(93);
        s1 = Integer.toString(93);
        System.out.println(s1); // 93

        String s2 = String.valueOf(98.8);
        s2 = Double.toString(98.8);
        System.out.println(s2); //98.8

        // 문자열을 숫자로
        int i = Integer.parseInt("93");
        System.out.println(i);//93

        double d = Double.parseDouble("98.8");
        System.out.println(d);//98.8

        /*int error = Integer.parseInt("자바");
        System.out.println(error);*/
    }
}
```

## 산술 연산자
수학 처럼 산술을 해주는 연산자다
```
public class _01_Operator1 {
    public static void main(String[] args) {
        // 산술 연산자

        // 일반 연산
        System.out.println(4+2); //6
        System.out.println(4-2); //2
        System.out.println(4*2); //8
        System.out.println(4/2); //2
        System.out.println(5/2); //2
        System.out.println(2/4); //0
        System.out.println(4%2); //0
        System.out.println(5%2); //1

        // 우선 순위 연산
        System.out.println(2+2*2);//6
        System.out.println((2+2)*2);//8
        System.out.println(2+(2*2));//6

        //변수를 이용한 연산
        int a = 20;
        int b = 10;
        int c;

        c = a+b;
        System.out.println(c);//30

        c = a-b;
        System.out.println(c);//10

        c = a*b;
        System.out.println(c);//200

        c = a/b;
        System.out.println(c);//2

        c = a%b;
        System.out.println(c);//0

        //즘감 연산자 ++,--
        int val = 10;
        System.out.println(val);//10
        System.out.println(++val);//11
        System.out.println(val);//11

        val = 10;
        System.out.println(val);//10
        System.out.println(val++);//10
        System.out.println(val);//11

        val = 10;
        System.out.println(val);//10
        System.out.println(--val);//9
        System.out.println(val);//9

        val = 10;
        System.out.println(val);//10
        System.out.println(val--);//10
        System.out.println(val);//9

        //은행 대기번호 표
        int waiting = 0;
        System.out.println("대기 인원 : "+ waiting++); // 대기 이원 : 0
        System.out.println("대기 인원 : "+ waiting++); // 대기 이원 : 1
        System.out.println("대기 인원 : "+ waiting++); // 대기 이원 : 2
        System.out.println("총 대기 인원 : "+ waiting); // 총 대기 인원 : 3

    }
}
```

## 대입연산자
대입을 하기 위해서 사용되는 연산자다
```
public class _02_Operator2 {
    public static void main(String[] args) {
        //대입 연산자
        int num = 10;
        num = 10 + 2;
        System.out.println(num); // 12

        num = num - 2;
        System.out.println(num); // 10

        num = num * 2;
        System.out.println(num); // 20

        num =num / 2;
        System.out.println(num); // 10

        num = num % 2;
        System.out.println(num); // 0

        // 복합 대입 연산자
        num = 10;

        //num = num + 2;
        num += 2;
        System.out.println(num); // 12

        //num = num - 2;
        num -= 2;
        System.out.println(num); // 10

        //num = num * 2;
        num *= 2;
        System.out.println(num); // 20

        //num = num / 2;
        num /= 2;
        System.out.println(num); // 10

        //num = num % 2;
        num %= 2;
        System.out.println(num); // 0
    }
}
```

## 비교 연산자
비교하기 위해서 사용되는 연산자다
```
public class _03_Operator3 {
    public static void main(String[] args) {
        // 비교 연산자
        System.out.println(5 > 3);  // 5는 3보다 크다 (참이면 true, 거짓이면 false)
        System.out.println(5 >= 3); // 5는 3보다 크거나 같다 (true)
        System.out.println(5 >= 5); // 5는 5보다 크거나 같다 (true)
        System.out.println(5 >= 7); // 5는 7보다 크거나 같다 (false)

        System.out.println(5 < 3);  //5는 3보다 작다 (false)
        System.out.println(5 <= 3); //5는 3보다 작거나 같다 (false)
        System.out.println(5 <= 5); //5는 5보다 작거나 같다 (true)
        System.out.println(5 <= 7); //5는 7보다 작거나 같다 (true)

        System.out.println(5 == 5); //5는 5와 같다 (true)
        System.out.println(5 == 3); //5는 3과 같다 (false)

        System.out.println(5 != 5); //5는 5와 같지 않다 (false)
        System.out.println(5 != 3); //5는 3와 같지 않다 (true)
    }
}
```

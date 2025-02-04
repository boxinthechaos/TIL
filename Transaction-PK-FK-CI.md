# Transaction-PK-FK-CI

# Transaction이란
Transaction(트랜잭션)은 쪼갤 수 없는 여 작업들을 논리적으로 묶은 최소단위로 묶은 것이다

# Transaction의 특징
1. 원자성(Atomicity)
  > 트랜잭션의 연산은 DB에 모두 반영 되던지 또는 모두 반영 되지 않아야 한다 만약 하나라도 실패한다면 앞서 성공한 것들을 원상복구 시켜나야 한다

2. 일관성(Consistency)
  > 트랜잭션의 작업 처리 결과는 항상 일관성 있어야 한다

3. 독립성(Isolation)
  > 어떤 트랜잭션이라도 다른 트랜잭션 작업에 끼어들 수 없다

4. 지속성(Durability)
  > 트랜잭션 작업이 완료된다면 결과는 영구적이어야 한다

# 트랜잭션이 원자성을 보장하는 방법
트랜잭션은 로직을 수행하고 난 뒤 모든 로직이 성공적으로 수행 되었다면 모든 결과를 DB에 commit 한다 만약 하나라도 실패 했다면 모든 작업을 rollback한다

# Spring의 Transaction 동작 방식에 대한 기본 개념
Spring에서는 @Transactional 어노테이션을 통해서 쉽고 깔끔하게 트랜잭션 처리를 해줄 수 있다. 그런데 `@Transactional`이 어떻게 동작하여 트랜잭션을 보장하는지 궁금해졌다

## Proxy
스프링은 기본적으로 트랜잭션을 Proxy를 통해서 처리한다
![image](https://github.com/user-attachments/assets/a851b4f8-29c7-43ae-a414-5d9372300aab)
실제로 PostService의 addPost 메서드를 요청하면, 동작 전의 코드로는 위와 같이 보이지만 실제 런타임에서는 아래와 같이 작동한다
![image](https://github.com/user-attachments/assets/a5142da4-9a10-4605-b70b-ea6620587d81)
스프링이 어플리케이션이 동작할 때, Transaction 전후 처리를 하는 Proxy를 동적으로 생성해준다

# 기본 키 (PK, Primary Key)
테이블 내에서 유일하게 존재하는 값의 조합을 설정해서, 중복된 데이터가 테이블에 삽입되는 것을 방지해주고 해당 필드가 NOT NULL 과 UNIQUE 제약 조건의 특징을 모두 가지게 된다

# 외래 키 (FK, Foreign Key)
다른 기본 키를 참조하는 속성 or 속성들의 집합이다
> - 참조 관계의 기본 키와 같은 속성을 가진다
> - 데이터가 새롭게 추가될 때, 외래 키에 해당하는 값이 외래 키가 참조하는 테이블에 존재하는지를 확인한다
> - 하나의 테이블을 다른 테이블에 의존하게 만든 (두 테이블을 연결하는 다리 역할)
>   - 외래 키가 포함된 테이블 = 자식 테이블
>   - 외래 키 값을 제공하는 테이블 = 부모 테이블

데이터의 무결성을 보장
> - 하나의 테이블에서 중복된 데이터가 삽입되는 것을 방지한다
> - 외래 키를 가지는 테이블이 참조하는 기준 테이블의 열은 반드시 PK, UNIQUE 제약조건이 설정되어 있어야 한다

DB 는 무조건 다(N)쪽이 외래 키를 갖는다
> - 외래 키가 있는 쪽이 연관관계의 주인이다
>     - 연관관계의 주인 O : 두 객체 사이에서 '조회, 저장, 수정, 삭제' 가능하다
>     - 연관관계의 주인 X : '조회'만 가능

# PK & FK
## 1) 1:1 연관 관계 (일대일)
![image](https://github.com/user-attachments/assets/29345d35-04bc-4ed5-abc0-1c32ba54228f)
각 전화번호가 단 한 명의 유저와 연결되어 있고, 그 반대도 동일하다면, Users 테이블과 Phonebook 테이블은 1:1 관계

## 2) 1:N 연관 관계 (일대다)
![image](https://github.com/user-attachments/assets/c49f423e-f6b7-4887-a99b-e97f0d66b188)
한 쌍의 부모(1) 中 여러 명의 자식(N)들이 어떤 부모에 속해 있는지 표현해야 한다.
따라서, 부모 테이블의 PK 를 자식 테이블에 FK 로 집어 넣어 관계를 표현한다.
(즉, 부모 테이블(1)에서는 내 자식들이 누구인지 정보를 넣을 필요가 없고,
자식 테이블(N)에서만 각각의 자식들이 자신의 부모 정보(FK)를 넣음으로써 관계를 표현)

## 3) N:M 연관 관계 (다대다)
![image](https://github.com/user-attachments/assets/794d6526-73bb-499f-b67c-12c08598d1b8)
N:M 관계는 관계를 가진 양쪽 엔티티 모두에서 1:N 관계를 가지는 것을 말한다. (서로가 서로를 1:N 관계로 보고 있는 것)
  - 학원:학생 = 1:N
  - 학생:학원 = 1:M
  - 따라서, 학원:학생 = N:M
![image](https://github.com/user-attachments/assets/40d25d62-08a6-4c00-b50c-51eac90b2ba3)
N:M 관계는 서로가 서로를 1:N 관계, 1:M 관계로 갖고 있기 때문에, 서로의 PK 를 자신의 외래키 컬럼으로 갖고 있으면 된다.
일반적으로 N:M 관계는 두 테이블의 대표키를 컬럼으로 갖는 또 다른 테이블(중간 테이블)을 생성해서 관리한다.


# PK키와 FK키를 스프링 부트에 연결하기

## 연관 관 정의 규칙
연관 관계를 매핑할 때 중요한 것은 크게 3가지 있다
- 방향 : 양방향, 단방향
- 연관 관계의 주인 : 양방향일때 연관 관계에서 관리를 주체한다
- 다중성 : 다대일(N:1), 일대다(1:N), 일대일(1:1), 다대다(N:N)

# 1.JPA로 테이블 만들기
 Java에서의 엔티티 설계
 ![image](https://github.com/user-attachments/assets/376f68ad-0418-4429-8aac-9bc323a0e1e8)
 
 ---
 ✔️ Member.java
 ```
@Entity
@Getter @Setter
public class Member {   

	@Id @GeneratedValue
	@Column(name = "MEMBER_ID")
	private Long id;
    
	private String name;
	private String city;
	private String street;
	private String zipcode;	
    
}
```
- @Id - 현재 엔티의 기본 키임을 나타낸다
- @Entity - JPA에서 관리한다
- @GeneratedValue - KEY값을 DBSM에 시퀀스처럼 자동증가 퀄럼으로 만들어 준다
- @Column(name = "MEMBER_ID") - 컬럼명을 새로 만들어 준다.
---

✔️ Order.java
```
@Entity							
@Table(name = "ORDERS")	
@Getter @Setter
public class Order {
	
	@Id @GeneratedValue
	@Column(name = "ORDER_ID")
	private Long id;
	
	@Column(name = "MEMBER_ID")		// Member.java에 MEMBER_ID와 FK관계
	private Long memberId;
	
	private LocalDateTime orderDate;
	private String status;
	
}
```
---

✔️ OrderItem.java
```
@Entity
@Getter @Setter
public class OrderItem {	
	
	@Id @GeneratedValue
	@Column(name = "ORDER_ITEM_ID")
	private Long id;
	
	@Column(name = "ORDER_ID")		// Order.java에 ORDER_ID와 FK관계
	private Long orderId;	
	
	@Column(name = "ITEM_ID")		// Item.java에 ITEM_ID와 FK관계
	private Long itemId;
	
	private int orderPrice;
	private int count;
	
}
```
---

✔️ Item.java
```@Entity
@Getter @Setter
public class Item {
	
	@Id @GeneratedValue
	@Column(name = "ITEM_ID")
	private Long id;
    
	private String name;
	private int price;
	private int stockQuantity;
}
```
---

✔️ JpaMain.java <- JPA 실행을 위한 main클래스
```
public class JpaMain {

	public static void main(String[] args) {

		EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
		EntityManager em = emf.createEntityManager();
		EntityTransaction tx = em.getTransaction();
		tx.begin();
		
		try {
        	tx.commit();
		} catch (Exception e) {
			tx.rollback();
		}finally {
			em.close();
			emf.close();
		}
	}
```
---

✔️ 클래스에서 화면 우클릭 > Run As > Java Application으로 실행하기
![image](https://github.com/user-attachments/assets/19e97ce6-d078-4027-8e8f-b187b96146fe)

---
✔️ DB - Table 생성
![image](https://github.com/user-attachments/assets/7f0f40d4-75fa-42bc-8e4e-a929318799d7)

---

# 2. 양방향 매핑 하기

## 2-1
⭐️ex) Member / Team
- 회원은 하나의 팀에만 소속된다
- 회원과 팀은 다대일 관계이다
> ![image](https://github.com/user-attachments/assets/d83f45eb-7bd2-4083-b1bb-66ecda97e524)
---

✔️ Member.java
```
@Entity
@Getter @Setter
public class Member {

	@Id @GeneratedValue
	@Column(name = "MEMBER_ID")
	private Long id;
	
	@Column(name = "USERNAME")
	private String name;
    
// 	FK 키 설정 전
//	@Column(name = "TEAM_ID")
//	private Long teamid;	
	

//  FK 키 설정
	@ManyToOne
	@JoinColumn(name = "TEAM_ID")  //  <- Team.java에 TEAM_ID와 연결된다
	private Team team;
}
```
---

✔️ Team.java
```
@Entity
@Getter @Setter
public class Team {

	@Id @GeneratedValue
	@Column(name = "TEAM_ID")	
	private Long id;
	private String name;

}
```
---

✔️ JpaMain.java
```
public class JpaMain {

	public static void main(String[] args) {

		EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
		EntityManager em = emf.createEntityManager();
		EntityTransaction tx = em.getTransaction();
		tx.begin();
        
		try {
			Team team = new Team();
			team.setName("TeamA");
			
			// -> persist하면 영속상태가 된다.
			// 영속상태가 될 때, PK의 값이 세팅이 된 후에 영속상태가 된다.
			// Team을 먼저 생성해서 PK를 생성해야 FK를 생성할 수 있다.
			em.persist(team); 
			
			Member member = new Member();
			member.setName("member1");
			// FK인 Member 에서 PK 객체인 Team을 통째로 가져온다. 
			member.setTeam(team);
	        em.persist(member);
	        
	        
	        //강제 db 쿼리를 보고 싶을 때
	        em.flush();
	        em.clear();
	        
	        //select
	        // find시에 1차캐시에서 가지고 와서 select문이 없다.
	        Member findmember = em.find(Member.class, member.getId());
	        // Member에서 Team 객체를 통째로 가져온다.
	        Team findTeam = findmember.getTeam();
	        
	                
			tx.commit(); 	// 영속성 컨텍스트에 들어가 테이블 생성
			} catch (Exception e) {
				tx.rollback();
			}finally {
				em.close();
				emf.close();
			}
		}
```
---

✔️ DB - Table 생성
![image](https://github.com/user-attachments/assets/c6adce5d-9252-45d1-b039-db92f420a859)

---
> - 테이블 연관관계
>   관계 1개
>   Member 테이블 입장에서 Team 테이블 조인 가능
>   Team 테이블 입장에서 Member 테이블 조인 가능

> - 객체 연관관계
>   관계 2개
>   Member 객체에서 Team 객체로 연관관계 1개(단방향)
>   Team 객체에서 Member 객체로 연관관계 1개(단방향)
>   사실은 단방향 연관관계가 2개 있는 것이다

> - 관리의 딜레마
>   둘 중 하나로 외래키를 관리해야 한다.
>   Member에서 Team으로 가는 team 참조 값과, Team에서 Member로 가는 members 참조 값이 있다.
>   Member에서 Team값이 수정 되었을 때 Member table의 TEAM_ID가 수정되야 하는지, Team에 있는 members를 수정했을 때 Member table의 TEAM_ID가 수정되야 하는지?
>   DB입장에서는 Member table에 있는 TEAM_ID만 update되면 된다. -> 룰(주인)이 생긴다.

> - 연관관계의 주인(Owner) - 양방향 매핑 규칙
>   객체의 두 관계중 하나를 연관관계의 주인으로 지정
>   연관관계의 주인만이 외래 키를 관리(등록, 수정)
>   주인이 아닌쪽은 읽기만 가능
>   주인은 mappedBy 속성을 사용X
>   -> `mappedBy` : 내가 누군가에 의해서 mapping되었다 라는 뜻
>   주인이 아니면 mappedBy 속성으로 주인 지정
>   ![image](https://github.com/user-attachments/assets/c7a36cee-1145-4989-894b-cc11c1a0d9cc)
>
>   ![image](https://github.com/user-attachments/assets/a128f09b-f95b-444b-8013-8fa7452a97ee)

![image](https://github.com/user-attachments/assets/f94426f9-3da4-4881-a9b7-3b993c2ad1c9)

- 현재 객체는 Member가 Team을 가졌으나, Team은 Member를 가지지 못한다
- 객체 참조와 외래키의 가장 큰 차이점
- 테이블은 FK만 있으면 양쪽에 연관관계를 알 수 있다.
- Member -> Team : N -> 1 = @ManyToOne
- Team -> Member : 1 -> N = @OneToMany

---
✔️ Team.java
```
@Entity
@Getter @Setter
public class Team {

	@Id @GeneratedValue
	@Column(name = "TEAM_ID")
	private Long id;
	private String name;
	
	/* team에 의해서 관리가 된다.
	mappedBy가 적힌 곳은 읽기만 가능하다.
	값을 넣어봐야 아무일도 벌어지지 않는다.
	대신 조회는 가능
	*/
	@OneToMany(mappedBy = "team")	//**
	private List<Member> member = new ArrayList<Member>();
	
	public void addMember(Member member) {
		member.setTeam(this);
		this.member.add(member);
	}

}
```
![image](https://github.com/user-attachments/assets/8088d788-fe46-41f3-a5d6-9ba6be099329)

---
✔️ JpaMain.java
```
public class JpaMain {

	public static void main(String[] args) {

		EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
		EntityManager em = emf.createEntityManager();
		EntityTransaction tx = em.getTransaction();
		tx.begin();
		
		try {
  			Team team = new Team();
			team.setName("TeamA");
			
			// -> persist하면 영속상태가 된다.
			// 영속상태가 될 때, PK의 값이 세팅이 된 후에 영속상태가 된다.
			// Team을 먼저 생성해서 PK를 생성해야 FK를 생성할 수 있다.
			em.persist(team); 
			
			Member member = new Member();
			member.setName("member1");
			// FK인 Member 에서 PK 객체인 Team을 통째로 가져온다. 
			//member.setTeam(team);
	        em.persist(member);
	        	        
	        //강제 db 쿼리를 보고 싶을 때
	       
	        em.flush();	 // DB에 넣어준다.
	        em.clear();  // 영속성 컨텍스트에 가지고 있던 crud구문을 비워준다.
            
			//============================================================
			// 양방향 매핑
	        Member findSideMember = em.find(Member.class, member.getId());
	        List<Member> members = findmember.getTeam().getMember();
	        
	        for(Member m : members) {
	        	System.out.println("result1 = " + m.getName());
	        }
	        //============================================================
	        
			tx.commit(); // commit하면 영속성 컨텍스트에 들어가기때문에 여기서 테이블 생성
		} catch (Exception e) {
			tx.rollback();
		}finally {
			em.close();
			emf.close();
		}
	}
}
```
> - 양방향 연관관계 주의
>   순수 객체 상태를 고려해서 항상 양쪽에 값을 설정하자
>   연관관계 편의 메소드를 생성하자
>   양방향 매핑시에 무한 루프 조심하자
> -> toString(), lombok lib 조심할 것

> - 양방향 매핑 정리
>   단방향 매핑만으로도 이미 연관관계 매핑은 완료
>   양방향 매핑은 반대 방향으로 조회기능이 추가 된 것 뿐
>   양방향 사용 이유 : JPQL에서 역방향으로 탐색할 일이 많음
>   단방향 매핑을 잘 하고 양방향은 필요할 때 추가해도 됨(테이블에 영향을 주지 않음)
>   결론 : 객체입장에서 양방향 매핑은 필수는 아님, 필요 시에 그때 생성해도 늦지 않는다

> - 연관관계의 주인을 정하는 기준
>   비지니스 로직을 기준으로 연관관계의 주인을 선택하면 안됨
>   연관관계의 주인은 외래 키의 위치를 기준으로 정해야함

# 📌 Item, Member, Order, OrderItem 클래스 양방향 매핑하기
![image](https://github.com/user-attachments/assets/fa39f51e-9ff6-4780-b1d0-b4c678daf4bc)

✔️ Item.java
```
@Entity
@Getter @Setter
public class Item {
	
	@Id @GeneratedValue
	@Column(name = "ITEM_ID")
	private Long id;
	
	@OneToMany(mappedBy = "item")
	private List<OrderItem> orderItem = new ArrayList<OrderItem>();
	
	public void addOrderItem(OrderItem orderItem) {
		orderItem.setItem(this);
		this.orderItem.add(orderItem);
	}
	
	private String name;
	private int price;
	private int stockQuantity;
}
```
---

✔️ Member.java
```
@Entity
@Getter @Setter
public class Member {

	@Id @GeneratedValue
	@Column(name = "MEMBER_ID")
	private Long id;
	
	private String name;
	private String city;
	private String street;
	private String zipcode;
	
	
	@OneToMany(mappedBy = "member")
	private List<Order> order = new ArrayList<Order>();
	
	public void addMember(Order order) {
		order.setMember(this);
		this.order.add(order);
	}
}
```
---

✔️ Order.java
```
@Entity
@Table(name = "ORDERS")
@Getter @Setter
public class Order {

	@Id @GeneratedValue
	@Column(name = "ORDER_ID")
	private Long id;
	
	@OneToMany(mappedBy = "order")
	private List<OrderItem> orderItem = new ArrayList<OrderItem>();
	
	public void addOrderItem(OrderItem orderItem) {
		orderItem.setOrder(this);
		this.orderItem.add(orderItem);
	}
	
//	@Column(name = "MEMBER_ID")
//	private Long memberId;
	
	@ManyToOne
	@JoinColumn(name = "MEMBER_ID")
	private Member member;
	
	
	private LocalDateTime orderDate;
	private String status;
}
```
---
✔️ OrderItem.java
```
@Entity
@Setter @Getter
public class OrderItem {

	@Id @GeneratedValue
	@Column(name = "ORDER_ITEM_ID")
	private Long id;
	
	
	@ManyToOne
	@JoinColumn(name = "ORDER_ID")
	private Order order;
	
	
//	@Column(name = "ORDER_ID")
//	private Long orderId;
	
//	@Column(name = "ITEM_ID")
//	private Long itemId;
	
	@ManyToOne
	@JoinColumn(name ="ITEM_ID")
	private Item item;
	
	private int orderprice;
	private int count;
}
```

# CI
CI는 자동화된 빌드 및 테스트가 수행된 후 개발자가 코드의 변경 사항을 중앙 리포지토리에 정기적으로 병합하는 DevOps 소프트웨어 개발 방식이다
CI의 핵심 목표는 버그를 신속하게 찾아 해결하고, 소프트웨어 품질을 개선하고, 새로운 소프트웨어 업데이트를 검증 및 릴리스하는 데 걸리는 시간을 단축하는 것이다

# CI가 필요한 이유
작업이 완료 되어야지만 마스터 브랜치에 병합하고 수정없이 버그가 누적되는것을 막기 위해서 한마디로 말하자면 소프트웨어 버그를 찾고 수정하는 과정과 소프트웨어 업데이트 과정을 단축시키기 위해서이다

# CI의 장점
- 개발자의 생상성을 향상 시켜준다
- 버그를 좀더 빠르게 발견하고 수정할 수 있다
- 업데이트를 좀더 빠르게 할 수 있다

#  jenkins란
젠킨스는 소프트웨어 개발시 지속적으로 통합 서비스를 제공하는 툴이다
CI(Continuous Integration)툴 이라고 표현한다
다수의 개발자들이 하나의 프로그램을 개발할 때 버전 충돌을 방지하는 용도로 각자 작업한 내용들을 공유영역에 있는 저장소에 빈번히 업로드 함으로써 	지속적인 통합이 가능하도록 해준다

# 젠킨스가 주는 이점
코드의 변경과 함께 이뤄지는 이 같은 자동화된 빌드와 테스트 작업들은 다음과 같은 이점들을 가져다 준다.
- 프로젝트 표준 컴파일 환경에서의 컴파일 오류 검출
- 자동화 테스트 수행
- 정적 코드를 분석하여 코딩 규약에 준수한지 확인
- 프로파일링 툴을 이용한 소스 변경에 따른 성능 변화 감시
- 결함 테스트 환경에 대한 배포작업
- 
이 외에도 젠킨스는 500여가지가 넘는 플러그인을 온라인으로 간단히 인스톨 할 수 있는 기능을 제공하고 있으며 파이썬과 같은 스크립트를 이용해 손쉽게 자신에게 필요한 기능을 추가 할 수도 있다

# ArgoCD란
ArgoCD는 gitOps방식으로 관리되는 Manifest(yaml)파일의 변경사항을 감시하며 현재 배포된 환경의 상태와 Github Manifest 파일에 정의된 상태를 동일하게 유지하는 역활이다
![alt text](image.png)
이전엔 Jenkins로 CD과정을 진행했을때는 SSH로 패키징된 파일을 컨테이너에 넘겨 주었다면 ArgoCD는 CD과정을 진행하는데 Kubernetes로 배포한다고 보면 된다

**ArgoCD는 쿠버네티스의 구성 요소를 배포하기 위해서는 Manifest (ex).yaml 파일을 구성해 실행해야 하는데 이러한 파일들은 계속해서 변경되기 때문에 지속적인 관리가 필요하다**
이를 간편하게 Git으로 관리하는 방식이 GitOps이고 GitOps를 실현시키며 Kubernetes에 배포하는 툴이 ArgoCD이다

# GitHub Actions이란?
GitHub Actions는 GitHub에서 제공하는 CI/CD 도구로, 코드 변경 시 자동으로 빌드, 테스트, 배포 등의 작업을 수행할 수 있다

# GitHub Actions 기본개념
- Workflow: 자동화 작업의 단위 (YAML 파일로 정의)
- Job: 하나의 Workflow 내에서 실행되는 작업 단위
- Step: Job 내에서 실행되는 개별 명령어
- Runner: Workflow를 실행하는 환경 (GitHub 호스트 또는 자체 호스트 가능)
- Event: Workflow를 트리거하는 GitHub 이벤트 (push, pull_request 등)

# GitHub Actions 주요기능
- **트리거(Event)**: push, pull_request, schedule(크론 스케줄러) 등
- **러너(Runner)**: GitHub 제공(기본) 또는 자체 호스트 가능
- **Marketplace 액션**: `actions/checkout@v3`, `actions/setup-java@v3` 같은 재사용 가능한 액션 사용 가능
- **환경 변수 & 시크릿**: `env:`, `secrets.GITHUB_TOKEN` 활용 가능
- **병렬 실행 및 의존성 설정** 가능

# CASCADE란
1. 참조 관계(기본키와 외래키의 관계)가 있을 경우 참조되는 데이터 자동으로 삭제 가능
2. 자신이 참조하고 있는 테이블의 데이터가 삭제되면 자동으로 자신의 데이터 또한 삭제
3. CASCADE 사용시 참조 무결성을 준수할 수 있음
> **참조 무결성**
> 
> 마스터 테이블(DEPT)에는 해당 부서번호(DEPTNO)가 없는데, 슬레이브 테이블(EMP)에는 해당 부서번호가 있는 경우 참조무결성 위배

# CASCADE의 종류
- ON DELETE CASCADE: 부모 레코드가 삭제되면 연결된 자식 레코드도 자동으로 삭제
- ON UPDATE CASCADE: 부모 레코드의 기본 키가 변경되면 연결된 자식 레코드의 기본 키도 자동으로 변경됨

# 사용예시
```
CREATE TABLE parent (
    id INT PRIMARY KEY,
    name VARCHAR(50)
);

CREATE TABLE child (
    id INT PRIMARY KEY,
    parent_id INT,
    name VARCHAR(50),
    FOREIGN KEY (parent_id) REFERENCES parent(id) ON DELETE CASCADE ON UPDATE CASCADE
);
```

# JPA에서의 CASCADE구현
JPA에서는 `CascadeType`을 활용하여 엔터티 간의 연관된 작업을 자동화할 수 있다

# CascadeType의 종류
- ALL: 모든 작업을 전파함
- PERSIST: 부모 엔티티를 저장할 때 자식 엔티티도 같이 저장함
- MERGE: 부모 엔티티를 병합할 때 자식 엔티티도 같이 병합함
- REMOVE: 부모 엔티티를 삭제할 때 자식 엔티티도 같이 삭제함
- REFRESH: 부모 엔티티를 세로고침할 때 자식 엔티티도 같이 세로고침함
- ETACH: 부모 엔터티가 detach될 때 자식 엔터티도 detach됨

# 사용 예시
```
@Entity
class Parent {
    @Id @GeneratedValue
    private Long id;

    private String name;

    @OneToMany(mappedBy = "parent", cascade = CascadeType.REMOVE, orphanRemoval = true)
    private List<Child> children = new ArrayList<>();
}

@Entity
class Child {
    @Id @GeneratedValue
    private Long id;

    private String name;

    @ManyToOne
    @JoinColumn(name = "parent_id")
    private Parent parent;
}
```
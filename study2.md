<h3>스프링부트 쇼핑몰 with JPA</h3>
<h3>책 예제를 통한 학습 중 발생한 문제 해결 기록</h3>
<hr>


<b>이클립스만 사용하다가 이번에 인텔리제이를 사용해보았습니다.</b>
<br>
인텔리제이에서 Lombok을 설치했지만, import하여 사용할 때<br>
Lombok이 적용되지 않아 @Setter, @Getter 등의 어노테이션이 작동하지 않아 에러가 발생하는 경우가 있었습니다. <br>
여러 가지 방법을 검색해보니, 버전을 업데이트하고 IDE를 껐다 켜며 클린 빌드를 하라는 조언이 있었지만, 여전히 같은 에러가 발생했습니다...
<br>

시도한 방법:
1. Lombok 재설치
2. Rebuild: 상단 메뉴바에서 Build > Rebuild 선택
3. Restart: 상단 메뉴바에서 File > Invalidate Caches / Restart 선택

<br>
해결 방법:<br>
`최신 버전으로 IntelliJ IDEA 자체를 업데이트` 하여 문제를 해결했습니다. <br>
예전에 인텔리제이를 설치한 후 업데이트를 하지 않은 것이 문제의 원인이었습니다.
<br>
<br>
<hr>

<h4>출력 결과를 OrderBy 키워드를 이용하면 오름차순 또는 내림차순으로 조회할 수 있다.</h4>
1. 오름차순 OrderBy + 속성명 + Asc
2. 내림차순 OrderBy + 속성명 + Desc
   
<br>

````

    @Test
    @DisplayName("가격 내림차순 조회 테스트")
    public void findByPriceLessThanOrderByPriceDesc (){
        this.createItemList();
        List<Item> itemList = itemRepository.findByPriceLessThanOrderByPriceDesc(10005);
        for(Item item :itemList){
            System.out.println(item.toString());
        }
    }

    출력결과 ⬇️
    Item(id=4, itemNm=테스트 상품4, price=10004, stockNumber=100, itemDetail=테스트 상품 상세 설명4, itemSellStatus=SELL, regTime=2025-02-12T15:45:16.302508, updateTime=2025-02-12T15:45:16.302514)
    Item(id=3, itemNm=테스트 상품3, price=10003, stockNumber=100, itemDetail=테스트 상품 상세 설명3, itemSellStatus=SELL, regTime=2025-02-12T15:45:16.301645, updateTime=2025-02-12T15:45:16.301653)
    Item(id=2, itemNm=테스트 상품2, price=10002, stockNumber=100, itemDetail=테스트 상품 상세 설명2, itemSellStatus=SELL, regTime=2025-02-12T15:45:16.300487, updateTime=2025-02-12T15:45:16.300497)
    Item(id=1, itemNm=테스트 상품1, price=10001, stockNumber=100, itemDetail=테스트 상품 상세 설명1, itemSellStatus=SELL, regTime=2025-02-12T15:45:16.252677, updateTime=2025-02-12T15:45:16.252692)


````

<br>
쿼리 메소드는 간단한 쿼리를 처리할 때는 유용하지만
복잡한 쿼리를 다루기에는 적합하지 않다.
이를 보완하기 위해선? Spring Data JPA 에서 제공하는 @Query 어노테이션 이용하면 SQL 과 유사한 JPQL 이라는 객체지향 쿼리 언어를 통해 통잡한 쿼리도 처리가 가능하다.

<br>

````
    //@Query 어노테이션을 이용하여 상품 데이터 조회

    @Query("select i from Item i where i.itemDetail like %:itemDetail% order by i.price desc")
    List<Item> findByItemDetail(@Param("itemDetail") String itemDetail);
````
<br>

하지만 @Query 어노테이션을 이용한 방법에도 단점이 있다.
JPQL 문법으로 문자열을 입력하기 때문에 잘못 입력하면 컴파일 시점에 에러를 발견할 수 없다.
이를 보완할 수 있는 방법? Querydsl

<br>
- Querydsl 의 장점?<br>
  1. 고정된 SQL문이 아닌 조건에 맞게 동적응로 쿼리를 생성할 수 있음<br>
  2. 비슷한 쿼리를 재사용할 수 있다<br>
  3. 문자열이 아닌 자바 소스코드로 작성하기 때문에 컴파일 시점에 오류를 발견할 수 있다<br>
  4. IDE 자동완성 기능을 이용할 수 있기 때문에 생산성 향상!<br>
<br>
- 사용방법?<br>
1. pom.xml 의존성 추가하기<br>
2. Qdomain 자바 코드를 생성하는 플러그인 추가하기 (엔티티를 기반으로 접두사로 Q가 붙는 클래스들을 자동으로 생성해주는 플러그인이다.)
<br>
<br>

````
        <dependency>
			<groupId>com.querydsl</groupId>
			<artifactId>querydsl-jpa</artifactId>
			<version>4.3.1</version>
		</dependency>
		<dependency>
			<groupId>com.querydsl</groupId>
			<artifactId>querydsl-apt</artifactId>
			<version>4.3.1</version>
		</dependency>



        <plugin>
				<groupId>com.mysema.maven</groupId>
				<artifactId>apt-maven-plugin</artifactId>
				<version>1.1.3</version>
				<executions>
					<execution>
						<goals>
							<goal>process</goal>
						</goals>
						<configuration>
							<outputDirectory>target/generated-sources/java</outputDirectory>
							<processor>com.querydsl.apt.jpa.JPAAnnotationProcessor</processor>
						</configuration>
					</execution>
				</executions>
		</plugin>
````
<br>
<br>
<hr>
에러 메시지:

`1 problem was encountered while building the effective model for org.javassist:javassist:jar:3.18.2-ga` 

Maven 빌드 환경에서 QueryDSL을 사용하기 위해서는 다음과 같은 의존성 주입이 필요합니다.


````
☑️ pom.xml - 의존성 설정
<dependency>
    <groupId>com.querydsl</groupId>
    <artifactId>querydsl-jpa</artifactId>
    <version>4.3.1</version>
</dependency>
<dependency>
    <groupId>com.querydsl</groupId>
    <artifactId>querydsl-apt</artifactId>
    <version>4.3.1</version>
</dependency>


☑️ pom.xml - 플러그인 설정
 <plugin>
    <groupId>com.mysema.maven</groupId>
    <artifactId>apt-maven-plugin</artifactId>
    <version>1.1.3</version>
    <executions>
        <execution>
            <goals>
                <goal>process</goal>
            </goals>
            <configuration>
                <outputDirectory>target/generated-sources/java</outputDirectory>
                <processor>com.querydsl.apt.jpa.JPAAnnotationProcessor</processor>
            </configuration>
        </execution>
    </executions>
</plugin>
````

하지만 pom.xml에 Qdomain이라는 자바 코드를 생성하는 플러그인을 추가하고 의존성을 받아오려고 해도 Qitem 클래스가 생성되지 않았고, 아래와 같은 에러가 발생했습니다.

````
1 problem was encountered while building the effective model for org.javassist:javassist:jar:3.18.2-ga
````

이 오류 메시지는 Maven이 javassist 라이브러리의 효과적인 모델을 빌드하는 과정에서 문제가 발생했음을 나타냅니다. 여러 가지 방법을 시도해본 결과, 의존성의 버전을 올리니 문제가 해결되었습니다
`4.3.1 -> 5.0.0` ...

````
        <dependency>
			<groupId>com.querydsl</groupId>
			<artifactId>querydsl-jpa</artifactId>
			<version>5.0.0</version>
			<classifier>jakarta</classifier>
		</dependency>

		<dependency>
			<groupId>com.querydsl</groupId>
			<artifactId>querydsl-apt</artifactId>
			<version>5.0.0</version>
			<classifier>jakarta</classifier>
		</dependency>
````

이렇게 변경한 후 Qitem 클래스가 정상적으로 생성되었습니다.
<br>
<br>
<hr>

에러 메시지: `java: Attempt to recreate a file for type com.shop.entity.QItem`

해결 방법:
1. pom.xml 파일에서 com.mysema.maven 관련 설정을 삭제합니다.
2. Maven 버튼(M)을 클릭한 후, 프로젝트 내의 Lifecycle에서 캐시 값을 삭제하고 clean을 실행합니다.
3. compile을 다시 실행하면 문제가 해결되어 정상적으로 작동합니다.

````
<plugin>
        <groupId>com.mysema.maven</groupId>
        <artifactId>apt-maven-plugin</artifactId>
        <version>1.1.3</version>
        <executions>
            <execution>
                <goals>
                    <goal>process</goal>
                </goals>
                <configuration>
                    <outputDirectory>target/generated-sources/java</outputDirectory>
                    <processor>com.querydsl.apt.jpa.JPAAnnotationProcessor</processor>
                </configuration>
            </execution>
        </executions>
</plugin>

//삭제
````
<br>
<br>
<hr>

Registry에서 compiler.automake.allow.when.app.running 항목이 없어서 검색해보니 
2021.2 버전부터는 Registry가 아닌 Advanced Settings에서 이 설정을 조정해야 한다고 합니다.

해결 방법:
- <h4>소스를 수정할 때 자동으로 빌드가 되도록 설정하기</h4>
  1. 설정 경로: Settings > Build, Execution, Deployment > Compiler<br>
  2. 여기서 Build Project Automatically 옵션을 체크합니다.<br>
- <h4>애플리케이션이 실행 도중 이어도 자동으로 재실행될 수 있도록 허락하기</h4>
  1. 설정 경로: Settings > Advanced Settings<br>
  2. Allow auto-make to start even if developed application is running 옵션을 체크합니다..<br>

​
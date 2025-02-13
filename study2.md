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
<br>
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
이클립스만 사용하다가 이번에 인텔리제이를 사용해보았습니다. <br>
인텔리제이에서 Lombok을 설치했지만, import하여 사용할 때 <br>
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
# TTBKK - BACKEND INFO (작성 : 정지원, 구경완)

# 목차
- 백에서 DB 저장되는 Entity 와 데이터  
- DB 에 자주 저장 되는 데이터    
- DB 에 자주 접근 하는 데이터  


# 백 ->DB 저장되는 Entity 와 데이터   
  
**- 각 타입들은 DB 스펙에 맞춰서 들어가게 됩니다.**
ex) User type 데이터가 DB에 저장 될때에는 User의 기본키 값으로 들어가게 됩니다.
**- 각 엔티티들의 첫번째 데이터들은 DB의 기본키로 저장됩니다.**

| User |  |
|--|--|
|id - (String)| Random UUID ("-" 뺀값) |
|role - (Enum)| 권한 (USER,ADMIN,SUPER_ADMIN) |
|socialId - (String)|소셜로그인 고유의 식별값|
|socialType - (String)|소셜로그인 플랫폼 (EX 구글)|
|createdAt - (LocalDateTime)|해당 유저가 만들어진 시간|
|updatedAt - (LocalDateTime)|해당 유저가 업데이트 된 최근시간|
---

| Place |  |
|--|--|
|id - (String)| Random UUID ("-" 뺀값) |
|name - (String)|장소 이름 (Ex) XX떡볶이-양재점)|
|latitude - (BigDecimal)|해당 장소의 위도 값 (Map API 데이터 사용)|
|longitude - (BigDecimal)|해당 장소의 경도 값 (Map API 데이터 사용)|
|isDeleted - (Boolean)|휴면계정이나 회원탈퇴 같은 기능 구현시 필요한 값 (휴면&탈퇴 유저가 쓴 데이터들 구분하기 위한 값 - 복구해야할 수도 있기 때문에.)|
|description - (String)|해당 장소에 대한 설명 (EX) 이 가게로 말할거 같으면 ...)|
|address - (String)|해당 장소의 주소 (Map API 데이터 사용)|
|createdBy - (User)|해당 장소를 만든 User|
|updatedBy - (User)|해당 장소를 최근에 Update 한 User|
|brand - (Brand)|해당 장소의 브랜드 가맹점이 없다면 "로컬"(EX)신전떡볶이, 엽기떡볶이 등)|
|createdAt - (LocalDateTime)|해당 장소가 만들어진 시간|
|updatedAt - (LocalDateTime)|해당 장소가 업데이트 된 최근시간|
 ___
| Hashtag |  |
|--|--|
|name - (String)| 해당 해쉬태그의 이름. 쉼표로 구분. Ex)존맛, 미쳤다 등등..|
|createdAt - (LocalDateTime)|해당 해쉬태그가 만들어진 시간|
---
| Brand |  |
|--|--|
|id - (String)| Random UUID ("-" 뺀값) |
|name - (String)| 브랜드 이름. 가맹점이 없다면 "로컬"(EX)신전떡볶이, 엽기떡볶이 등)  |
|description - (String)|해당 브랜드에 대한 설명|
|createdBy - (User)|해당 브랜드를 생성한 User|
|updatedBy - (User)|해당 브랜드를 최근에 Update 한 User|
|createdAt - (LocalDateTime)|해당 유저가 만들어진 시간|
|updatedAt - (LocalDateTime)|해당 유저가 업데이트 된 최근시간|
---
  
# DB 에 자주 저장 될거라 예상되는 데이터 순위  
1. **Place** ( Brand 를 저장하기 위해선, Place 를 작성해야 되고 Brand를 저장하지 않아도 이미 저장되어있다면 Place 만 따로 저장할 수 있기 때문에 장기적으로 보았을때에는 Place 가 가장 저장이 많이 될거라 예상된다.)
2. **Hashtag** ( Hashtag 는 정해진 형식이 제약된 것이 없고, 다양하게 유저가 만들어낼 수 있으므로 초반에 가장 자주 저장 될거라 예상된다. 하지만 장기적으로 보았을때에는 자주 접근 되지 않을거라 예상된다.)
3. **Brand** ( Brand 도 초반 데이터가 없을 경우에는 자주 저장 될 수 있겠지만 어느 정도 데이터가 생기고 장기적으로 보았을때에는 자주 접근 되지 않을 거라 예상된다. Update 측면에서도 해당 브랜드의 신메뉴가 나오거나, 이슈가 터지거나 이런 특별한 일이 발생할때 , 해당 브랜드의 Description update 가 발생 할거라 예상된다.)
4. **User** (데이터를 저장할때 해당 USER 정보 또한 저장 하게 되어있으므로 , 서비스에 처음 저장 하기 위해선 User 데이터 저장이 필수이다. 하지만 처음 회원가입 즉 User 에 대한 정보가 저장이되면 Update 할 내용도 없고 자주 저장 되지 않는 데이터라 예상된다. 더하여 조회 같은 기능이 로그인 없이 가능 하다면 User 저장 빈도는 더 낮아질 거라 예상된다.)  
  
___
# DB 에 자주 접근 하는 데이터  

1. Place ( 해당 서비스를 이용하는 유저들이 가장 이용하려는 목적은 내가 좋아하는 떡볶이 맛집을 공유하려는 것도 있지만 공유 받고 싶어서 내주변의 맛있는 떡볶이집이 어디있는지를 알고 싶어 하는 유저가 많을거라 예상된다. 더하여 가장 많은 시간동안 유저가 노출되는 기능을 예상해보면 지도안에서 고정 크기의 사각형 안의 장소들을 보여주는 기능(Grid API)을 가장 많이 사용할 거같다. 

ps. 기술적인 내용을 최대한 쓰지 않으려 했지만 이 내용을 필요할거 같아 첨부한다. 고정 크기의 사각형 안의 장소들을 보여주는 기능을 개선하기 위해서 Redis 라는 기술을 도입했다. 하지만 결국 Redis 도 시간이 지나고 요청을 하면 DB에 접근을 하기 때문에 그부분도 고려 하여 순위를 매겼다.

2. HashTag ( 검색 기능이 추가 된다면 브랜드를 검색하기 보단 해쉬태그를 이용한 검색이 자주 이용될거라 예상된다. EX) "로제 떡볶이", "존맛" 등등 , Place 조회 할때 Hashtag 도 같이 조회된다.)
3. Brand ( Place 를 조회 할때 Brand 도 같이 조회되기 때문에)
4. User ( User 를 DB에서 직접 접근하기 보단 토큰을 통해서 권한 이든, userId 든 얻어 올 수 있기 때문에 )

## 적으면서 궁금한 점 & 건의할 점 

<건의할 점>
isDeleted 같은 경우 default 값을 false 로 박아두는게 어떤가요?

<궁금한 점>
updatedBy - 해당 장소를 최근에 Update 한 User 이걸 언제쓰이는지 설명을 듣고싶습니다.

place, brand 등 에서 description 은 모든 유저가 접근 가능한지? 

해쉬태그의 수는 제한이 있는지?

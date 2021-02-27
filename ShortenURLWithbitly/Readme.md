Bitly v4 API를 이용하여 bitly shorten URL 자동 반환기를 만들어보자
===
***


### Bitly란?

  - [Bitly](https://bitly.com/) 는 단축 도메인 서비스이다. 단축도메인이란 주소를 축소하여 제공하는 서비스로, '축약 도메인'이라고도 한다. 영어로는 URL shortening이라고 한다.
  
  - 이 bitly의 서비스인 단축도메인의 장점과 단점으로는 무엇이 있을까?
  
    1. 장점
    	
        - 원래 도메인을 훨씬 짧게 해주기에, 복잡한 문자배열로 인해 떨어지는 가독성을 보완해줄 수 있다.
    
    2. 단점
    
    	- 단축 주소이기에, 실제주소가 아니다. 그렇기에 해당 단축 URL이 어디로 연결되는지 알 방법이 없다. 그렇기에 스팸문자, 피싱사이트에서 많이 악용되기도 한다.
 
 - 단축 URL서비스를 제공하는 다양한 사이트
 
   - bitly
   
   - abit.ly
   
   - flic.kr
   
 - 이중 오늘은 가장 보편적으로 쓰이기도 하며, API가 제공되는 Bitly를 활용해볼 것이다.

### API사용 준비하기

  1. 우선 API준비를 위해서 bitly에 로그인을 하자 그러면 아래와 같이 창이 나오는것을 볼 수 있다.
  
  ![](https://images.velog.io/images/andrewyoon10/post/356767b4-5b67-406d-b9fa-6e5a983a3cf2/1.png)
  
  2. 만약 로그인을 OAuth를 이용해서 로그인(Google, Facebook등으로) 하였다면, 좌측 상단 Profile Settings에서 change password를 하여 password를 한번 바꿔주고 와야한다.
  
  ![](https://images.velog.io/images/andrewyoon10/post/84cd385d-faa3-476d-92d7-e73e3c4dfe95/2.png)
  
  3. 그 후 다시 Profile Settings로 들어온 후에 Generic Access Token이라는 부분이 있다. Generic Access Token은 API 토큰 값을 얻는 부분이다. 
  ![](https://images.velog.io/images/andrewyoon10/post/192b7045-005c-4b79-82ad-3706c0ada64f/3.png)
  
위의 사진을 보면 'Your current default is ~~~'라고 하는 부분이 있다. 여기서 나타내는 값은 내가 shorten URL을 받기위해 사용할 API Default Group이다.(크게 신경 안써도됨) 키 값을 얻기 위해서는 밑에 password 텍스트 상자에 비밀번호를 입력하고 Generate Token을 클릭하면 API Token이 커서에 복사된다.

### 본격적으로 API를 사용해보자

  - 자 API를 활용할 것이다. 당연히 API든 패키지든 가장 정확하면서 많은 Reference정보를 담고 있는것은 Document이다. [bitly API의 Documentation을 우선 들어가자](https://dev.bitly.com/api-reference)
  
  - 보면 뭔가 되게 많다
  
    ![](https://images.velog.io/images/andrewyoon10/post/41381259-2e24-42e8-99aa-59613f6dbcb2/4.png)
  
   이 포스트에서는 기본적인 API활용만 다루기에 가장 기본적인 shorten URL API만 활용할 것이다.
   
  - 우선 Bitlinks를 클릭해보자 그러면 아래와 같은 창이 나온다.
  
    ![](https://images.velog.io/images/andrewyoon10/post/6c49b2a7-fe14-44a0-be82-009ddb260235/5.png)
    
    기본적으로 알 수 있는 것들을 정리해보자
    
    - Request HTTP Method : POST
    
    - API End Point : /v4/shorten
    
    - Parameters
    
      - long_url : shorten URL로 반환받고 싶은 URL을 의미한다. 필수 매개변수이다.
      
      - domain : bit.ly를 고정으로 해주면 된다.
      
      - group_guid : ?
    
    자 알 수 있는것들을 간추려 보았다. 여기서 정말 애매한 것이 있다. Parameter중 group_guid라는 것은 'string'타입의 value라는것 외에는 알 수 없다. 이 부분을 이제 Document에 가서 찾아보면 아래와 같은 내용이 있다.
    
    ![](https://images.velog.io/images/andrewyoon10/post/5b1d371c-0fae-4b8a-80fa-28e59a706f65/6.png)
    
    해석해보면 계정에 있는 사용자 그룹을 식별합니다. 모든 사용자는 조직 내에서 하나 이상의 그룹에 속합니다. API에 대한 대부분의 작업은 그룹을 대신합니다.  기본적으로 GUID의 의미는 '전역 고유 식별자'(Globally Unique Identifier)인데, 말 그대로 API Token을 발급받을때 소속된 API Default Group에 대한 식별 코드를 의미하는 것이다.
  
  - 그렇다면 이제 필요한 GUID값을 얻어보자. GUID는 API Reference의 Group의 Retrieve Group에서 얻을 수 있다.
  
    ![](https://images.velog.io/images/andrewyoon10/post/0065129e-d7f7-40b5-bbc4-e2512736ecfd/7.png)
    
    GUID를 얻기 위해서는 Query Parameter에 Token Value를 header에 넣어주어 GET으로 요청하면 된다.
    [Postman](https://web.postman.co/home) 을 활용해서 API를 테스트해보자
    ![](https://images.velog.io/images/andrewyoon10/post/c6b081be-728f-41da-b44f-20780d4d86e3/8.png)
    
    요청을 한 결과 JSON에서 "guid"값이 있는것을 볼 수 있다. 파이썬 코드는 아래와 같다.
  ![](https://images.velog.io/images/andrewyoon10/post/94576bc1-2aa6-41df-8e3e-fe7cc763561b/9.png)
  
  - 이제 shorten URL을 API를 통해 받아보자. 위에서 보았던 shorten a link부분을 활용하여 API 테스트를 진행하자. 주의해야할 것은 shorten a link는 POST메소드를 통해 reqeust를 해야한다. 여기서 예시로 네이버 뉴스의 한 기사를 bit.ly shorten URL로 받고싶다고 가정하자.([예시 링크](https://news.naver.com/main/read.nhn?mode=LSD&mid=shm&sid1=101&oid=001&aid=0012230040))
  
  ![](https://images.velog.io/images/andrewyoon10/post/eedc591d-1480-4533-90b1-eda02618b647/10.png)
  
  ![](https://images.velog.io/images/andrewyoon10/post/f474bde2-ea7d-4229-9460-9ef187c9776d/11.png)
  
  결과적으로 response JSON 부분의 "link"부분이 shorten URL부분이다.
  
  - 이 글을 작성하고 있는날 오전, 사지방에서 간단하게 bitly API를 활용하여 자동 bit.ly shorten URL 반환기를 만들어 보았다 깃허브와 전반적인 코드 사진은 아래에 있다.
  
  ![](https://images.velog.io/images/andrewyoon10/post/b56fa25d-0d35-447c-b478-0cbcf5b02578/13.png)
  
  Github : https://github.com/J-hoplin1/Lots-Of-Useful-Things/blob/main/ShortenURLWithbitly/useBitlyAPI.py
  
  Code
  
  ![](https://images.velog.io/images/andrewyoon10/post/3f351a89-9b47-439f-b465-6846366b718b/12.png)

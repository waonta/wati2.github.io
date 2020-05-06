---
layout: post
title: 클론 코딩 - 언럭키 페북
date: 2020-05-05
---

### Unlucky Facebook


##### 계획

1. 페이스북에서 보여지는 기능들 중에 구현하고 싶은 기능을 선정
2. 기능구현을 최소한의 노력을 통하여 구현하도록 계획
3. 욕심을 버리고 구현하고자 하는 것에 집중

##### DB에서 만들 Table 총 3개


###### 1. Post Table

{:.table .table-striped .table-bordered .table-sm}
|postId|제목|내용|계정ID|작성일|좋아요|
|---|---|---|---|---|---|
|001|일상|잤다|김|2020-05-05|3|
|002|맛집탐방|먹었다|박|2020-05-05|3|

포스트 관리 위한 테이블
포스트와 댓글은 **1:N** 관계  
**postId = primaryKey**

###### 2. 댓글 Table

{:.table .table-striped .table-bordered .table-sm}
|replyId|postId|댓글순서|대댓글순서|댓글내용|시간|계정ID|
|---|---|---|---|---|---|---|
|1|001|1 |0|맛있다      | 12:01 | 김 |
|2|001|2 |0|양 ㄷㄷ     | 12:02 | 이 |
|3|001|3 |0|비주얼 ㄷㄷ | 12:03 | 방 |
|4|001|1 |1|혼자먹냐?   | 13:50 | 문 |
|5|001|1 |2|담에 ㄱ?    | 17:00 | 김 |

**댓글순서**는 글에서 댓글의 순서를 의미함 1이면 글의 첫번째 댓글임  
**대댓글순서**는 0인경우 기본댓글을 의미한다 0보다 큰경우 대댓글임  
**replyId = primaryKey**

###### 3. User Table (후순위)

{:.table .table-striped .table-bordered .table-sm}
|id | pw | friend |
|---|---|---|
|김| 1111 | 방,소 |
|이| 1111 | 방,소 |
|방| 1111 | |
|소| 1111 | |
|korean10| 1111 | 김 |
|korean20| 1111 | 이 |

유저 아이디 암호 친구관계 나타내는 테이블  
특정 계정이 포스트한 게시물을 제한 할때 필요


##### View
1. 포스트 리스트 화면
2. for문으로 전체 post in posts 통해 제목 내용 가져옴
3. for문 안에서 댓글관련 for문 수행 reply in replys && 해당 Post ID 만 출력 + 순서와 대댓글 컬럼을 통한 오름차순 적용  

##### View와 DataBase간 주고받을 데이터
1. Post List ( 전체 혹은 Limit 5 ), 변수명: posts 
2. 1에서 나온 Post id에 관한 댓글 데이터들, 변수명: replys
3. http get만 사용해도 구현 가능
{% highlight json linenos %}
    {
        postId_1:{컬럼1:값, 컬럼2:값 ...}, 
        postId_2:{컬럼1:값, 컬럼2:값 ...}
    }
{% endhighlight %}  
{% highlight json linenos %}
    {
        replyId_1{컬럼1:값, 컬럼2:값 ...},
        replyId_2{컬럼1:값, 컬럼2:값 ...}
    }
{% endhighlight %} 

##### 후순위 (아직 구현예정 없음)
  * 회원가입 기능 / View
  * 로그인 기능 / View
  * 친구관계에따른 포스트 View 차이 구현

---
title : '개발 블로그 시작하기(티스토리, velog, github)'
date : 2024-05-20T14:39:37+09:00
draft : false
authors : ["braveseokyung"]
description : 미루고 미뤄온 개발블로그 ... 시작하기
categories : ["blog"]
series : ["hugo-blog"]
series_weight : 1
toc:
    auto: false
---

<!--more-->

## 블로그 고민의 역사
블로그를 만들어야겠다고 결심한지 거의 2년.. 
글을 쓰기도 전에 고민해야할 게 너무 많아서 블로그를 본격적으로 할 수가 없었다. 플랫폼이랑 스킨이랑 기왕 하는거 예쁘게 꾸미고 싶은 마음에 이것저것 깔짝대기만 하다가, 타블로그 친구들 덕분에 미뤄왔던 블로그 만들기를 해치울 수 있었다. 

나는 결과적으로 이번 시리즈에서 설명할 Hugo 블로그로 정착했는데, 나에게 중요했던 부분들은 다음과 같다.

### 블로그를 선택한 기준
* TOC가 제공되어야 함
* 코드 작성과 latex 작성이 편해야 함 (markdown으로 모두 가능하게)
* 가독성이 좋아야 함(내기준: 줄간격이 너무 넓지 않고 단락 구분이 명확해야 함)
* 하이라이팅(코드, 콜아웃)이 예쁘면 좋겠음
* 물론 위에 것들은 어느정도 커스텀이 가능하지만, 웬만하면 처음부터 잘 구성된 테마를 골라서 쉽게 글을 쓸 수 있으면 좋겠음

너무 까다롭다 보니 하나를 쓰다보면 뭔가 조금씩 아쉽고 그래서 글 한개 쓰다말고 다른 걸 깔짝대보고의 반복이었다. 아래는 내가 써봤던 플랫폼과 테마인데, 고민했던 만큼 충분히 장점이 있는 것들이라 참고가 되었음 좋겠다.

### velog
개발자 블로그로서 필요한 것들을 웬만하면 다 갖추고 있어서 엄청 편하다.(코드블럭, latex, TOC)
특이하게 시리즈로 글을 쓸 수 있는데, 같은 주제를 시간을 두고 공부를 하면서 쓸 때 굉장히 유용한다.

하지만 태그를 카테고리롤 사용하는 것이 좀 불편하고, 뭔가뭔가 디자인이 아쉽다.. 특히 콜아웃이 초록색 줄에 회색바탕인게 눈에 잘 들어오지 않았다.


### 티스토리
티스토리 역시 테마만 잘 고르면 편하게 글을 작성할 수 있는데, 나는 특히
[Hello](https://pronist.tistory.com/5#%EA%B8%B0%EB%8A%A5-1)
이 테마가 마음에 들었다. 

그런데 내가 블로그를 쓰는데 서툴어서 그런지, 다 쓴 글을 봤을 때 가독성이 떨어진다는 느낌을 받았다. 그리고 마크다운으로 쓰는 것이 사실상 불가능해서 내가 기존에 정리해둔 글들을 편하게 옮기기 어려웠다.

### Github - Jekyll
사실 github은 너무 어려워서 처음에는 생각하지 않았다. 그치만 구글링하다가 .io로 끝나는 블로그들을 보니까 너무 간지나고 또 글을 쓸 때마다 잔디를 심을 수 있다는 말에 홀려서 시작하게 됐다 ㅎㅎ

테마는 [Chirpy](https://github.com/cotes2020/jekyll-theme-chirpy) 로 골랐는데, 정말 고생했다 ... 
Chirpy는 처음 하는 사람이 적용하기에는 좀 어려운 테마인 것 같다. 

힘들게 적용해서 정착하려고 했지만 또 그놈의 뭔가뭔가 아쉬워 타령 시작. 전체적으로 흐릿한 분위기가 뭔가 가독성이 떨어지는 느낌이 들어 눈물을 머금고 포기했다. 또 빌드하고 결과물을 보기까지 시간이 10분 정도 걸리는데, 이것도 성격급한 나한테 잘 맞지 않았다.

### 마지막! Github - Hugo
Github 블로그를 만드는 방법이 Jekyll 말고 또 있다는 것을 아는가!!
나는 Github으로 블로그를 만드는 것이라고 생각했는데, 사실 github은 그냥 페이지를 배포하는 방법일 뿐이고, 블로그 페이지 자체는 site generator라고 해서 블로그 틀을 제공하는 툴을 사용하는 것이었다. Jekyll은 그 site generator의 한 종류이고, Hugo도 마찬가지이다!

공부하다가 우연히 본 블로그의 테마가 마음에 들었는데, 검색해보니까 Hugo의 [DoIt](https://themes.gohugo.io/themes/doit/) 테마였다. 이 테마에 반해서 무작정 Hugo로 결정되었다.

내가 생각하는 이 테마의 장점은 이렇다.

* 테마가 전반적으로 선명함(소제목, 글 구분이 뚜렷, 한 문단안에서는 줄간격 좁음)
* 하이라이팅이 정말정말 예쁨 종류도 많고!
* TOC, latex, 코드블럭 모두 가능
* velog의 장점인 series도 있음
* 테마를 설명하는 [documentation](https://hugodoit.pages.dev/theme-documentation-basics/)이 상세해서 적용하기 (비교적) 쉬움.
* local에서 `hugo server -D` 로 빌드 결과를 바로바로 확인할 수 있음.

이것저것 썼지만, 결국 예쁜 테마가 짱이다! 가 결론이다 ㅎㅎㅎ

## Github blog 변경시 주의사항
나처럼 이미 {username}.github.io로 블로그를 만든 상태에서 블로그를 다시 만들 경우, 꼭 **사이트 게시 취소 또는 삭제**를 해야한다. 그래야 그 도메인을 다시 쓸 수 있다.

리포지토리를 삭제하는 것이 가장 쉬운 방법이지만, 리포지토리에서 settings-pages로 들어가 unpublish site를 누르면 된다.
{{<image src="스크린샷 2024-05-20 오후 4.02.43.png">}} 

그리고 내가 삽질한 부분은 여기인데 아무리 테마를 적용해도 사이트가 바뀌지 않아서 적용이 안됐구나, 하고 다시 해보고 다시해보고 반복하다가, 그냥 캐시가 삭제되지 않아 내 컴퓨터에서 옛날 블로그로 연결되고 있다는 사실을 깨달았다..

{{< admonition failure "페이지 삭제되지 않는 현상" >}}
웹캐시가 남아있어서 페이지가 계속 연결될 수 있다
강력 새로고침( cmd + shift + r) 을 해보자!

[ref: 깃헙 블로그 삭제되지 않는 현상](https://godtaehee.tistory.com/1) 
{{< /admonition >}}

기존 블로그를 삭제했다면 준비는 끝! 다음 글에서는 본격적으로 Hugo 블로그를 만들고 Github pages로 호스팅해보자.





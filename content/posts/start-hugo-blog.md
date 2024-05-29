---
title : '블로그 만들기 with Hugo & Github'
date : 2024-05-21T15:20:41+09:00
draft : false
authors : ["braveseokyung"]
description : Hugo로 블로그 만들고 Github Pages로 배포하기
categories : ["blog"]
series : ["hugo-blog"]
series_weight : 2
toc:
    auto: false
---

<!--more-->
## 개발환경
Mac OS

## Install Hugo
terminal을 열고

```bash
brew install hugo
```

## 로컬에서 Hugo 프로젝트 생성
로컬에서 원하는 위치에 Hugo 프로젝트를 생성한다. 이 프로젝트가 블로그가 되므로, 나는 blog라고 생성했다.
```bash
hugo new site blog
```
### 테마 추가하기
[휴고 테마](https://themes.gohugo.io/) <-여기서 원하는 테마를 고른다.
내가 고려한 테마는 [PaperMod](https://themes.gohugo.io/themes/hugo-papermod/),[Lowkey](https://themes.gohugo.io/themes/lowkey-hugo-theme/),[DoIt](https://themes.gohugo.io/themes/doit/) 이였고 그 중 이 블로그의 테마인 DoIt을 선택했다!

선택한 테마의 github repository에 들어가 https 주소를 복사해온 후, 위에서 생성한 프로젝트 폴더에 themes/{테마 이름} 으로 추가해준다.
이때 'submodule'을 사용하는데, 요건 뭔지 아직 잘 모르겠으요..

```bash
cd blog
git init
git submodule add https://github.com/HEIGE-PCloud/DoIt.git themes/DoIt
```
위의 과정을 완료하면 blog 프로젝트 폴더 안에 다음과 같은 폴더들이 생긴다.
{{<image src="스크린샷 2024-05-21 오후 3.46.29.png">}}
public 폴더는 나중에 생성되는 폴더이므로 없어도 괜찮다. 또 config 폴더는 블로그의 제목, 설명, TOC 등등 여러 설정에 대한 폴더인데, 조금 이따가 설명하겠다. 지금은 config 폴더 대신에 hugo.toml 파일이 있어야 한다.

이 hugo.toml 파일로 들어가면 
```toml
baseURL = 'https://braveseokyung.github.io/'
languageCode = 'en-us'
title = 'brog'
```
이렇게 되어있을텐데, baseURL, title을 바꿔주고
`theme='DoIt'`을 추가해준다.

### 로컬에서 블로그 확인하기
테마를 가져왔으면 터미널에 다음을 입력한다.
```bash
hugo serve
```
http://localhost:1313/ 에 접속했을 때 블로그 페이지가 뜬다면 로컬에서 블로그 만들기 성공!

## Github pages로 배포하기
로컬에서 만들어진 Hugo 블로그를 Github pages로 배포해보자.

### Github Repository 생성
Github에서 Repository 2개를 생성한다.
하나는 로컬의 blog 폴더와 연결될 repository이고, 다른 하나는 글과 관련된 public 폴더가 올라갈 repository이다.
첫번째는 어떤 이름으로 생성해도 상관없지만, 두번째는 반드시 {계정이름}.github.io 로 생성해야한다. 

### 로컬 폴더와 연결하기
```bash
# 블로그 폴더 연결, blog 폴더 안으로 들어와서 실행
git init
git remote add origin {github blog repository https 주소}

# {계정이름}.github.io를 연결할 submodule public을 생성
git submodule add -b main {github .io repository https 주소} public

hugo -D

# {계정이름}.github.io repository에 로컬 public 폴더 업로드
cd public
git add .
git commit -m "First commit"
git push origin main

# blog repository에 로컬 blog 폴더 업로드
cd ..
git add .
git commit -m "First commit"
git push origin master
```

### 배포 확인하기
{계정이름}.github.io repository의 Settings로 들어가서 왼쪽 menu들 중 Pages를 클릭
{{<image src="스크린샷 2024-05-21 오후 4.04.58.png">}} 
이미지 내용대로 설정되어있는지 확인하고, 사이트를 클릭!

사이트가 웹에 배포되엇다!!!

## 첫 글 작성하고 글 설정 세팅하기(configuration)
아마 위의 과정을 거쳤으면 배포는 되었어도 텅 빈 페이지가 나올 것이다. 첫 글도 작성하고 페이지도 나에게 맞춰 설정을 바꿔보자.

Hugo의 장점은 글을 작성하면서 로컬에서 결과를 확인할 수 있다는 점이다!
```bash
hugo server -D
```
로 draft로 작성하고 있는 글들도 미리 확인해볼 수 있다. 또 exampleSite 폴더에 있는 post .md파일들이 DoIt Demo 블로그에 있는 글들이기 때문에 비교해보면서 syntax를 익히면 글을 쓰는 게 조금 더 수월할 것이다. 그럼 첫 번째 포스트를 생성해보자!

### 로컬에서 글 생성하기
```bash
hugo new posts/first-post.md
```

blog-content-posts 파일에 first-post.md가 생성된 것을 확인할 수 있다. 

### frontmatter
frontmatter는 글의 맨 앞에 글에 대한 정보를 설명해주는 부분이다.
```markdown
---
title : '블로그 만들기 with Hugo & Github'
date : 2024-05-21T15:20:41+09:00
draft : true
authors : ["braveseokyung"]
description : Hugo로 블로그 만들고 Github Pages로 배포하기
categories : ["blog"]
series : ["hugo-blog"]
series_weight : 2
toc:
    auto: false
---
```

draft : false로 설정하면 commit 시에 글이 실제로 배포된다.

### hugo.toml로 글 설정 세팅하기
이대로 쭉 글을 쓰다 보면 Series가 적용이 안되고 TOC가 등장하지 않는데, 이 세팅들을 `hugo.toml`을 수정하여 바꿔줄 수 있다. 

blog/themes/DoIt/exampleSite에 들어가서 config/_default 파일을 보면 여러가지 .toml 파일이 있는 것을 확인할 수 있다. 이 .toml 파일들은 참고하여 hugo.toml 파일에 합쳤다. 이후 exampleSite처럼 blog/config/_default 폴더를 만들어서 hugo.toml 파일을 옮겨주었다.

지금까지 수정한 hugo.toml 파일은 아래와 같다. 블로그를 쓰면서 필요한 것이 있을 때마다 계속 업데이트할 예정이다.
```toml
baseURL = 'https://braveseokyung.github.io/'
languageCode = 'en-us'
title = 'brog'
theme ='DoIt'
#site default theme ("light", "dark", "black", "auto")
defaultTheme = "light"

[markup]
  [markup.goldmark]
    [markup.goldmark.extensions]
      [markup.goldmark.extensions.passthrough]
        enable = true
        [markup.goldmark.extensions.passthrough.delimiters]
          block = [['$$', '$$']]
          inline = [['$', '$']]

[params]
    [params.page]
        # whether to enable series navigation
        seriesNavigation=true

        [params.page.toc]
        # whether to enable the table of the contents
        enable = true

        # whether to keep the static table of the contents in front of the post
        keepStatic = false

        # whether to make the table of the contents in the sidebar automatically collapsed
        auto = true

        [params.page.code]
        # the maximum number of lines of displayed code by default
        maxShownLines = 15

        [params.page.math]
        enable = true
        blockLeftDelimiter = '$$'
        blockRightDelimiter = '$$'
        inlineLeftDelimiter = '$'
        inlineRightDelimiter = '$'
        copyTex = true
        mhchem = true

# Options for taxonomies
[taxonomies]
author = "authors"
category = "categories"
tag = "tags"
series = "series"
```

* [params.page.toc] toc enable
* [parmas.page.code] code block이 maxShownLines를 넘지 않으면 열려있고, 넘으면 토글로 닫혀있음.
* [taxonomies] series 폴더가 생성됨
* [markup], [params.page.math] latex(block-$$, inline-$)

### 첫 글 배포하기
draft : false로 바꾼 후,

```bash
hugo -D

# {계정이름}.github.io repository에 로컬 public 폴더 업로드
cd public
git add .
git commit -m "First commit"
git push origin main

# blog repository에 로컬 blog 폴더 업로드
cd ..
git add .
git commit -m "First commit"
git push origin master
```
## 끝!!
드디어 .io로 끝나는 블로그가 생기다니 밥 안먹어도 배부르다. 내새꾸 같은 블로그 열심히 써봐야지

## References
[Hugo 공식 Demo 블로그](https://hugodoit.pages.dev/theme-documentation-basics/#style-customization)

[Hugo 블로그 만들기 - GOODJIN](https://goodjin.kr/make-hugo-blog-in-local/)

[integerous.github.io](https://github.com/Integerous/Integerous.github.io?tab=readme-ov-file)
















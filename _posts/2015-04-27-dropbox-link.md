---
layout: post
title: "드롭박스(Dropbox)에서 링크 공유 방법"
comments: true
category: Skill
---

드롭박스를 사용하여 링크를 공유 떄 해당 파일 옆에 공유를 누르면 링크 주소가 뜬다.
`https://www.dropbox.com/s/5ha0aqrksqy64qe/one.png?dl=0`
이런 식으로 주소를 가르쳐 주는데 이 주소를 그대로 사용하면 이미지가 뜨지 않는다.
방법은 간단하다

1. `https://~`에서 `http://~` 로 바꾸어준다.

2. `www` 를 지우고 `dl`로 바꾸어준다.

3. 주소 마지막 매개변수 처리를 지워준다.

변경 전:
`https://www.dropbox.com/s/5ha0aqrksqy64qe/one.png?dl=0`

변경 후:
`http://dl.dropbox.com/s/5ha0aqrksqy64qe/one.png`

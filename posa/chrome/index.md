---
layout: bootstrap
title: 구글 크롬의 고성능 네트워킹
comment: YAML Front Matter
---
# 구글 크롬의 역사와 핵심 원칙2

구글 크롬(Chrome)은 2008년 하반기에 윈도우즈 플랫폼 베타 버전 형태로 처음 발표했다.
구글이 개발한 크롬은 자유로운 BSD 라이선스의 크로미엄(Chromium) 프로젝트로도 나와있다.
많은 사람들은 **브라우저 전쟁이 재현되지 않을까** 하며 크롬 발표를 놀랍게 받아들였다.
구글이 정말 더 잘 할 수 있을까?

> "크롬이 너무 좋아서 근본적으로 내 생각을 바꿀 수밖에 없었다..." - 에릭 슈미트,
> 구글 크롬 개발 계획을 [초기에는 주저했던][reluctance] 경험

[reluctance]: http://blogs.wsj.com/digits/2009/07/09/sun-valley-schmidt-didnt-want-to-build-chrome-initially-he-says/

<img src="http://1-ps.googleusercontent.com/h/www.igvita.com/posts/13/posa/xcomic-chrome.png.pagespeed.ic.UmplcgofTj.webp" style="float: left; margin-right: 1em;">
가능함을 증명했다.
오늘날 크롬은 웹에서 가장 많이 사용하는 (스탯카운터에 따르면 시장 점유율 [35% 이상][statcounter]) 브라우저 중 하나이고,
윈도우즈, 리눅스, OS X, 크롬 OS는 물론이고 안드로이드와 iOS 플랫폼을 지원한다.
분명하게 사용자은 기능에 공감했고, 크롬의 여러 혁신은 이미 다른 유명 브라우저에 들어가기도 했다.

[statcounter]: http://gs.statcounter.com/?PHPSESSID=oc1i9oue7por39rmhqq2eouoh0

예전 구글 크롬의 개념과 혁신을 설명한 [38페이지 만화책][comicbook]은 크롬의 구상과 설계 과정을 멋지게 들려주었다.
그러나 당시는 시작에 불과했다.
처음에 브라우저를 개발할 동기를 준 핵심 원칙은 계속된 향상을 인도한 원칙이 되었다.

[comicbook]: http://www.google.com/googlebooks/chrome/

* <strong style="color: green;">속도:</strong> **가장 빠른** 브라우저를 만든다는 목표
* <strong style="color: green;">보안:</strong> 사용자에게 **가장 안전한** 환경을 제공한다
* <strong style="color: green;">안정성:</strong> **탄력적이고 안정적인** 웹어플리케이션 플랫폼을 제공한다
* <strong style="color: green;">단순성:</strong> 세련된 기술을 **단순한 사용자 경험**으로 포장

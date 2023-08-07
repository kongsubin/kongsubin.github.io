---
layout: post
title: coil과 PhotoView 사용하기 
sitemap: false
categories: [study]
tags: [android]
description: >
  android coil을 사용하여 이미지 로딩 하기 & PhotoView를 이용하여 이미지 확대축소 하기 
---

### Paging의 구성 요소

![Paging의 구성 요소](https://developer.android.com/static/topic/libraries/architecture/images/paging3-library-architecture.svg?hl=ko)

  PagingSource
  - 페이지의 번호나 키로 API나 DB에서 데이터를 불러오는 추상클래스.

  PagingConfig
  - 페이지의 크기나 플레이스홀더의 사용 여부.

  Pager
  - PagingConfig에서 설정된 값으로 PagingSource를 호출하고 키를 관리함. 
  - 페이징된 데이터를 PagingData로 만들어 PagingDataAdapter에서 사용할 수 있도록 제공.

  PagingDataAdapter 
  - RecyclerView.Adapter를 상속해 데이터를 RecyclerView에 표시하고, 데이터의 로딩 상태를 노출함. 
  - 아이템을 비교하는 DiffUtill.ItemCallback을 생성자로 받아 데이터가 전달될 때 기존의 아이템과 비교하여 변경된 것을 어댑터에 알려줌. 

  RemoteMediator
  - API에서 데이터를 받아 로컬 DB에 캐시하는 경우 사용.
  - PagingSource가 로컬 DB에 캐시된 데이터를 Pager에 모두 제공했을때, API에서 데이터를 불러와 데이터베이스를 채우는 역할을 함. 

  LoadStateAdapter
  - 로드 상태에 따라 프로그래스바나 재시도버튼을 표시하기 위해 사용.



### Paging3 라이브러리를 사용하여 페이징 만들기. 

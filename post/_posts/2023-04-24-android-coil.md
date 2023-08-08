---
layout: post
title: coil과 PhotoView 사용하기 
sitemap: false
categories: [study]
tags: [android]
description: >
  android coil을 사용하여 이미지 로딩 하기 & PhotoView를 이용하여 이미지 확대축소 하기 
---


# Coil (Coroutine Image Loader)
android image loader 라이브러리이다. 
코루틴 기반으로 만들어져 코틀린 익스텐션을 이용하여 손쉽게 사용이 가능하다. 
> 구글의 Glide, 페이스북의 Fresco 


## 1. imageView의 확장함수 load를 사용하는 방법. 
> 간단하게 이미지 불러오기가 가능하며, 람다 함수를 이용해 옵션값을 지정할 수 있음. 

~~~kotlin
import coil.load

imageView.load("https://kongsubin.github.io/assets/img/logo.png") {
    placeholder(R.drawable.placeholder)
    transformations(CircleCropTransformation())
}
~~~

- placeholder : 이미지를 불러오는 동안 표시할 이미지 지정
- error : 로딩을 실패했을 때 대체할 이미지를 지정
- transformations : 이미지를 변형 

## 2. load함수를 사용하지 않고, 로더 객체를 사용하는 방법 
일반적으로 하나의 앱에서 설정이 다른 이미지 로더가 필요한 경우는 잘 없다. 
따라서 싱글톤으로 관리하는 것이 일반적이다. 

~~~kotlin
val imageLoader = Coil.imageLoader(context)
val imageLoader2 = context.imageLoader

Coil.setImageLoader(imageLoader)
~~~

~~~kotlin
val imageLoader = ImageLoader.Builder(context)
    .availableMemoryPercentage(0.2)
    .bitmapConfig(Bitmap.Config.RGB_565)
    .crossfade(true)
    .okHttpClient {
        OkHttpClient.Builder()
            .cache(CoilUtils.createDefaultCache(context))
            .build()
    }
    .build()

val request = ImageRequest.Builder(context)
    .data("https://kongsubin.github.io/assets/img/logo.png")
    .target(imageView)
    .build()

imageLoader.enqueue(request)

~~~


# PhotoView
> 두 손가락으로 이미지를 확대하거나 축소하는 기능을 제공하는 오픈소스 프로젝트 

## 사용방법

### 1. settings.gradle 설정
> PhotoView를 받아 올 수 있는 저장소 추가 

~~~kotlin
...

dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        google()
        mavenCentral()
        maven { url "https://jitpack.io" } // PhotoView 를 받아올 수 있는 저장소
    }
}

...
~~~

### 2. build.gradle 의존성 추가 
~~~.gradle
dependencies {
    ...
    implementation 'com.github.chrisbanes:PhotoView:2.3.0'
    ...
}
~~~

### 3. 사용하기
xml 
~~~xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/black">

    <com.github.chrisbanes.photoview.PhotoView
        android:id="@+id/image"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />


</androidx.constraintlayout.widget.ConstraintLayout>
~~~

kt
~~~kotlin
class ImageViewerActivity : AppCompatActivity() {
    companion object {
        const val EXTRA_URL = "url"
    }

    lateinit var binding: ActivityImageViewerBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        binding = ActivityImageViewerBinding.inflate(layoutInflater)
        setContentView(binding.root)

        supportActionBar?.setHomeAsUpIndicator(R.drawable.ic_close)
        supportActionBar?.setDisplayHomeAsUpEnabled(true)

        val url = intent.getStringExtra(EXTRA_URL)
        binding.image.load(url)
    }
}
~~~
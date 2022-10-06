---
layout: post
title: Kotlin에서 알아야할 함수들.
sitemap: false
categories: [study]
tags: [kotlin]
---

# 코틀린에서 알아야할 함수 6가지.

# 1. apply()
> apply() 함수는 block으로 정의된 구간을 수행하고 appply를 사용ㅇ한 객체를 다시 반환한다. 

주로 리턴값으로 인자를 넘겨주기 때문에 함수를 호출한 변수에서 속성값을 지정해서 사용할 때 편리하다. 

### apply() 함수 예제 
> 캘린더 뷰에 date가 바뀐 것을 감지하여, 캘린더 객체에 속성값을 설정해준다. 
~~~kotlin
    val calendar = Calendar.getInstance()

    binding.calendarView.setOnDateChangeListener { _, year, month, dayOfMonth ->
        calendar.apply {
            set(Calendar.YEAR, year)
            set(Calendar.MONTH, month)
            set(Calendar.DAY_OF_MONTH, dayOfMonth)
        }
    }
~~~

<br>
<br>
<br>

# 2. run() 
> run 함수는 객체에서 호출할 수 있는 방법, run 함수 자체로 홀로 사용할 수 있는 방법으로 총 2가지 형태로 사용할 수 있다. 
> 두가지 경우 모두 block 코드에서 수행한 결과값을 리턴한다. 


### run() 함수 예제 
~~~kotlin
    convertView?.run {
        findViewById(R.id.add).setOnClickListener{
            Toast.makeText(context, "Clicked", Toast.LENGTH_SHORT).show()
        }
    }
~~~

<br>
<br>
<br>

# 3. let() 
> let 함수는 객체를 통해서만 실행이 가능하다. run과 유사한 기능이지만, let 함수는 this로 넘겨주어 it을 통해 객체의 변수에 접근할 수 있다. 


### let() 함수 예제 1
~~~kotlin
    convertView?.let {
        it.findViewById(R.id.add).setOnClickListener{
            Toast.makeText(context, "Clicked", Toast.LENGTH_SHORT).show()
        }
    }
~~~
> 이름을 따로 주는 경우
~~~kotlin
    convertView?.let {view ->
        view.findViewById(R.id.add).setOnClickListener{
            Toast.makeText(context, "Clicked", Toast.LENGTH_SHORT).show()
        }
    }
~~~

### let() 함수 예제 2
> selectedTodo는 Todo라는 data class의 list이다. let 함수를 통해 객체의 값을 화면에 바인딩해주고 있다.  
~~~kotlin
    viewModel.selectedTodo?.let {
        binding.todoEditText.setText(it.title)
        binding.calendarView.date = it.date
    }
~~~

<br>
<br>
<br>

# 4. with()
> 넘기려는 객체를 괄호 안에 넣고, {}를 수행한다. 


### with() 함수 예제
~~~kotlin
with(convertView){
    findViewById(R.id.add).setOnClickListener {
        Toast.makeText(context, "Clicked, Toast.LENGTH_SHORT).show()
    }
}
~~~ 
> 인자로 객체를 넘겨주므로, 내부에서 객체의 null값을 체크하는 부분이 들어가야한다. 

<br>
<br>
<br>

# 5. forEach()
> forEach 함수는 for문과 같다. 

### forEach() 함수 예제
> listOf에 저장되어 있는 값들을 받을 때는 it을 이용하여 받을 수 있다. 
~~~kotlin
listOf<String>("mike", "jim", "harry").forEach {
    Toast.makeText(this, "find $it", Toast.LENGTH_SHORT).show()
}
~~~
<br>
<br>
<br>

# 6. onEach()
> forEach와 비슷하지만 onEach는 {}안에 취한 행동의 결과값을 넘겨줌. 


### onEach() 함수 예제 
> 나이가 30대인 사람에 대해서만 Toast메시지를 띄워주고 최종적으로 30대 이상인 사람의 size를 구한다. 
~~~kotlin
    fun getListSize(list:Array<Person>):Int = list.filter { it.age >= 30}
        .onEach { Toast.makeText(this, "Hello ${it.name}", Toast.LENGTH_SHORT).show() }.size
~~~

<br>
<br>
<br>

# 7. filter()
> 콜렉션에서 필터에 선언한 부분만 발췌하여 결과값을 넘겨준다. filter함수의 구문은 Boolean으로 결정할 수 있는 구문이어야함. 

### filter() 함수 예제
~~~kotlin
fun addOdd():Int {
    var result = 0;
    (1..50).filter{ (it%2-1)==0 }.forEach { result+=it }
    return result
}
~~~


<br>
<br>
<br>

참고 서적
- 핵심 문법과 예제로 배우는 코틀린 (이난주 지음)
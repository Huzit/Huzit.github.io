---
layout: post 
title: RecyclerViewItemClickEvent
subtitle: RecyclerViewItemClickEvent
categories: Android
tags: [android]
---
## 1. 리사이클러뷰(RecyclerView) 아이템 클릭
이전 글에선 리사이클러뷰를 기본 사용법과 예제를 살펴봤습니다. 이제 리사이클러뷰 아이템 클릭 이벤트를 처리하는 방법을 알아보겠습니다.

리스트뷰를 써본 경험이 있다면 리스트뷰의 아이템 클릭을 <span style=color:#20C997>setOnItemClickListener()</span>를 통해 아이템 클릭 이벤트를 처리해보셨을 겁니다.

그래서 리사이클러뷰에서도 리스트뷰와 마찬가지로 setOnItemClickListener()와 유사한 방법을 통해 아이템 클릭이벤트를 설정할 수 있을거 같지만 **리사이클러뷰에선 기본적으로 제공해주는 아이템 클릭 이벤트 메소드가 없습니다.**

그렇다면 리사이클러뷰에서 아이템 클릭 이벤트를 처리하는 방법은 없는걸까요?

## 2. 아이템 클릭 처리주체
리스트뷰는 유사한 형태와 크기의 아이템뷰를 한 줄로 나열합니다. 이러한 단순함으로 인해 아이템 뷰의 위치파악하기 비교적 간단합니다.

하지만 리사이클러뷰는, 리스트뷰에 비해 훨씬 유연하고 다양한 형태로 아이템을 표시하게 만들어줍니다. <br>레이아웃 매니저를 통해 아이템을 다양하게 배치할 수 있고, 애니메이션 효과를 통해 다이나믹한 화면을 구성할 수 있게 해줍니다. 하지만 이런 장점이 이벤트 클릭처리를 복잡하게 만드는 요인이 됩니다. 그래서 리사이클러뷰는 아이템 클릭이벤트 리스너를 직접 다루지않고 <span style=color:#20C997>**아이템 뷰에서 OnClickListener를 통해 처리**하게 만들었습니다. </span>

## 3. 구현 핵심 개념
>아이템 뷰에서 OnClickListener를 통해 처리하게 만들었습니다.

위 문장을 보고 코드에서 어떻게 구현할 지 바로 떠올린다면 당신은 천재입니다.

이전에 작성했던 [리사이클러뷰란?](https://velog.io/@appletorch/RecyclerView%EB%9E%80) 내용을 다시 한 번 보겠습니다.
>정리하자면 어댑터를 통해 만들어진 각 아이템뷰는 뷰홀더객체에 저장되어 화면에 표시되고, 필요에 따라 생성 또는 재활용 됩니다.

이제 머릿속에 어느정도 코드의 형태가 그려질 것입니다.
* 아이템뷰에서 클릭 이벤트 처리
* 아이템뷰는 뷰홀더에 저장
* <span style=color:#20C997>뷰홀터에서 클릭 이벤트 작성</span>

따라서 아래 코드처럼 뷰 홀더가 만들어지는 시점에 클릭이벤트를 처리하면 됩니다.
```kotlin
    inner class SearchMainViewHolder(view: View) : RecyclerView.ViewHolder(view) {


        init {
            view.setOnClickListener {
            //TODO 코드 작성
            }
        }
    }
```

## 4. 아이템 위치 알아내기
위 내용에서 우린 클릭 이벤트를 처리할 수 있게 되었습니다. 다음에 할 일은 현재 클릭 이벤트가 발생된 <span style=color:#20C997>아이템의 위치</span>를 알아내는 것입니다. 보통 리사이클러뷰에서 클릭 이벤트를 사용할 때에는 해당 아이템뷰와 연결된 데이터를 확인, 수정, 삭제 및 선택된 아이템에 따라 다른 기능을 수행하기 위해 사용합니다. <span style=color:#20C997>**따라서 클릭 이벤트 구현시 가장 먼저 해야할 일은 이벤트가 발생한 아이템의 위치를 알아내는 것입니다.** </span>

위치정보를 알아내는 것은 매우 쉽습니다.
classes.jar > androidx > recyclerview > widget > RecyclerView.java > ViewHolder에 보면
```java
public final int getAdapterPosition() {
    if (mOwnerRecyclerView == null) {
        return NO_POSITION;
    }
    return mOwnerRecyclerView.getAdapterPositionFor(this);
}
```
리사이클러뷰 <span style=color:#20C997>자체적으로 위치를 확인할 수 있는 메서드를 제공</span>하는 것을 알 수 있습니다.
따라서 우리는 위 코드에 대해 적절한 검사후 사용하면 됩니다.


```kotlin
    inner class SearchMainViewHolder(view: View) : RecyclerView.ViewHolder(view) {

        init {
            view.setOnClickListener {
              val pos = adapterPosition
              if(pos != RecyclerView.NO_POSITION){
                  //TODO
              }
            }
        }
    }
```

## 5. 아이템 위치로 데이터 리스트 접근하기
클릭 이벤트 발생위치를 알게됬으니 데이터를 가져오는 것은 간단합니다.
```kotlin
    inner class SearchMainViewHolder(view: View) : RecyclerView.ViewHolder(view) {

        init {
            view.setOnClickListener {
              val pos = adapterPosition
                if(pos != RecyclerView.NO_POSITION){
                    val item = dataset.get(pos)
                }
            }
        }
    }
```

## 6. 리사이클러뷰 외부에서 아이템 클릭 이벤트 처리하기
위의 경우 어댑터 내부에서만 사용가능한 방법입니다. 위 경우외에 액티비티, 프래그먼트에서 아이템 클릭 이벤트를 처리해야하는 경우가 있습니다.

이런경우 가장 쉽게 구현할 수 있는 방법은, <span style=color:#20C997>우리가 직접 커스텀 리스너를 만들어 주는 것</span>입니다.
우선 **1. 어댑터에 직접 리스너 인터페이스를 정의**한 다음, **2. 호출할 곳에서 해당 리스너 객체를 생성**하고 **3. 어댑터에 전달하여 호출되도록 만드는 것**입니다. <span style=color:#20C997>자식(여기선 어댑터)이 부모(액티비티, 프래그먼트)의 이벤트 핸들러를 호출하는 것입니다.</span>
![출처 : https://recipes4dev.tistory.com/168](https://images.velog.io/images/appletorch/post/4b1ad57a-0ba2-46aa-9937-6d780123b943/image.png)
### 1. 커스텀 리스너 인터페이스 정의
가장 먼저 해야할 일은 어댑터내부에 새로운 리스너를 정의하는 것입니다. 리스너에서 선언되는 메서드의 이름과 파라미터의 형식은 필요에 따라 정하면 됩니다.
```kotlin
class SearchCurrentAdapter(private val dataset: ArrayList<SearchData>) 
: RecyclerView.Adapter<SearchCurrentAdapter.SearchMainViewHolder>() {
    //커스텀 리스너
    interface OnItemClickListener{
        fun onItemClick(view: View, position: Int)
    }
    ...
}
```

### 2. 리스너 객체 전달 메서드와 저장할 변수 추가
외부의 객체참조를 어댑터에 전달하는 매서드와 객체 참조를 저장하는 변수도 추가합니다.
```kotlin
class SearchCurrentAdapter(private val dataset: ArrayList<SearchData>) 
: RecyclerView.Adapter<SearchCurrentAdapter.SearchMainViewHolder>() {
    //커스텀 리스너
    interface OnItemClickListener{
        fun onItemClick(view: View, position: Int)
    }
    //객체 저장 변수
    private lateinit var mOnItemClickListener: OnItemClickListener
    //객체 전달 메서드
    fun setOnItemClickListener(onItemClickListener: OnItemClickListener){
        mOnItemClickListener = onItemClickListener
    }
    ...
}
```
### 3. 아이템 클릭 핸들러 메서드에서 리스너 객체 메서드 호출
어댑터 뷰홀더에서 아이템 클릭시 우리가 만든 커스텀 리스너의 이벤트 메서드를 호출하는 코드를 작성합니다.
```kotlin
    inner class SearchMainViewHolder(view: View) : RecyclerView.ViewHolder(view) {

        init {
            view.setOnClickListener {
              val pos = adapterPosition
                if(pos != RecyclerView.NO_POSITION && mOnItemClickListener != null){
                    mOnItemClickListener.onItemClick(view, pos)
                }
            }
        }
    }
```

### 4. 액티비티(또는 프래그먼트)에서 커스텀 리스너 객체생성 및 전달
마지막으로 액티비티 또는 프래그먼트에서 커스텀 이벤트 리스너 객체를 생성하여 어댑터에 전달합니다.
```kotlin
class SearchCurrentActivity : AppCompatActivity() {
    companion object{
        val dataset = ArrayList<SearchData>()
    }
    private lateinit var binding: SearchActivityCurrentBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = SearchActivityCurrentBinding.inflate(layoutInflater)
        setContentView(binding.root)

        setRecycler()
    }

    private fun setRecycler(){
    	//리사이클러뷰 바인딩
        val mRecyclerView = binding.searchRecyclerCurrent
        val mSearchData = SearchData()
        //데이터 리스트에 추가
        dataset.add(mSearchData)

        val intent = Intent(applicationContext, SearchDetailActivity::class.java)
        //어댑터 생성
        val adapter = SearchCurrentAdapter(dataset)
	//클릭이벤트 호출<<<
        adapter.setOnItemClickListener(object: SearchCurrentAdapter.OnItemClickListener{
            override fun onItemClick(view: View, position: Int) {
                intent.putExtra("searchData", dataset.get(position))
                startActivity(intent)
            }
        })
	//어댑터 설정
        mRecyclerView.adapter = adapter
        mRecyclerView.layoutManager = LinearLayoutManager(applicationContext)
    }
}
```
#### adapter 코드 전문
```kotlin
class SearchCurrentAdapter(private val dataset: ArrayList<SearchData>) : RecyclerView.Adapter<SearchCurrentAdapter.SearchMainViewHolder>() {
    //커스텀 리스너
    interface OnItemClickListener{
        fun onItemClick(view: View, position: Int)
    }
    //객체 저장 변수
    private lateinit var mOnItemClickListener: OnItemClickListener
	//객체 전달 메서드
    fun setOnItemClickListener(onItemClickListener: OnItemClickListener){
        mOnItemClickListener = onItemClickListener
    }

    inner class SearchMainViewHolder(view: View) : RecyclerView.ViewHolder(view) {

        init {
            view.setOnClickListener {
                val pos = adapterPosition

                if(pos != RecyclerView.NO_POSITION && mOnItemClickListener != null) {
                    
                    mOnItemClickListener.onItemClick(view, pos)
                }
            }
        }
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): SearchMainViewHolder {
	...
    }


    override fun onBindViewHolder(holder: SearchMainViewHolder, position: Int) {
        ...
    }

    override fun getItemCount(): Int = dataset.size
}
```
### 결론
위의 방법을 이용하면 간편하게 리사이클러뷰 아이템 클릭 이벤트를 만들 수 있습니다.
## 출처
[리사이클러뷰 클릭 이벤트](https://recipes4dev.tistory.com/168)

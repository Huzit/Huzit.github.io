---
layout: post 
title: RecyclerView
subtitle: RecyclerView
categories: Android
tags: [android]
---
## RecyclerView란?

### 1. 정의
[안드로이드 공식문서](https://developer.android.com/reference/androidx/recyclerview/widget/RecyclerView)에는 <span style="color:#FC4C68"> A flexible view for providing a limited window into a large data set.</span> 로 소개하고 있습니다. 번역하면 **'대규모 데이터셋을 제한된 범위에 제공하기 위한 유연한 뷰'** 입니다.
조금 이해를 돕기위해 설명을 덧붙이자면
> 사용자가 관리하는 <span style="color:red">데이터 셋</span>을 아이템 단위로 구성하여 화면에 출력하는 <span style="color:red">뷰 그룹</span>으로 제한된 화면에 제공하기위해 스크롤 가능한 <span style="color:red">리스트</span>로 표시해주는 위젯'이라 말할 수 있습니다.

이름으로 넘어가서 왜 리사이클러 뷰(RecyclerView)라고 정했을까요?
그 이유는 리스트 뷰(ListView)와 다르게 매번 리스트 항목이 갱신될 때마다 <span style="color:#FC4C68">기존에 사용했던 아이템 뷰를 재활용</span>하기 때문입니다. 그리고 이를 위해 기본적으로 **뷰홀더(ViewHolder)패턴**을 사용하도록 만들어놨습니다.


### 2. 장/단점
이러한 리사이클러 뷰(RecyclerView)의 최대 장점은 <span style="color:#FC4C68">"유연함"</span>입니다.
> 리사이클러뷰(RecyclerView)는 아이템을 표시하기위해 뷰홀더(ViewHolder)를 필수적으로 생성하도록 되어있습니다. 
> 이는 단순 리사이클러뷰(RecyclerView)에 포함된 요소임을 넘어 개발자가 직접 뷰홀더(ViewHolder) 패턴을 적용할 때 고민해야 했던 여러 이슈들이 구현사항에 포함되었다는 것을 의미합니다.

쉬운 예로, 리스트뷰에서 화면의 나열 방향을 수직에서 수평으로 바꾸려면 리스트뷰가 아닌 다른 뷰를 사용하거나, 리스트뷰 기능을 상당부분 재 구현 해야합니다. 
또한 동적으로 구성하기에 한계도 있습니다. 만약 사용자의 선택에 따라 뷰를 새로운 형태로 바꾸고자 한다면 어댑터 내에서 개발자가 직접 처리해야합니다. 
하지만 리사이클러뷰는 이러한 단점을 보완하여 개발자가 쉽게 구현할 수 있도록 만들어줍니다. <span style="color:#FC4C68">구현요소 또는 구현에 따른 결과물이 **쉽게 변경되거나 확장** 될 수 있음을 의미합니다.</span>


### 3. 구성요소
#### 1. RecyclerView
![](https://images.velog.io/images/appletorch/post/4182aeaf-6e90-4d90-9591-c4cbc3406a3b/image.png)
리사이클러뷰는 v7 지원 라이브러리에서 제공되는 위젯으로 사용자의 데이터를 리스트 형태로 화면에 표시하는 컨테이너 역할입니다.

#### 2. Adapter
![](https://images.velog.io/images/appletorch/post/1cdeab6b-1bc3-4007-8cc1-399383e6bb3a/image.png)
사용자의 데이터 리스트로 부터 아이템뷰를 만드는 역할입니다.

#### 3. LayoutManager
![](https://images.velog.io/images/appletorch/post/f5e30216-5ee1-4515-9818-1edd385ea1b0/image.png)
리사이클러뷰가 아이템을 화면에 표시할 때 배치되는 형태를 관리하는 요소입니다.
배치 형태는 다음과 같습니다.
* LinearLayoutManager : 수직(vertical)또는 수평(horizontal) 방향으로 아이템뷰 배치
* GridLayoutManager : 비율이 같은 격자(Grid) 형태로 배치
* StaggeredGridLayoutManager : 엇갈린(Staggered) 격자(Grid)형태로 배치
  ![](https://images.velog.io/images/appletorch/post/39138283-3a52-4d8f-b93d-be1787a7864b/image.png)

#### 4. ViewHolder
![](https://images.velog.io/images/appletorch/post/2c8b7e72-d0f3-4fc8-90fa-0fb3d1569074/image.png)
화면에 표시될 아이템뷰를 저장하는 객체입니다. 어댑터에 의해 관리되고 필요에 따라 어댑터에서 생성됩니다.
>정리하자면 어댑터를 통해 만들어진 각 아이템뷰는 뷰홀더객체에 저장되어 화면에 표시되고, 필요에 따라 생성 또는 재활용 됩니다.

### 4. 구현순서
** <span style="color:#FC4C68">리사이클러뷰 추가 -> 아이템뷰(뷰 홀더) 레이아웃 추가 -> 어댑터 구현 -> 리사이클러뷰에서 어댑터, 레이아웃 매니저 지정</span> **

-----------
## RecyclerView 구현
### 1. RecyclerViewLaout 구현
레이아웃은 간단합니다. 원하는 곳에 구현해주기만 하면 됩니다.
```xml
        <androidx.recyclerview.widget.RecyclerView
                android:id="@+id/search_recycler_main"
                android:src="@drawable/baseline_home_24"
                android:layout_width="match_parent"
                android:layout_height="match_parent"/>
```
### 2. ViewHolderLayout 구현
재사용될 레이아웃입니다.
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout 
                                                   xmlns:android="http://schemas.android.com/apk/res/android"
                                                   xmlns:app="http://schemas.android.com/apk/res-auto"
                                                   xmlns:tools="http://schemas.android.com/tools"
                                                   android:layout_width="match_parent"
                                                   android:layout_height="120dp">

    <TextView
            android:text="10:12 오전 10:30"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/search_datetime"
            app:layout_constraintTop_toTopOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            android:layout_marginLeft="@dimen/pageMargin"
            android:layout_marginStart="@dimen/pageMargin"
            android:layout_marginTop="@dimen/pageMarginHalf"/>

    <TextView
            android:text="냉장고"
            android:textSize="@dimen/font_size"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/search_product"
            app:layout_constraintTop_toBottomOf="@+id/search_datetime"
            app:layout_constraintStart_toStartOf="parent"
            android:layout_marginLeft="@dimen/pageMargin"
            android:layout_marginStart="@dimen/pageMargin"
            android:textColor="#000000"/>

    <TextView
            android:text="얼음을 꺼냈는데 얼어있어요\n냉장고에 물을 넣었더니 안따뜻해요"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/search_textarea"
            app:layout_constraintTop_toBottomOf="@+id/search_product"
            app:layout_constraintStart_toStartOf="parent"
            android:layout_marginLeft="@dimen/pageMargin"
            android:layout_marginStart="@dimen/pageMargin"/>

    <TextView
            android:id="@+id/search_process"
            android:text="@string/ongoing"
            android:layout_width="@dimen/recyclerTag"
            android:layout_height="wrap_content"
            app:layout_constraintEnd_toStartOf="@+id/search_imageButton"
            android:gravity="center"
            android:textColor="@color/white"
            android:background="@drawable/button_shape"
            android:backgroundTint="@color/blue"
            app:layout_constraintTop_toTopOf="parent"
            android:layout_marginTop="@dimen/pageMarginHalf"
            android:layout_marginRight="@dimen/pageMargin"
            android:layout_marginEnd="@dimen/pageMargin"/>

    <ImageButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:background="@drawable/baseline_arrow_forward_ios_24"
            android:id="@+id/search_imageButton"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintTop_toTopOf="parent"
            android:layout_marginRight="@dimen/pageMargin"
            android:layout_marginEnd="@dimen/pageMargin"
            android:layout_marginTop="@dimen/pageMarginHalf"/>
</androidx.constraintlayout.widget.ConstraintLayout>
```
![](https://images.velog.io/images/appletorch/post/41b15f01-c6b2-4515-afd0-ec4692279859/image.png)
### 3. Adapter 구현
구현 단계에서 가장 중요한 Adapter입니다. Adapter에선 특별한 경우를 제외하고 ViewHolder를 inner Class로 구현하게됩니다.

#### 전반적인 코드 형태
```kotlin
package com.example.user_client.search

import android.view.ViewGroup
import androidx.recyclerview.widget.RecyclerView


class SearchMainAdapter(private val dataset: ArrayList<_DataType_>) : RecyclerView.Adapter<SearchMainAdapter.SearchMainViewHolder>() {

    inner class SearchMainViewHolder(view: View) : RecyclerView.ViewHolder(view){
        
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): SearchMainViewHolder {
        TODO("Not yet implemented")
    }

    override fun onBindViewHolder(holder: SearchMainViewHolder, position: Int) {
        TODO("Not yet implemented")
    }

    override fun getItemCount(): Int {
        TODO("Not yet implemented")
    }
}
```
기본적인 Adapter틀입니다. RecyclerView.Adapter를 implement받아줍니다. inner class로 뷰홀더를 만들어서 RecyclerView.ViewHolder()를 상속받고 필수 구현해야하는 함수들을 오버라이드 해줍니다.

```kotlin
data class SearchMain(
    val dateTime : String = "0",
    val product : String = "펭귄",
    val textArea : String = "테스트",
    val process :String = "진행중?",
    val imageButtonColor : Int = R.color.fuckingblue
)
```
데이터를 아이템뷰에 넣어줄 때 사용할 데이터 클래스도 작성해줍니다.
주요 함수의 기능
>* onCreateViewHolder : 해당 뷰그룹의 LayoutInflater로 inflate한 뷰홀더 레이아웃을 뷰홀더에 넣어준다
>* SearchMainViewHolder : 아이템뷰 객체 생성
>* onBindViewHolder : 뷰홀더에 데이터를 넣어준다
>* getItemCount  : 아이템뷰 갯수를 반환

#### 1. SearchMainViewHolder

```kotlin
    inner class SearchMainViewHolder(view: View) : RecyclerView.ViewHolder(view){
    //아이템뷰 객체 생성
        val dateTime = view.findViewById<TextView>(R.id.search_datetime)
        val product = view.findViewById<TextView>(R.id.search_product)
        val textArea = view.findViewById<TextView>(R.id.search_textarea)
        val process = view.findViewById<TextView>(R.id.search_process)
        val imageButton = view.findViewById<ImageButton>(R.id.search_imageButton)
    }
```
#### 2. onCreateViewHolder
```kotlin
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): SearchMainViewHolder {
    //구현한 뷰홀더 레이아웃을 객체형태로 변환(inflate)하고 반환해준다.
        val view = LayoutInflater.from(parent.context).inflate(R.layout.search_viewgroup, parent, false)
        return SearchMainViewHolder(view)
    }
```

#### 3. onBindViewHolder

```kotlin
    override fun onBindViewHolder(holder: SearchMainViewHolder, position: Int) {
    //사용자 데이터를 아이템뷰 객체에 바인딩 해준다.
        holder.dateTime.text = dataset[position].dateTime
        holder.product.text = dataset[position].product
        holder.textArea.text = dataset[position].textArea
        holder.process.text = dataset[position].process
    }
```

#### 4. getItemCount()
```kotlin
override fun getItemCount(): Int = dataset.size
```
### 4. RecyclerView에서 Adapter, LayoutManager호출
```kotlin
class SearchFragment : Fragment() {
    companion object{
        val dataset = ArrayList<SearchMain>()
    }
    private var binding : SearchFragmentMainBinding? = null
    private val view get() = binding!!

    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        binding = SearchFragmentMainBinding.inflate(inflater, container, false)

        //recyclerView 호출
        val mRecyclerView = view.searchRecyclerMain
        //사용자 데이터 생성(default)
        val mSearchMain = SearchMain()
        dataset.add(mSearchMain)
        //어댑터 설정
        mRecyclerView.adapter = SearchMainAdapter(dataset)
        //layoutManager 설정
        mRecyclerView.layoutManager = LinearLayoutManager(container!!.context)

        return view.root
    }

    override fun onDestroyView() {
        binding = null
        super.onDestroyView()
    }
}
```
## 코드 전문
```kotlin
class SearchAdapter(private val dataset: ArrayList<SearchData>) : RecyclerView.Adapter<SearchAdapter.SearchDataViewHolder>() {

    inner class SearchDataViewHolder(view: View) : RecyclerView.ViewHolder(view){
        val dateTime = view.findViewById<TextView>(R.id.search_datetime)
        val product = view.findViewById<TextView>(R.id.search_product)
        val textArea = view.findViewById<TextView>(R.id.search_textarea)
        val process = view.findViewById<TextView>(R.id.search_process)
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): SearchDataViewHolder {
        val view = LayoutInflater.from(parent.context).inflate(R.layout.search_viewgroup, parent, false)
        return SearchDataViewHolder(view)
    }

    override fun onBindViewHolder(holder: SearchDataViewHolder, position: Int) {
        var process = dataset[position].process
        holder.dateTime.text = dataset[position].dateTime
        holder.product.text = dataset[position].product
        holder.textArea.text = dataset[position].productInfo
        holder.process.text = process
        //진행상황별 라벨색 변경
        when(process){
            "진행중" -> holder.process.setBackgroundResource(R.drawable.label_blue)
            "입고완료", "상세보기" -> holder.process.setBackgroundResource(R.drawable.label_green)
            "예약대기" -> holder.process.setBackgroundResource(R.drawable.label_red)
        }
    }

    override fun getItemCount(): Int = dataset.size
}
```
## 결과
![](https://images.velog.io/images/appletorch/post/9306f6dc-0162-4f56-aa01-446aa5b7dbdd/KakaoTalk_20211017_051511087.gif)
## 참고
https://parkho79.tistory.com/152   
[리사이클러 뷰 기본](https://recipes4dev.tistory.com/154)   
[리사이클러 뷰 클릭 이벤트](https://recipes4dev.tistory.com/168?category=790402)   

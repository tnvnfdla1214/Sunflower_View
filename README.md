# Sunflower_View

## GardenActivity
먼저 SPA(Single-Page-Application) 구조인 이 프로젝트에서 유일한 액티비티인 GardenActivity 입니다.

1. 네비게이션 Host로 프래그먼트의 껍데기 역할을 담당해서 별다른 로직은 없습니다.
2. XML에서는 Jetpack Navigation을 활용하기 위한 옵션들을 갖고있고 FragmetContainerView 레이아웃으로 프래그먼트를 담게 됩니다.
## HomeViewPagerFragment
![image](https://user-images.githubusercontent.com/48902047/149520731-8e2db08d-1b22-4399-b84d-0b78f68fe59f.png)

Toolbar + ViewPager2 + TabLayout 구조로 되어 있는 HomeViewPagerFragment 입니다.

1. XML은 \<layout>으로 데이터바인딩 처리 되어있고 프래그먼트에서는 FragemntViewPagerBinding.infalte()로 infalte 하는 것을 볼 수 있습니다.
2. TabLayout 과 ViewPager가 연결되고 ViewPager에는 어댑터가 세팅됩니다.
3. 데이터바인딩과 초기화 작업을 한후 데이터바인딩 클래스에는 상응하는 레이아웃 파일의 루트 뷰에 관한 직접 참조를 제공하는 binding.root 를 반환해줍니다.
4. TabLayoutMediator 라는 클래스는 [ViewPager2](https://developer.android.com/jetpack/androidx/releases/viewpager2?hl=ko)로 1버전과 비교하여 에 세로방향 지원, DiffUtill 적용 등 여러가지 이점과 특징이 있는데  그 중 하나로 TabLayout 객체와 함께 사용할 수 있다는 점입니다.
![image](https://user-images.githubusercontent.com/48902047/149522636-3fb1aee3-f816-4dca-b7ea-65e8ee529808.png)

## SunflowerPagerAdapter
![image](https://user-images.githubusercontent.com/48902047/149522456-7ff30d43-9ec4-47f2-8552-0ceb556b46ca.png)

ViewPager2 + Fragment 단위로 이루어진 어댑터이므로 FragmentStateAdapter 를 상속받는 것을 볼 수 있습니다. ViewPager1 에서 사용하던 FragmentStatePagerAdapter는 deprecated 되고 FragmentStateAdapter 로 대체되었습니다.
## PlantListFragment
![image](https://user-images.githubusercontent.com/48902047/149523151-53a4f3a2-b3a1-45a5-bea5-db1f65246d89.png)

식물들의 리스트를 보여주는 프래그먼트입니다. 리스트다 보니 리사이클러뷰와 어댑터를 세팅하는 코드가 있습니다.

1. 오른쪽 상단의 메뉴를 만드는 onCreateOptionsMenu() 함수도 오버라이드 되어있는 것을 볼 수 있고 이 메뉴를 클릭 시 onOptionsItemSelected() 에 의해 데이터가 업데이트 되는 로직이 실행됩니다.
2. 이어서 이 프래그먼트에서 사용한 어댑터를 살펴보면 ListAdapter를 상속받아 Diffutil을 적용하였습니다. 이에 대해 간략하게 설명하면,  요약하면 안드로이드 어댑터에서 현재 데이터 리스트와 교체될 데이터 리스트를 비교하여 무엇이 다른지(바꼈는지) 알아내는 클래스로 리사이클러뷰에서 기존 아이템리스트에 수정 혹은 변경이 있을 시 전체를 갈아치우는게 아니라 변경되야하는 데이터만 빠르게 바꿔주는 역할을 합니다. 또한 기존 일반적인 어댑터와 다르게 submitList() 라는 함수로 아이템을 추가하는 것도 특징입니다.

### PlantAdapter

1. 먼저 아이템 XML이 데이터바인딩 처리되고 값이 세팅되는 것을 볼 수 있습니다.
2. navigateToPlant() 함수를 보면 식물아이템 클릭시 Jetpack Navigation을 사용해 DeatailFragment 로 식물 고유 ID 와 함께 네비게이션 됨을 볼 수 있습니다.
3. ListAdapter 를 상속받고 DiffutilCallback을 설정하여 기존 아이템 리스트에 변경이 있을 경우 달라진 아이템일 경우에만 빠르게 수정해줄 수 있게 구현이 되어 있습니다.
4. 기존의 어댑터처럼 itemList 변수나 add(), notifyDatachanger() 와 같은 함수를 따로 만들 필요없고 자체적으로 구현이 되어 있습니다.

## PlantDetailFragment
![image](https://user-images.githubusercontent.com/48902047/149524417-845bfbae-ff8d-46d4-b3fa-82d2d5024d2f.png)
식물 리스트의 아이템 클릭 시 띄어지는 PlantDetailFragment 입니다. 말 그대로 식물의 상세정보를 제공해줍니다.

1 private val args: PlantDetailFragmentArgs by navArgs() 을 보면 알 수 있듯이 Jetpack Navgation의 파라미터 인자값을 전달받는 기능을 사용하고 있습니다. 여기서는 식물의 고유 ID 값을 전달 받습니다.
2 데이터바인딩 처리가 되어있습니다.
3. 스크롤에 따라 화면의 툴바(앱바)가 접혀지고 펴지는 애니메이션을 가진 Collapsing Toolbar Layout 이 사용되었습니다.  XML을 보면
```Kotlin
CoordinatorLayout

   -AppBarLayout
   
       --CollapsingToolbarLayout
       
            ---ImageView, Toolbar
            
    -NestedScrollView
    
        --ConstraintLayout
        
    -FloatingActionButton
```
와 같은 구조로 되어있는 것을 볼 수 있습니다.

추가로 plantDetailScrollview.setOnScrollChangeListener 부분을 보면, 스크롤 리스너로 더 세밀하게 뷰를 컨트롤 할 수 있습니다. (주석에 설명 잘 되어있습니다.)

4 플러스(+) 모양의 플로팅액션버튼을 클릭하면 ViewModel의 addPlantToGarden() 를 호출해 식물이 My Garden에 저장되고 스낵바 메시지가 띄어집니다. 그리고 플로팅버튼은 사라지게 됩니다. 그리고 이미 MyGarden에 추가된 식물은 플로팅버튼이 hide되게 구현되어 있습니다.
5 툴바 클릭 시 createShareIntent() 가 호출되어 공유하기 기능이 실행됩니다. 텍스트가 공유되게 구현되어있습니다.

추가로 XML 데이터바인딩에서 데이터바인딩 어댑터를 사용한 데이터바인딩 속성이(app:XXX) 적용되어 있는데 여기서 볼 수 있습니다.

Ex) 이미지로딩, Visiblity, HTML to Text 랜더링 등

```Kotlin
@BindingAdapter("imageFromUrl")
fun bindImageFromUrl(view: ImageView, imageUrl: String?) {
    if (!imageUrl.isNullOrEmpty()) {
        Glide.with(view.context)
            .load(imageUrl)
            .transition(DrawableTransitionOptions.withCrossFade())
            .into(view)
    }
}

@BindingAdapter("isFabGone")
fun bindIsFabGone(view: FloatingActionButton, isGone: Boolean?) {
    if (isGone == null || isGone) {
        view.hide()
    } else {
        view.show()
    }
}

@BindingAdapter("renderHtml")
fun bindRenderHtml(view: TextView, description: String?) {
    if (description != null) {
        view.text = HtmlCompat.fromHtml(description, FROM_HTML_MODE_COMPACT)
        view.movementMethod = LinkMovementMethod.getInstance()
    } else {
        view.text = ""
    }
}

@BindingAdapter("wateringText")
fun bindWateringText(textView: TextView, wateringInterval: Int) {
    val resources = textView.context.resources
    val quantityString = resources.getQuantityString(
        R.plurals.watering_needs_suffix,
        wateringInterval,
        wateringInterval
    )

    textView.text = quantityString
}
```
6. @AssistedFactory를 사용하여 뷰모델의 @Assisted 생성자 파라미터를 주입해줍니다.
![image](https://user-images.githubusercontent.com/48902047/149525804-a2c2e33a-c24a-44f3-a5d3-aae6601ad468.png)
![image](https://user-images.githubusercontent.com/48902047/149527574-a833f6b1-328d-4665-b548-0cece2166f7d.png)
![image](https://user-images.githubusercontent.com/48902047/149527590-9ac1887b-9cd9-4405-ac33-d99c535d727a.png)

## GardenFragment
![image](https://user-images.githubusercontent.com/48902047/149527649-e0669ed2-abd4-4718-89d5-ba56f4abc3ef.png)
마지막으로 식물 상세화면에서 추가한 식물들을 보여준 GardenFragment 프래그먼트입니다.
1 마찬가지로 데이터바인딩이 되어있습니다.
2. 이 화면도 리사이클러뷰로 리스트 구조로 되어있므르 어댑터(GalleryAdapter)를 사용합니다.
3. XML을 보면 저장한 식물이 있냐 없냐에 따라 Visiblity가 결정됨을 볼 수 있습니다. app:isGone 은 밑과 같이 데이터바인딩 어댑터로 구현되어있다. 
```Kotlin
//BindingAdapters.kt
@BindingAdapter("isGone")
fun bindIsGone(view: View, isGone: Boolean) {
    view.visibility = if (isGone) {
        View.GONE
    } else {
        View.VISIBLE
    }
}
```
이 프래그먼트에서 사용한 GardenPlantingAdapter는 PlantList 에서 ListAdapter 로 거의 똑같으므로 생략하도록 하겠습니다.

---
title: "[Android] Binding 이란?"
excerpt: "ViewBinding & DataBinding 란?"

categories:
  - Android
tags:
  - [tag1, tag2]

permalink: /Android/Binding/

toc: true
toc_sticky: true

date: 2025-05-30
last_modified_at: 2026-03-28
---

# ViewBinding
1. 특징
	- 코드량 감소 : findViewById() 메소드 대체 		
	- Null Safe : 코드가 간결해지며, Null Pointer Exception 방지
	- Type Safe : View에 잘못된 타입 지정에 대한 사용 시 Class Cast Exception 방지

## (1) build.gradle
```
buildFeatures {
	viewBinding true 
}
```
## (2) MainActvity.java
```
public class MainActivity extends AppCompatActivity {
	private ActivityMainBinding binding;

@Override  
protected void onCreate(Bundle savedInstanceState) {  
    super.onCreate(savedInstanceState);

	// Camel 표기법사용
	// main_activity.xml -> ActivityMainBinding
	binding = ActivityMainBinding.inflate(getLayoutInflater());  
	setContentView(binding.getRoot());

	// 예시)
	binding.title.setTest("텍스트 변경");
}
```

# DataBinding
1. 특징
	- 가독성 : xml 내 UI 변경 코드 작성 가능
	- 화면의 데이터 처리와 이벤트 처리 등을 xml 내 작성
	- Activty or Fragment 내에는 데이터를 위한 Logic 중심 코드만 작성 가능

## (1) build.gradle
```Java
buildFeatures {
	dataBinding true
}
```
## (2) xml
```
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android">
    <data>
        <variable
            name="user"
            type="com.example.app.User" />
    </data>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        android:padding="16dp">

        <TextView
            android:id="@+id/userName"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@{user.name}" />

        <TextView
            android:id="@+id/userAge"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@{String.valueOf(user.age)}" />
    </LinearLayout>
</layout>
```
## (3) User.java
```
public class User {
    private String name;
    private int age;

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }
}
```
## (4) MainActivity.java
```
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // DataBindingUtil을 사용하여 레이아웃을 인플레이트하고
        // 바인딩 객체를 생성
        ActivityMainBinding binding = DataBindingUtil.inflate(
                getLayoutInflater(),
                R.layout.activity_main,
                null,
                false
        );
        binding.setLifecycleOwner(getViewLifecycleOwner());

        // User 객체를 생성하고 layout에 바인딩
        User user = new User("GJKim", 30);
        binding.setUser(user);

        // 바인딩된 뷰를 setContentView로 설정
        setContentView(binding.getRoot());
    }
}
```

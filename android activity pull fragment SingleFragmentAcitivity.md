--- 
title: Fragment 提交的Activity的封装 
tags:  ["android"]
notebook: Knowledge 
---

# Fragment 提交的Activity的封装 

###SingleFragmentActivity.java

```java
	import android.os.Bundle;
	import android.support.v4.app.Fragment;
	import android.support.v4.app.FragmentActivity;
	import android.support.v4.app.FragmentManager;
	import android.support.v7.app.AppCompatActivity;

	/***
	 * ********** SingleFragmentActivity **********
	 *
	 * 父类: FragmentActivity
	 * 功能: 将生成Fragment的代码封装在父类中继承的onCreate()。
	 * 接口: abstract Fragment createFragment()
	 *      具体的fragment生成过程还需要自定义，这里的封装只是将
	 *      fragment指向布局容器这部分相同的代码写出来。
	 *
	 * Created by taoroot on 17/2/25.
	 */

	public abstract class SingleFragmentActivity extends AppCompatActivity {
        //如何新建fragment
	    protected abstract Fragment createFragment();                         

	    @Override
	    public void onCreate(Bundle savedInstanceState) {
	        super.onCreate(savedInstanceState);
	        setContentView(R.layout.activity_fragment);
        	//Fragment管理器
	        FragmentManager fm = getSupportFragmentManager(); 
            //fragment指向布局资源fragment容器组件的ID                  
            Fragment fragment = fm.findFragmentById(R.id.fragment_container);   
            //先查看下Fragment容器中是否已经有Fragment了
            //这个可能是由于某些,导致Activity重建,但是Fragment并没有被清除,可以直接拿过来使用.
        	//所以这里要先判断下fragment是否是没空,是空的才对他分配内存.                                                              
	        if(fragment == null){                                              
	            fragment = createFragment();
	            fm.beginTransaction()
                    .add(R.id.fragment_container, fragment)
                    .commit();
	        }
	    }
	}

###activity_fragment.XML

    <?xml version="1.0" encoding="utf-8"?>
    <FrameLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        android:id="@+id/fragment_container"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
    </FrameLayout>
```

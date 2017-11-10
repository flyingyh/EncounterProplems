# EncounterProplems
常见问题总结
  1.导入项目很慢：

方法：不用下载gradle，直接修改gradle-wrapper.properties文件
        1.打开Android Studio，选择一个已经能正常导入或已经成功创建的android studio项目，没有可以新建一个项目，找到项目的gradle文件夹，打开后找到wrapper文件夹里面的gradle-wrapper.properties文件，复制最后一行。类似：distributionUrl=https\://services.gradle.org/distributions/gradle-2.14.1-all.zip

        2.打开需要导入的项目的文件夹，找到gradle文件夹，打开后找到wrapper文件夹里面的gradle-wrapper.properties文件，打开，将上一步骤复制好的文本替换掉gradle-wrapper.properties文件的最后一行，然后保存。重启android studio，便能正常导入该项目，无需在线下载对应版本的gradle。


2.底部弹框：botoomsheet

  弹窗(宽度满屏)：用fragment
	建立一个GuaranteePaymentFragment 继承DialogFragment
	
	跳到该对话框
  GuaranteePaymentFragment paymentFragment = new GuaranteePaymentFragment();
                Bundle bundle = new Bundle();
                bundle.putString("payFragment2", tv_inv.getText()+"");
                paymentFragment.setArguments(bundle);
                paymentFragment.show(getSupportFragmentManager(),"payFragment2");

3.刷新控件：SwipeRefreshLayout只有下拉刷新 
	  PullRefreshListVIew
	  TwinklingRefreshLayout(自己打算使用的)
 	  TwinklingRefreshLayout：
    setHeaderView(new SinaRefreshView(getContext()));
    tr.setOnRefreshListener(new RefreshListenerAdapter() {
            @Override
            public void onRefresh(TwinklingRefreshLayout refreshLayout) {
                super.onRefresh(refreshLayout);
            }

            @Override
            public void onLoadMore(TwinklingRefreshLayout refreshLayout) {
                super.onLoadMore(refreshLayout);
                page++;
            }
        });

4.设置文本颜色：
tv_seventh.setText(Html.fromHtml("<font color=\"#ff532a\">输入金额超过可提现余额</font>"));

5.Listview的adapter.notifyDataSetChanged()不起作用的原因
	notifyDataSetChanged其实执行的是getView()方法，如果数据源没有改变就不会执行
	还有就是当listview的高度为wrapcontent时，没效果，改为固定高度就有效果了，原因是

6.防止键盘把界面顶上：
	要在AndroidManifest.xml里面的Activity加上这样一段：android:windowSoftInputMode="stateAlwaysHidden|adjustPan"
  
7.fragment相关问题：APP放半个小时没用,然后再刷新,就出现bug了,刷新会出现双层
    该问题出现的原因：fragment虽然不继承于View，但是与普通的View享受同样的待遇，普通的View在Actiivty内存不够的时候会进行现场保护，保留View当前状                       态，fragment也是如此。初始化Fragment时，在activity每次onCreate的时候都会走，但是Acitivity被回收以后由于fragment特殊性，会被                     Actiivty现场保存，也就是说在这个Activity现场恢复回来以后，本来已经保存了一个fragment，然后又走了你的代码再创建了一个Framgent
    解决方法：
        第一种是：重写onsaveInstanceState（）,把super的方法给干掉
        第二种是： @Override
                  protected void onCreate(Bundle savedInstanceState) {
                    super.onCreate(savedInstanceState);
                    //在此处对全局变量进行恢复
                    if(savedInstanceState != null){
                      //如果发现Activity有数据恢复，则不手动 add fragment
                    }
                  }
        
    

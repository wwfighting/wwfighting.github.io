---
layout: post
title:  "startActivityForResult"
comments: true
categories: android
---

* content
{:toc}

## startActivityForResult的运用场景

最近在项目上遇到页面跳转携带参数的情况，之前一篇博客写到了在android中的传值的几种情况，然而
我却忽略了activity一个最简单的回调事件！！！！

比如一个经常用到的场景：“修改个人信息”！ 在 Activity A 中可以查看到某个登录用户的基本信息，
当你要进行修改的时候，比如修改该用户的 “个性签名”，点击Activity A中的个性签名跳转到Activity B中
进行该用户个性签名的编写，然后返回到A中，将更改的信息的进行提交，与服务器进行交互。这时候问题来了，
如果是startActivity的话，会重新开启一个Activity A，即使将B finish()掉，在A界面按底部返回键，也是回
到之前的A界面，再按一次返回键才回退出A。
这时候最好的解决方法是使用startActivityForResult()方法！

使用情况：从A启动B，B干完活将结果返回到A。
具体的使用方法为：若 A -> B，即 A启动B，在A中跳转B的代码为：

public final static int REQUEST_CODE = 100;
public final static int RESULT_CODE = 200;

Intent intent = new Intent(A.this, B.class);
startActivityForResult(intent, REQUEST_CODE);

在B中的代码为：

  public final static int RESULT_CODE = 200;
  String strReuslt = "B中返回到A的结果";
  Intent intent = new Intent(B.this, A.class);
  intent.putExtra("result", strReuslt);
  strReuslt(RESULT_CODE, intent);
  finish();//记住一定要finish掉activity B;


这时候再次跳转到A，要重写onActivityResult()方法来处理B中返回的结果：

  protected void onActivityResult(int requestCode, int resultCode, intent data){
    if(data != null){
      switch(resultCode){
        case:RESULT_CODE
        String strReuslt = data.getSringExtra("result");
        break;
        default:
        break;
      }
    }
  }

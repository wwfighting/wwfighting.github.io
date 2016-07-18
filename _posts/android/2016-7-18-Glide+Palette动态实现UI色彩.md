---
layout: post
title:  "Glide+Palette动态实现UI色彩"
comments: true
categories: android
---

* content
{:toc}

## 介绍Glide、Palette

Glide：
* 一款Google官方开发的加载处理图片的三方库。类似Picasso，API的调用与Picasso极其相似，
不过Glide可以直接加载gif动图；
* 其方法也比Picasso更加的丰富，所以Glide库的大小基本上是Picasso的两倍；
* 不仅仅依附于context，可以是当前的Activity也可以是Fragment。

Palette：
* 中文意思为调色板，即给界面进行上色。用法为从Bitmap中提取出主色调，然后设置给其他的View。
* Palette能提取以下突出的颜色：
 六种样本，即Palette.Swatch对象。
		Vibrant(充满活力的)
		Vibrant dark(充满活力的黑)
		Vibrant light(充满活力的亮)
		Muted(柔和的)
		Muted dark(柔和的黑)
		Muted lighr(柔和的亮)

## 具体的使用

1) 封装一个GlideUtil方法，用来将Glide加载的网络图片GlideDrawable对象转换成Bitmap：

				public class GlideUtil {
				    //得到bitmap
				    private GlideUtil() {
				    }

				    public static Bitmap getBitmap(GlideDrawable glideDrawable) {
				        if (glideDrawable instanceof GlideBitmapDrawable) {
				            return ((GlideBitmapDrawable) glideDrawable).getBitmap();
				        } else if (glideDrawable instanceof GifDrawable) {
				            return ((GifDrawable) glideDrawable).getFirstFrame();
				        }
				        return null;
				    }
				}

2) 封装一个ColorUtil方法，用来获得Bitmap对象的主色调：

				public class ColorUtil {
				    public static int getImageColor(Bitmap bitmap){
				        Palette palette = Palette.from(bitmap).generate();
				        if(palette == null || palette.getLightVibrantSwatch() == null){
				            return Color.LTGRAY;
				        }
				        return palette.getLightVibrantSwatch().getRgb();
				    }
				}

2) 实现Glide的Listener监听器，实现其onResourceReady()回调方法，获得Bitmap。
利用Palette提取出Bitmap色调，设置为Button的背景色

				private RequestListener shotLoadListener = new RequestListener<String, GlideDrawable>() {
				        @Override
				        public boolean onException(Exception e, String model, Target<GlideDrawable> target,
				                                   boolean isFirstResource) {
				            return false;
				        }

				        @Override
				        public boolean onResourceReady(GlideDrawable resource, String model,
				                                       Target<GlideDrawable> target, boolean isFromMemoryCache,
				                                       boolean isFirstResource) {
				            final Bitmap bitmap = GlideUtil.getBitmap(resource);
				            Palette.from(bitmap)
				                    .maximumColorCount(3)
				                    .clearFilters()
				                    .setRegion(0, 0, bitmap.getWidth() - 1, bitmap.getHeight() - 1)
				                    .generate(new Palette.PaletteAsyncListener() {
				                        @Override
				                        public void onGenerated(Palette palette) {
				                            mbtn.setBackgroundColor(ColorUtil.getImageColor(bitmap));
				                        }
				                    });
				            return false;
				        }
				    };


3) 获得图片，实现RequestListener接口：
        private void loadImage(){
        			 Glide.with(this)
        							 .load("http://www.gxershou.com/pictures/1/20/1505251436451.jpg")
        							 .listener(shotLoadListener)
        							 .centerCrop()
        							 .crossFade()
        							 .transform(new CircleTransform(this))
        							 .into(mivShow);
        	 }

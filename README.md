## 前言

大家好啊！好久没有见到我了吧！为什么呢！当然是因为开学啦，这学期很多课，身为部长实验室也也很多活动和一堆小师弟，同时还有蓝桥杯和华为软件开发大赛，而且最近在做一个综合性比较高的作品，没错了，就是酷云，一款仿网易云音乐的在线播放器。当然了，现在我还没有完成这个作品，而且只是刚刚开始写而已，不过也遇到了很多坑，所以写下这篇文章来记录一下这次开发中各种各样的坑，希望对各位有所帮助，文章会随着我的开发进度而不定期更新，各位有问题也可以给我发私信，我们一起讨论解决。至于为什么选择网易云音乐我觉得主要是因为它不仅有着 93% 的主流音乐版权，可以找到最齐全的主流音乐；出色的用户界面和体验，让用户感觉舒适；还有人性化的体验，如生日推荐音乐。

---

## 自定义 View 控件

不知道各位有没有体验过网易云音乐 APP ,通过我最近的体验，发现它有着很好的用户界面，对各种操作都有着很好的响应，大家可以先去体验一下，下面给出几张网易云音乐的 UI。

![这里写图片描述](http://img.blog.csdn.net/20170316192839490?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamltX19jaGFybGVz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![这里写图片描述](http://img.blog.csdn.net/20170316192901006?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamltX19jaGFybGVz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![这里写图片描述](http://img.blog.csdn.net/20170316192916710?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamltX19jaGFybGVz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![这里写图片描述](http://img.blog.csdn.net/20170316192948193?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamltX19jaGFybGVz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

接下来放上酷云相对应的页面

![这里写图片描述](http://img.blog.csdn.net/20170316192033916?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamltX19jaGFybGVz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![这里写图片描述](http://img.blog.csdn.net/20170316192119214?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamltX19jaGFybGVz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![这里写图片描述](http://img.blog.csdn.net/20170316192147980?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamltX19jaGFybGVz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![这里写图片描述](http://img.blog.csdn.net/20170316192237294?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamltX19jaGFybGVz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

说实话从开始接触自定义 View 到现在也不久，学的不多，但是比较喜欢挑战，因此便有了这个项目的开始。好了，相信大家也已经体验过网易云的用户界面了，就算没有，接下来我也会一点点为你解答。

---

### 自定义 SplashScreen 实现启动动画

```
/**
 * Created by JimCharles on 2017/3/7.
 */

import android.app.Activity;
import android.app.Dialog;
import android.graphics.Color;
import android.util.DisplayMetrics;
import android.view.ViewGroup;
import android.view.Window;
import android.view.WindowManager;
import android.widget.LinearLayout;

import com.androxue.coolcloud.R;

public class SplashScreen {

    public final static int SLIDE_LEFT = 1;
    public final static int SLIDE_UP = 2;
    public final static int FADE_OUT = 3;

    private Dialog splashDialog;

    private Activity activity;

    public SplashScreen(Activity activity) {
        this.activity = activity;
    }

    public void show(final int imageResource, final int animation) {
        Runnable runnable = new Runnable() {
            public void run() {
                // Get reference to display
                DisplayMetrics metrics = new DisplayMetrics();
//                Display display = activity.getWindowManager().getDefaultDisplay();

                // Create the layout for the dialog
                LinearLayout root = new LinearLayout(activity);
                root.setMinimumHeight(metrics.heightPixels);
                root.setMinimumWidth(metrics.widthPixels);
                root.setOrientation(LinearLayout.VERTICAL);
                root.setBackgroundColor(Color.BLACK);
                root.setLayoutParams(new LinearLayout.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT,
                        ViewGroup.LayoutParams.MATCH_PARENT, 0.0F));
                root.setBackgroundResource(imageResource);

                // Create and show the dialog
                splashDialog = new Dialog(activity, android.R.style.Theme_Translucent_NoTitleBar);
                // check to see if the splash screen should be full screen
                if ((activity.getWindow().getAttributes().flags & WindowManager.LayoutParams.FLAG_FULLSCREEN)
                        == WindowManager.LayoutParams.FLAG_FULLSCREEN) {
                    splashDialog.getWindow().setFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN,
                            WindowManager.LayoutParams.FLAG_FULLSCREEN);
                }
                Window window = splashDialog.getWindow();
                switch (animation) {
                    case SLIDE_LEFT:
                        window.setWindowAnimations(R.style.dialog_anim_slide_left);
                        break;
                    case SLIDE_UP:
                        window.setWindowAnimations(R.style.dialog_anim_slide_up);
                        break;
                    case FADE_OUT:
                        window.setWindowAnimations(R.style.dialog_anim_fade_out);
                        break;
                }

                splashDialog.setContentView(root);
                splashDialog.setCancelable(false);
                splashDialog.show();

                // Set Runnable to remove splash screen just in case
                /*final Handler handler = new Handler();
                handler.postDelayed(new Runnable() {
                    public void run() {
                        removeSplashScreen();
                    }
                }, millis);*/
            }
        };
        activity.runOnUiThread(runnable);
    }

    public void removeSplashScreen() {
        if (splashDialog != null && splashDialog.isShowing()) {
            splashDialog.dismiss();
            splashDialog = null;
        }
    }

}

```

然后在 MianActivity 的onCreate()方法中添加以下两行代码即可实现应用启动动画：

```
splashScreen = new SplashScreen(this);
        splashScreen.show(R.drawable.art_login_bg,
                SplashScreen.SLIDE_LEFT);
```

---

### ViewPager＋PagerTabStrip 实现滑动切屏

进去网易云后，主界面如下一个最大的特点就是滑动切屏了，这个效果既方便又帅气，所以网易云音乐中大量应用了这一效果，那么我们首先便来讲解一下滑动切屏的实现。

首先构建在 activity_main.xml 中构建 ViewPager 和 PagerTabStrip，用来实现切屏效果，代码如下：

```
<android.support.v4.view.ViewPager
             android:layout_width="match_parent"
             android:layout_height="match_parent"
             android:layout_gravity="center"
             android:gravity="center"
             android:id="@+id/vp">
             <android.support.v4.view.PagerTabStrip
                android:layout_width="match_parent"
                 android:layout_height="wrap_content"
                 android:id="@+id/tap">
             </android.support.v4.view.PagerTabStrip>
    </android.support.v4.view.ViewPager>
```

然后构建切换的子 View，为了方便观察，这里只是通过 ImageView 简单的设置显示不同的颜色，main_layout_1.xml

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <ImageView
        android:id="@+id/view_1"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:src="#76ff03"/>

</LinearLayout>
```

然后在 Activity 中进行设置，MainActivity.java

```
package com.androxue.coolcloud.activity;

import android.content.Intent;
import android.graphics.Color;
import android.os.Bundle;
import android.support.annotation.Nullable;
import android.support.v4.view.PagerAdapter;
import android.support.v4.view.PagerTabStrip;
import android.support.v4.view.ViewPager;
import android.support.v7.app.AppCompatActivity;
import android.view.View;
import android.view.ViewGroup;
import android.widget.ImageView;

import com.androxue.coolcloud.R;

import java.util.ArrayList;

/**
 * Created by JimCharles on 2017/3/11.
 */

public class MainActivity extends AppCompatActivity {

    private ViewPager vp;
     //声明存储ViewPager下子视图的集合
     ArrayList<View> views = new ArrayList<>();
     //显示效果中每个视图的标题
     String[] titles={"私信", "评论", "@我", "通知"};

     @Override
     protected void onCreate(@Nullable Bundle savedInstanceState) {
         super.onCreate(savedInstanceState);
         setContentView(R.layout.activity_message);
         vp = (ViewPager) findViewById(R.id.vp);
         initView();//调用初始化视图方法
         vp.setAdapter(new MyAdapter());//设置适配器
         ImageView back = (ImageView) findViewById(R.id.back);
         back.setOnClickListener(new View.OnClickListener() {
             @Override
             public void onClick(View v) {
                 Intent intent = new Intent(MessageActivity.this, MainActivity.class);
                 startActivity(intent);
             }
         });
     }

    //初始化视图的方法（通过布局填充器获取用于滑动的视图并存入对应的的集合）
    private void initView() {
        View v1 = getLayoutInflater().inflate(R.layout.main_layout_1, null);
        View v2 = getLayoutInflater().inflate(R.layout.main_layout_2, null);
        View v3 = getLayoutInflater().inflate(R.layout.main_layout_3, null);
        View v4 = getLayoutInflater().inflate(R.layout.main_layout_4, null);
        views.add(v1);
        views.add(v2);
        views.add(v3);
        views.add(v4);

        PagerTabStrip pagerTabStrip= (PagerTabStrip)findViewById(R.id.tap);
        pagerTabStrip.setDrawFullUnderline(false);//取消标题栏子View之间的分割线
        pagerTabStrip.setTabIndicatorColor(Color.RED);//改变指示器颜色为红色
        pagerTabStrip.setTextColor(Color.RED);//该变字体颜色为红色
    }

     private class MyAdapter extends PagerAdapter {

         @Override
         public int getCount() {
             return views.size();
         }

        @Override
         public boolean isViewFromObject(View view, Object object) {
            return view == object;
        }

         //重写销毁滑动视图布局（将子视图移出视图存储集合（ViewGroup））
         @Override
         public void destroyItem(ViewGroup container, int position, Object object) {
             container.removeView(views.get(position));
         }

         //重写初始化滑动视图布局（从视图集合中取出对应视图，添加到ViewGroup）
         @Override
         public Object instantiateItem(ViewGroup container, int position) {
             View v = views.get(position);
             container.addView(v);
             return v;
         }

         @Override
         public CharSequence getPageTitle(int position) {
             return titles[position];
         }
    }
}

```

好了，一个滑动切屏的功能就这样完成了，大家可以自行运行体验一下。

---

### 圆形头像

关于圆形头像的实现在之前[ Android 自定义 View 之 draw 原理分析](http://blog.csdn.net/jim__charles/article/details/56307488)一文已经有讲到，这里简单的讲解一下，顺便讲解另一种实现方式，两种方式都是通过自定义View控件来实现的。


#### 方式一：Shader+onDraw() 实现圆形头像

```
protected void onDraw(Canvas canvas) {
     super.onDraw(canvas);
     Paint paint = new Paint();
     paint.setAntiAlias(true);
     Bitmap bitmap = BitmapFactory.decodeResource(getResources(),R.drawable.girl);
     int radius = bitmap.getWidth()/2;
     BitmapShader bitmapShader = new BitmapShader(bitmap,Shader.TileMode.REPEAT,Shader.TileMode.REPEAT);
     paint.setShader(bitmapShader);
     canvas.translate(250,430);
     canvas.drawCircle(radius, radius, radius, paint);
}
```

和之前讲到的一样，甚至没有改代码，利用 BitmapShader 并重写 onDraw() 方法来实现圆形头像，不过这里有一点需要注意的是图片的像素大小才能完美的实现圆形头像，可以自行进行尝试。

#### 方式二：自定义 CircleImageView 实现圆形头像

```
package com.androxue.coolcloud.widget;

import android.content.Context;
import android.content.res.TypedArray;
import android.graphics.Bitmap;
import android.graphics.BitmapShader;
import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.ColorFilter;
import android.graphics.Matrix;
import android.graphics.Paint;
import android.graphics.RectF;
import android.graphics.Shader;
import android.graphics.drawable.BitmapDrawable;
import android.graphics.drawable.ColorDrawable;
import android.graphics.drawable.Drawable;
import android.net.Uri;
import android.support.annotation.ColorInt;
import android.support.annotation.ColorRes;
import android.support.annotation.DrawableRes;
import android.util.AttributeSet;

import com.androxue.coolcloud.R;

/**
 * Created by JimCharles on 2017/3/9.
 */

public class CircleImageView extends android.support.v7.widget.AppCompatImageView {

    private static final ScaleType SCALE_TYPE = ScaleType.CENTER_CROP;

    private static final Bitmap.Config BITMAP_CONFIG = Bitmap.Config.ARGB_8888;
    private static final int COLORDRAWABLE_DIMENSION = 2;

    private static final int DEFAULT_BORDER_WIDTH = 0;
    private static final int DEFAULT_BORDER_COLOR = Color.BLACK;
    private static final int DEFAULT_FILL_COLOR = Color.TRANSPARENT;
    private static final boolean DEFAULT_BORDER_OVERLAY = false;

    private final RectF mDrawableRect = new RectF();
    private final RectF mBorderRect = new RectF();

    private final Matrix mShaderMatrix = new Matrix();
    private final Paint mBitmapPaint = new Paint();
    private final Paint mBorderPaint = new Paint();
    private final Paint mFillPaint = new Paint();

    private int mBorderColor = DEFAULT_BORDER_COLOR;
    private int mBorderWidth = DEFAULT_BORDER_WIDTH;
    private int mFillColor = DEFAULT_FILL_COLOR;

    private Bitmap mBitmap;
    private BitmapShader mBitmapShader;
    private int mBitmapWidth;
    private int mBitmapHeight;

    private float mDrawableRadius;
    private float mBorderRadius;

    private ColorFilter mColorFilter;

    private boolean mReady;
    private boolean mSetupPending;
    private boolean mBorderOverlay;
    private boolean mDisableCircularTransformation;

    public CircleImageView(Context context) {
        super(context);

        init();
    }

    public CircleImageView(Context context, AttributeSet attrs) {
        this(context, attrs, 0);
    }

    public CircleImageView(Context context, AttributeSet attrs, int defStyle) {
        super(context, attrs, defStyle);

        TypedArray a = context.obtainStyledAttributes(attrs, R.styleable.CircleImageView, defStyle, 0);

        mBorderWidth = a.getDimensionPixelSize(R.styleable.CircleImageView_civ_border_width, DEFAULT_BORDER_WIDTH);
        mBorderColor = a.getColor(R.styleable.CircleImageView_civ_border_color, DEFAULT_BORDER_COLOR);
        mBorderOverlay = a.getBoolean(R.styleable.CircleImageView_civ_border_overlay, DEFAULT_BORDER_OVERLAY);
        mFillColor = a.getColor(R.styleable.CircleImageView_civ_fill_color, DEFAULT_FILL_COLOR);

        a.recycle();

        init();
    }

    private void init() {
        super.setScaleType(SCALE_TYPE);
        mReady = true;

        if (mSetupPending) {
            setup();
            mSetupPending = false;
        }
    }

    @Override
    public ScaleType getScaleType() {
        return SCALE_TYPE;
    }

    @Override
    public void setScaleType(ScaleType scaleType) {
        if (scaleType != SCALE_TYPE) {
            throw new IllegalArgumentException(String.format("ScaleType %s not supported.", scaleType));
        }
    }

    @Override
    public void setAdjustViewBounds(boolean adjustViewBounds) {
        if (adjustViewBounds) {
            throw new IllegalArgumentException("adjustViewBounds not supported.");
        }
    }

    @Override
    protected void onDraw(Canvas canvas) {
        if (mDisableCircularTransformation) {
            super.onDraw(canvas);
            return;
        }

        if (mBitmap == null) {
            return;
        }

        if (mFillColor != Color.TRANSPARENT) {
            canvas.drawCircle(mDrawableRect.centerX(), mDrawableRect.centerY(), mDrawableRadius, mFillPaint);
        }
        canvas.drawCircle(mDrawableRect.centerX(), mDrawableRect.centerY(), mDrawableRadius, mBitmapPaint);
        if (mBorderWidth > 0) {
            canvas.drawCircle(mBorderRect.centerX(), mBorderRect.centerY(), mBorderRadius, mBorderPaint);
        }
    }

    @Override
    protected void onSizeChanged(int w, int h, int oldw, int oldh) {
        super.onSizeChanged(w, h, oldw, oldh);
        setup();
    }

    @Override
    public void setPadding(int left, int top, int right, int bottom) {
        super.setPadding(left, top, right, bottom);
        setup();
    }

    @Override
    public void setPaddingRelative(int start, int top, int end, int bottom) {
        super.setPaddingRelative(start, top, end, bottom);
        setup();
    }

    public int getBorderColor() {
        return mBorderColor;
    }

    public void setBorderColor(@ColorInt int borderColor) {
        if (borderColor == mBorderColor) {
            return;
        }

        mBorderColor = borderColor;
        mBorderPaint.setColor(mBorderColor);
        invalidate();
    }

    /**
     * @deprecated Use {@link #setBorderColor(int)} instead
     */
    @Deprecated
    public void setBorderColorResource(@ColorRes int borderColorRes) {
        setBorderColor(getContext().getResources().getColor(borderColorRes));
    }

    /**
     * Return the color drawn behind the circle-shaped drawable.
     *
     * @return The color drawn behind the drawable
     *
     * @deprecated Fill color support is going to be removed in the future
     */
    @Deprecated
    public int getFillColor() {
        return mFillColor;
    }

    /**
     * Set a color to be drawn behind the circle-shaped drawable. Note that
     * this has no effect if the drawable is opaque or no drawable is set.
     *
     * @param fillColor The color to be drawn behind the drawable
     *
     * @deprecated Fill color support is going to be removed in the future
     */
    @Deprecated
    public void setFillColor(@ColorInt int fillColor) {
        if (fillColor == mFillColor) {
            return;
        }

        mFillColor = fillColor;
        mFillPaint.setColor(fillColor);
        invalidate();
    }

    /**
     * Set a color to be drawn behind the circle-shaped drawable. Note that
     * this has no effect if the drawable is opaque or no drawable is set.
     *
     * @param fillColorRes The color resource to be resolved to a color and
     *                     drawn behind the drawable
     *
     * @deprecated Fill color support is going to be removed in the future
     */
    @Deprecated
    public void setFillColorResource(@ColorRes int fillColorRes) {
        setFillColor(getContext().getResources().getColor(fillColorRes));
    }

    public int getBorderWidth() {
        return mBorderWidth;
    }

    public void setBorderWidth(int borderWidth) {
        if (borderWidth == mBorderWidth) {
            return;
        }

        mBorderWidth = borderWidth;
        setup();
    }

    public boolean isBorderOverlay() {
        return mBorderOverlay;
    }

    public void setBorderOverlay(boolean borderOverlay) {
        if (borderOverlay == mBorderOverlay) {
            return;
        }

        mBorderOverlay = borderOverlay;
        setup();
    }

    public boolean isDisableCircularTransformation() {
        return mDisableCircularTransformation;
    }

    public void setDisableCircularTransformation(boolean disableCircularTransformation) {
        if (mDisableCircularTransformation == disableCircularTransformation) {
            return;
        }

        mDisableCircularTransformation = disableCircularTransformation;
        initializeBitmap();
    }

    @Override
    public void setImageBitmap(Bitmap bm) {
        super.setImageBitmap(bm);
        initializeBitmap();
    }

    @Override
    public void setImageDrawable(Drawable drawable) {
        super.setImageDrawable(drawable);
        initializeBitmap();
    }

    @Override
    public void setImageResource(@DrawableRes int resId) {
        super.setImageResource(resId);
        initializeBitmap();
    }

    @Override
    public void setImageURI(Uri uri) {
        super.setImageURI(uri);
        initializeBitmap();
    }

    @Override
    public void setColorFilter(ColorFilter cf) {
        if (cf == mColorFilter) {
            return;
        }

        mColorFilter = cf;
        applyColorFilter();
        invalidate();
    }

    @Override
    public ColorFilter getColorFilter() {
        return mColorFilter;
    }

    private void applyColorFilter() {
        if (mBitmapPaint != null) {
            mBitmapPaint.setColorFilter(mColorFilter);
        }
    }

    private Bitmap getBitmapFromDrawable(Drawable drawable) {
        if (drawable == null) {
            return null;
        }

        if (drawable instanceof BitmapDrawable) {
            return ((BitmapDrawable) drawable).getBitmap();
        }

        try {
            Bitmap bitmap;

            if (drawable instanceof ColorDrawable) {
                bitmap = Bitmap.createBitmap(COLORDRAWABLE_DIMENSION, COLORDRAWABLE_DIMENSION, BITMAP_CONFIG);
            } else {
                bitmap = Bitmap.createBitmap(drawable.getIntrinsicWidth(), drawable.getIntrinsicHeight(), BITMAP_CONFIG);
            }

            Canvas canvas = new Canvas(bitmap);
            drawable.setBounds(0, 0, canvas.getWidth(), canvas.getHeight());
            drawable.draw(canvas);
            return bitmap;
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }

    private void initializeBitmap() {
        if (mDisableCircularTransformation) {
            mBitmap = null;
        } else {
            mBitmap = getBitmapFromDrawable(getDrawable());
        }
        setup();
    }

    private void setup() {
        if (!mReady) {
            mSetupPending = true;
            return;
        }

        if (getWidth() == 0 && getHeight() == 0) {
            return;
        }

        if (mBitmap == null) {
            invalidate();
            return;
        }

        mBitmapShader = new BitmapShader(mBitmap, Shader.TileMode.CLAMP, Shader.TileMode.CLAMP);

        mBitmapPaint.setAntiAlias(true);
        mBitmapPaint.setShader(mBitmapShader);

        mBorderPaint.setStyle(Paint.Style.STROKE);
        mBorderPaint.setAntiAlias(true);
        mBorderPaint.setColor(mBorderColor);
        mBorderPaint.setStrokeWidth(mBorderWidth);

        mFillPaint.setStyle(Paint.Style.FILL);
        mFillPaint.setAntiAlias(true);
        mFillPaint.setColor(mFillColor);

        mBitmapHeight = mBitmap.getHeight();
        mBitmapWidth = mBitmap.getWidth();

        mBorderRect.set(calculateBounds());
        mBorderRadius = Math.min((mBorderRect.height() - mBorderWidth) / 2.0f, (mBorderRect.width() - mBorderWidth) / 2.0f);

        mDrawableRect.set(mBorderRect);
        if (!mBorderOverlay && mBorderWidth > 0) {
            mDrawableRect.inset(mBorderWidth - 1.0f, mBorderWidth - 1.0f);
        }
        mDrawableRadius = Math.min(mDrawableRect.height() / 2.0f, mDrawableRect.width() / 2.0f);

        applyColorFilter();
        updateShaderMatrix();
        invalidate();
    }

    private RectF calculateBounds() {
        int availableWidth  = getWidth() - getPaddingLeft() - getPaddingRight();
        int availableHeight = getHeight() - getPaddingTop() - getPaddingBottom();

        int sideLength = Math.min(availableWidth, availableHeight);

        float left = getPaddingLeft() + (availableWidth - sideLength) / 2f;
        float top = getPaddingTop() + (availableHeight - sideLength) / 2f;

        return new RectF(left, top, left + sideLength, top + sideLength);
    }

    private void updateShaderMatrix() {
        float scale;
        float dx = 0;
        float dy = 0;

        mShaderMatrix.set(null);

        if (mBitmapWidth * mDrawableRect.height() > mDrawableRect.width() * mBitmapHeight) {
            scale = mDrawableRect.height() / (float) mBitmapHeight;
            dx = (mDrawableRect.width() - mBitmapWidth * scale) * 0.5f;
        } else {
            scale = mDrawableRect.width() / (float) mBitmapWidth;
            dy = (mDrawableRect.height() - mBitmapHeight * scale) * 0.5f;
        }

        mShaderMatrix.setScale(scale, scale);
        mShaderMatrix.postTranslate((int) (dx + 0.5f) + mDrawableRect.left, (int) (dy + 0.5f) + mDrawableRect.top);

        mBitmapShader.setLocalMatrix(mShaderMatrix);
    }

}

```

这种方式是通过自定义 View 来实现圆形头像的，代码很简单

---

### 夜间模式

关于夜间模式的实现是在我开发过程中遇到的一个比较大坑，但我们都知道夜间模式是一款 APP 应该具备的，通过我最近的了解，总结出了四种实现方式。一是定义两套主题，对每一个需要实现夜间模式的控件都另外定义一套夜间主题，但这样子工程量大，我是一个人开发的，所以并没有采用这种方式；二是利用 Google 官方提供的 UiModeManger；三是调用 NightModeHelper 实现夜间模式;四是利用 AppCompatDelegate 实现夜间模式。

#### Light_Style + Night_Style（即 setTheme）实现夜间模式

Light_Style  + Night_Style 实现夜间模式的思路在于分别定义日间模式和夜间模式两套主题，然后再分别添加两套模式资源文件，也就是添加两个 module，生成 apk 文件，而模式的实现也就是对这两个 apk 文件进行处理，其实也就是价格 .apk 后缀更改为 .skin 或者其他后缀，实现难度较大，不建议使用

```

```

#### UiModeManger 实现夜间模式（）

UiModeManger 是谷歌官方给我们提供的实现夜间模式的方式，我们来看一下官方文档对它的描述：

> This class provides access to the system uimode services. These services allow applications to control UI modes of the device. It provides functionality to disable the car mode and it gives access to the night mode settings.

上面的意思就是：

> 这类提供对系统uimode服务的访问。这些服务允许应用程序控制设备的UI模式。它提供了禁用汽车模式的功能，它允许访问夜间模式设置。

看到这样的解释大家可能会有疑问，不是用来实现夜间模式的吗？怎么跑出了一个车载模式呢？仔细看这个类的文档，发现有一个 setNightMode() 方法，这不就是设置夜间模式的意思吗？点进去看到这个方法的解释如下：

> Sets the night mode. Changes to the night mode are only effective when the car or desk mode is enabled on a device.

意思如下：
 
> 设置夜间模式。对夜间模式的更改仅在设备上启用汽车或桌面模式时有效。

不得不说坑爹啊！必须在开启车载模式或者桌面模式的条件下才能设置夜间模式，再仔细看一下，发现UiModeManager 仅提供了方法设置车载模式的设置而没有Desk模式的设置方法，所以下面演示通过 UiModeManger 来实现夜间模式的方式。

首先在 res 包下创建 drawable-night-hdpi、drawable-night-mdpi、drawable-night-xhdpi、drawable-night-xxhdpi、drawable-night-xxxhdpi、values-night 文件夹，然后将相对应的资源放入新创建的对应的包下，并在 values-night 包下新建 colors.xml 资源文件，以下颜色仅供参考

```
<resources>

    <color name="night_mode_color ">#7f7f7f</color>
    <color name="night_mode_dark_color ">#d20000</color>
    <color name="night_mode_color ">#0a0a0a</color>    

</resources>
```

需要注意的一点是颜色名需要和 vales 包下的 colors.xml 文件中颜色名相同，但颜色值可以不同

然后在 values 包下的 styles.xml 文件中自定义实现夜间模式的主题

```
<style name="Theme.Test" parent="Theme.AppCompat.Light.NoActionBar">
    <item name="colorPrimary">@color/night_mode_color </item>
    <item name="colorPrimaryDark">@color/night_mode_dark_color </item>
    <item name="colorAccent">@color/night_mode_color </item>
    <item name="android:windowBackground">@color/dark</item>
</style>
```

好了，这样我们就自定义了夜间模式的主题了，接下来在需要实现夜间模式的地方加上一下代码就可以实现夜间模式了

```
UiModeManager uiManager = (UiModeManager) getSystemService(Context.UI_MODE_SERVICE);
if (isNightMode) {
    uiManager.enableCarMode(0);
    uiManager.setNightMode(UiModeManager.MODE_NIGHT_YES);
} else {
    uiManager.disableCarMode(0);
    uiManager.setNightMode(UiModeManager.MODE_NIGHT_NO);
}
```

#### NightModeHelper 实现夜间模式（）

```
import java.lang.ref.WeakReference;

import android.app.Activity;
import android.content.SharedPreferences;
import android.content.res.Configuration;
import android.preference.PreferenceManager;

public class NightModeHelper {

	private static final String PREF_KEY = "nightModeState";
	
	private static int sUiNightMode = Configuration.UI_MODE_NIGHT_UNDEFINED;
	
	private WeakReference<Activity> mActivity;
	private SharedPreferences mPrefs;
	
	public NightModeHelper(Activity activity, int theme) {
		int currentMode = (activity.getResources().getConfiguration()
			.uiMode & Configuration.UI_MODE_NIGHT_MASK);
		mPrefs = PreferenceManager.getDefaultSharedPreferences(activity);
		init(activity, theme, mPrefs.getInt(PREF_KEY, currentMode));
	}
	
	public NightModeHelper(Activity activity, int theme, int defaultUiMode) {
		init(activity, theme, defaultUiMode);
	}
	
	private void init(Activity activity, int theme, int defaultUiMode) {
		mActivity = new WeakReference<Activity>(activity);
		if(sUiNightMode == Configuration.UI_MODE_NIGHT_UNDEFINED){
			sUiNightMode = defaultUiMode;
		}
		updateConfig(sUiNightMode);
		
	}
	
	private void updateConfig(int uiNightMode) {
		Activity activity = mActivity.get();
		if(activity == null){
			throw new IllegalStateException("Activity went away?");
		}
		Configuration newConfig = new Configuration(activity.getResources().getConfiguration());
		newConfig.uiMode &= ~Configuration.UI_MODE_NIGHT_MASK;
		newConfig.uiMode |= uiNightMode;
		activity.getResources().updateConfiguration(newConfig, null);
		sUiNightMode = uiNightMode;
		if(mPrefs != null){
			mPrefs.edit()
				.putInt(PREF_KEY, sUiNightMode)
				.apply();
		}
	}
	
	public static int getUiNightMode() {
		return sUiNightMode;
	}
	
	public void toggle() {
		if(sUiNightMode == Configuration.UI_MODE_NIGHT_YES){
			notNight();
		}
		else{
			night();
		}
	}
	
	public void notNight() {
		updateConfig(Configuration.UI_MODE_NIGHT_NO);
		mActivity.get().recreate();
	}
	
	public void night() {
		updateConfig(Configuration.UI_MODE_NIGHT_YES);
		mActivity.get().recreate();
	}
}
```

用这种方式实现夜间模式只需要定义一个NightModeHelper.java类，然后在需要实现夜间模式的Activity的onCreate()方法setContentView()方法hou加上下面一行代码即可：

```
mNightModeHelper = new NightModeHelper(this, R.style.AppTheme_Light);

```

#### AppCompatDelegate 实现夜间模式

```
//夜间模式
            SharedPreferences sp = this.getSharedPreferences("loonggg", MODE_PRIVATE);
            boolean isNight = sp.getBoolean("night", false);
            if (isNight) {
                AppCompatDelegate.setDefaultNightMode(AppCompatDelegate.MODE_NIGHT_NO);
                sp.edit().putBoolean("night", false).apply();
            } else {
                AppCompatDelegate.setDefaultNightMode(AppCompatDelegate.MODE_NIGHT_YES);
                sp.edit().putBoolean("night", true).apply();
            }
            recreate();
```

---

### 广告轮播的自定义实现

在大多数应用 APP 的主页面上都会出现广告轮播，或是推广自己的产品，或是为其它公司做广告推销，但不得不说，这样的插入也让整个 UI 界面的风格别有一番风味。下面我们来看一看网易云音乐的广告轮播的实现方式吧。

首先新建一个 Info.java 类用来保存图片的网址、设置的标题等信息并实现 set 和 get 方法，代码如下：

```
public class Info {

  private String url;
  private String title;

  public Info(String title, String url) {
      this.url = url;
      this.title = title;
  }

  public String getUrl() {
      return url;
  }

  public void setUrl(String url) {
      this.url = url;
  }

  public String getTitle() {
      return title;
  }

  public void setTitle(String title) {
      this.title = title;
  }
}
```

然后自定义轮播控件的布局，使用 ViewPager 来实现轮播效果，这里为了显示重叠的效果布局方式采用 RelativeLayout 即相对布局，然后嵌套两个 LinearLayout 分别用来存放指示器（在代码中动态添加）和显示标题。具体代码如下：

```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:layout_width="match_parent"
  android:layout_height="match_parent"
  android:background="@android:color/white"
  android:orientation="vertical">
  <android.support.v4.view.ViewPager
      android:id="@+id/cycle_view_pager"
      android:layout_width="match_parent"
      android:layout_height="match_parent" />
  <LinearLayout
      android:id="@+id/cycle_indicator"
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:layout_alignParentBottom="true"
      android:layout_marginBottom="10dp"
      android:gravity="center"
      android:orientation="horizontal" />
  <LinearLayout
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:layout_above="@id/cycle_indicator"
      android:orientation="vertical">
      <TextView
          android:id="@+id/cycle_title"
          android:layout_width="match_parent"
          android:layout_height="wrap_content"
          android:layout_marginBottom="10dp"
          android:gravity="center"
          android:textColor="@android:color/white"
          android:textSize="20sp" />
  </LinearLayout>
</RelativeLayout>
```

上面说到要在代码中动态添加指示器，所以需要创建一个自定义响应，新建 CycleViewPager.java 类

```
package com.androxue.coolcloud.widget;

import android.content.Context;
import android.os.Handler;
import android.os.Message;
import android.support.v4.view.PagerAdapter;
import android.support.v4.view.ViewPager;
import android.util.AttributeSet;
import android.util.Log;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.FrameLayout;
import android.widget.ImageView;
import android.widget.LinearLayout;
import android.widget.TextView;

import com.androxue.coolcloud.R;
import com.androxue.coolcloud.activity.MainActivity;
import com.androxue.coolcloud.info.Info;

import java.util.ArrayList;
import java.util.List;

/**
 * Created by JimCharles on 2017/3/19.
 */

public class CycleViewPager extends FrameLayout implements ViewPager.OnPageChangeListener {

    private static final String TAG = "CycleViewPager";
    private Context mContext;

    private ViewPager mViewPager;//实现轮播图的ViewPager

    private TextView mTitle;//标题

    private LinearLayout mIndicatorLayout; // 指示器

    private Handler handler;//每几秒后执行下一张的切换

    private int WHEEL = 100; // 转动

    private int WHEEL_WAIT = 101; // 等待

    private List<View> mViews = new ArrayList<>(); //需要轮播的View，数量为轮播图数量+2

    private ImageView[] mIndicators;    //指示器小圆点

    private boolean isScrolling = false; // 滚动框是否滚动着

    private boolean isCycle = true; // 是否循环，默认为true

    private boolean isWheel = true; // 是否轮播，默认为true

    private int delay = 4000; // 默认轮播时间

    private int mCurrentPosition = 0; // 轮播当前位置

    private long releaseTime = 0; // 手指松开、页面不滚动时间，防止手机松开后短时间进行切换

    private ViewPagerAdapter mAdapter;

    private ImageCycleViewListener mImageCycleViewListener;

    private List<Info> infos;//数据集合

    private int mIndicatorSelected;//指示器图片，被选择状态

    private int mIndicatorUnselected;//指示器图片，未被选择状态

    final Runnable runnable = new Runnable() {

        @Override
        public void run() {
            if (mContext != null && isWheel) {
                long now = System.currentTimeMillis();
                // 检测上一次滑动时间与本次之间是否有触击(手滑动)操作，有的话等待下次轮播
                if (now - releaseTime > delay - 500) {
                    handler.sendEmptyMessage(WHEEL);
                } else {
                    handler.sendEmptyMessage(WHEEL_WAIT);
                }
            }
        }
    };

    public CycleViewPager(Context context) {
        this(context, null);
    }

    public CycleViewPager(Context context, AttributeSet attrs) {
        this(context, attrs, 0);
    }

    public CycleViewPager(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        this.mContext = context;
        initView();
    }

    /**
     * 初始化View
     */
    private void initView() {
        LayoutInflater.from(mContext).inflate(R.layout.layout_circle_view, this, true);
        mViewPager = (ViewPager) findViewById(R.id.cycle_view_pager);
        mTitle = (TextView) findViewById(R.id.cycle_title);
        mIndicatorLayout = (LinearLayout) findViewById(R.id.cycle_indicator);

        handler = new Handler() {
            @Override
            public void handleMessage(Message msg) {
                super.handleMessage(msg);
                if (msg.what == WHEEL && mViews.size() > 0) {
                    if (!isScrolling) {
                        //当前为非滚动状态，切换到下一页
                        int posttion = (mCurrentPosition + 1) % mViews.size();
                        mViewPager.setCurrentItem(posttion, true);
                    }
                    releaseTime = System.currentTimeMillis();
                    handler.removeCallbacks(runnable);
                    handler.postDelayed(runnable, delay);
                    return;

                }
                if (msg.what == WHEEL_WAIT && mViews.size() > 0) {
                    handler.removeCallbacks(runnable);
                    handler.postDelayed(runnable, delay);
                }
            }
        };
    }

    /**
     * 设置指示器图片，在setData之前调用
     * @param select   选中时的图片
     * @param unselect 未选中时的图片
     */
    public void setIndicators(int select, int unselect) {
        mIndicatorSelected = select;
        mIndicatorUnselected = unselect;
    }

    public void setData(List<Info> list, ImageCycleViewListener listener) {
        setData(list, listener, 0);
    }


    /**
     * 初始化viewpager
     * @param list         要显示的数据
     * @param showPosition 默认显示位置
     */
    public void setData(List<Info> list, ImageCycleViewListener listener, int showPosition) {

        if (list == null || list.size() == 0) {
            //没有数据时隐藏整个布局
            this.setVisibility(View.GONE);
            return;
        }

        mViews.clear();
        infos = list;

        if (isCycle) {
            //添加轮播图View，数量为集合数+2
            // 将最后一个View添加进来
            mViews.add(getImageView(mContext, infos.get(infos.size() - 1).getUrl()));
            for (int i = 0; i < infos.size(); i++) {
                mViews.add(getImageView(mContext, infos.get(i).getUrl()));
            }
            // 将第一个View添加进来
            mViews.add(getImageView(mContext, infos.get(0).getUrl()));
        } else {
            //只添加对应数量的View
            for (int i = 0; i < infos.size(); i++) {
                mViews.add(getImageView(mContext, infos.get(i).getUrl()));
            }
        }


        if (mViews == null || mViews.size() == 0) {
            //没有View时隐藏整个布局
            this.setVisibility(View.GONE);
            return;
        }

        mImageCycleViewListener = listener;

        int ivSize = mViews.size();

        // 设置指示器
        mIndicators = new ImageView[ivSize];
        if (isCycle)
            mIndicators = new ImageView[ivSize - 2];
        mIndicatorLayout.removeAllViews();
        for (int i = 0; i < mIndicators.length; i++) {
            mIndicators[i] = new ImageView(mContext);
            LinearLayout.LayoutParams lp = new LinearLayout.LayoutParams(
                    LinearLayout.LayoutParams.WRAP_CONTENT, LinearLayout.LayoutParams.WRAP_CONTENT);
            lp.setMargins(10, 0, 10, 0);
            mIndicators[i].setLayoutParams(lp);
            mIndicatorLayout.addView(mIndicators[i]);
        }

        mAdapter = new ViewPagerAdapter();

        // 默认指向第一项，下方viewPager.setCurrentItem将触发重新计算指示器指向
        setIndicator(0);

        mViewPager.setOffscreenPageLimit(3);
        mViewPager.setOnPageChangeListener(this);
        mViewPager.setAdapter(mAdapter);
        if (showPosition < 0 || showPosition >= mViews.size())
            showPosition = 0;
        if (isCycle) {
            showPosition = showPosition + 1;
        }
        mViewPager.setCurrentItem(showPosition);

        setWheel(true);//设置轮播
    }

    /**
     * 获取轮播图View
     */
    private View getImageView(Context context, String url) {
        return MainActivity.getImageView(context, url);
    }

    /**
     * 设置指示器，和文字内容
     * @param selectedPosition 默认指示器位置
     */
    private void setIndicator(int selectedPosition) {
        setText(mTitle, infos.get(selectedPosition).getTitle());
        try {

            for (int i = 0; i < mIndicators.length; i++) {
                mIndicators[i]
                        .setBackgroundResource(mIndicatorUnselected);
            }
            if (mIndicators.length > selectedPosition)
                mIndicators[selectedPosition]
                        .setBackgroundResource(mIndicatorSelected);
        } catch (Exception e) {
            Log.i(TAG, "指示器路径不正确");
        }
    }

    /**
     * 页面适配器 返回对应的view
     */
    private class ViewPagerAdapter extends PagerAdapter {

        @Override
        public int getCount() {
            return mViews.size();
        }

        @Override
        public boolean isViewFromObject(View arg0, Object arg1) {
            return arg0 == arg1;
        }

        @Override
        public void destroyItem(ViewGroup container, int position, Object object) {
            container.removeView((View) object);
        }

        @Override
        public View instantiateItem(ViewGroup container, final int position) {
            View v = mViews.get(position);
            if (mImageCycleViewListener != null) {
                v.setOnClickListener(new OnClickListener() {

                    @Override
                    public void onClick(View v) {
                        mImageCycleViewListener.onImageClick(infos.get(mCurrentPosition - 1), mCurrentPosition, v);
                    }
                });
            }
            container.addView(v);
            return v;
        }

        @Override
        public int getItemPosition(Object object) {
            return POSITION_NONE;
        }
    }

    @Override
    public void onPageScrolled(int position, float positionOffset, int positionOffsetPixels) {

    }

    @Override
    public void onPageSelected(int arg0) {
        int max = mViews.size() - 1;
        int position = arg0;
        mCurrentPosition = arg0;
        if (isCycle) {
            if (arg0 == 0) {

                //滚动到mView的1个（界面上的最后一个），将mCurrentPosition设置为max - 1
                mCurrentPosition = max - 1;
            } else if (arg0 == max) {
                //滚动到mView的最后一个（界面上的第一个），将mCurrentPosition设置为1
                mCurrentPosition = 1;
            }
            position = mCurrentPosition - 1;
        }
        setIndicator(position);
    }

    @Override
    public void onPageScrollStateChanged(int state) {
        if (state == 1) { // viewPager在滚动
            isScrolling = true;
            return;
        } else if (state == 0) { // viewPager滚动结束

            releaseTime = System.currentTimeMillis();
            //跳转到第mCurrentPosition个页面（没有动画效果，实际效果页面上没变化）
            mViewPager.setCurrentItem(mCurrentPosition, false);

        }
        isScrolling = false;
    }

    /**
     * 为textview设置文字
     */
    public static void setText(TextView textView, String text) {
        if (text != null && textView != null) textView.setText(text);
    }

    /**
     * 为textview设置文字
     */
    public static void setText(TextView textView, int text) {
        if (textView != null) setText(textView, text + "");
    }

    /**
     * 是否循环，默认开启。必须在setData前调用
     */
    public void setCycle(boolean isCycle) {
        this.isCycle = isCycle;
    }


    /**
     * 是否处于循环状态
     */
    public boolean isCycle() {
        return isCycle;
    }

    /**
     * 设置是否轮播，默认轮播,轮播一定是循环的 
     */
    public void setWheel(boolean isWheel) {
        this.isWheel = isWheel;
        isCycle = true;
        if (isWheel) {
            handler.postDelayed(runnable, delay);
        }
    }


    /**
     * 刷新数据，当外部视图更新后，通知刷新数据
     */
    public void refreshData() {
        if (mAdapter != null)
            mAdapter.notifyDataSetChanged();
    }

    /**
     * 是否处于轮播状态
     */
    public boolean isWheel() {
        return isWheel;
    }

    /**
     * 设置轮播暂停时间,单位毫秒（默认4000毫秒）
     * @param delay
     */
    public void setDelay(int delay) {
        this.delay = delay;
    }

    /**
     * 轮播控件的监听事件
     */
    public static interface ImageCycleViewListener {

        /**
         * 单击图片事件
         */
        public void onImageClick(Info info, int position, View imageView);
    }
}

```

然后再主页面中UI中引用自定义的 CycleViewPager 控件并在 Activity 中进行对应操作即可，具体代码如下：

```
<com.androxue.coolcloud.widget.CycleViewPager
        android:id="@+id/cycle_view"
        android:layout_width="395dp"
        android:layout_height="160dp"
        tools:layout_editor_absoluteY="0dp"
        tools:layout_editor_absoluteX="8dp" />
```

```
public class MainActivity extends AppCompatActivity {

  /**
   * 模拟请求后得到的数据
   */
  List<Info> mList = new ArrayList<>();

  /**
   * 轮播图
   */
  CycleViewPager mCycleViewPager;

  @Override
  protected void onCreate(Bundle savedInstanceState) {
      super.onCreate(savedInstanceState);
      setContentView(R.layout.activity_main);
      initData();
      initView();
  }

  /**
   * 初始化数据
   */
  private void initData() {
      mList.add(new Info("标题1", 
            "http://img2.3lian.com/2014/c7/25/d/40.jpg"));
      mList.add(new Info("标题2", 
            "http://img2.3lian.com/2014/c7/25/d/41.jpg"));
      mList.add(new Info("标题3", 
           "http://imgsrc.baidu.com/forum/pic/item/b64543a98226cffc8872e00cb9014a90f603ea30.jpg"));
      mList.add(new Info("标题4", 
           "http://imgsrc.baidu.com/forum/pic/item/261bee0a19d8bc3e6db92913828ba61eaad345d4.jpg"));
  }

  /**
   * 初始化View
   */
  private void initView() {
      mCycleViewPager = (CycleViewPager) findViewById(R.id.cycle_view);
      //设置选中和未选中时的图片
      mCycleViewPager.setIndicators(R.mipmap.ad_select, R.mipmap.ad_unselect);
      //设置轮播间隔时间
      mCycleViewPager.setDelay(2000);
      mCycleViewPager.setData(mList, mAdCycleViewListener);
  }

  /**
   * 轮播图点击监听
   */
  private CycleViewPager.ImageCycleViewListener mAdCycleViewListener = 
                new CycleViewPager.ImageCycleViewListener() {

      @Override
      public void onImageClick(Info info, int position, View imageView) {

          if (mCycleViewPager.isCycle()) {
              position = position - 1;
          }
          Toast.makeText(MainActivity.this, info.getTitle() +
               "选择了--" + position, Toast.LENGTH_LONG).show();
      }
  };
}
```

至此广告轮播功能已经实现了，有兴趣的朋友自己码一下。

---

对于酷云这个项目，想了很久，到底是做到最好，还是能用就好，在我看来，完成这个项目的难度还是有的，加上其他限制，导致进度缓慢，不过我觉得还是要将它呈现出来，所以决定写下酷云开发过程的一系列文章，希望能和大家一起进步。如果有更好的方法实现或者有错误，希望您在评论区能指出，我会及时更改。

---

### Http Post 实现网络请求、数据提交

使用 Http 协议来对服务器接口进行请求，请求方式为 Post，请求方法包括 http请求头（cookie、referer）

```
protected boolean sendHttpPostRequest(String request) throws IOException {
        HttpURLConnection connection = null;
        try{
            String httpUrl = "http://music.163.com/api/search/pc/";
            URL url = new URL(httpUrl);
            connection = (HttpURLConnection) url.openConnection();
            //设置http请求方法
            connection.setDoOutput(true);
            connection.setDoInput(true);
            connection.setRequestMethod("POST");//设置请求方式为post
            connection.setReadTimeout(5000);//设置读取超时为5秒
            connection.setConnectTimeout(10000);//设置连接超时为10秒
            //设置请求头，包括Cookie、refer、Charset
            connection.setRequestProperty("os", "pc");
            connection.setRequestProperty("MUSIC_U", "5339640232");
            connection.setRequestProperty("Charset", "utf-8");
            //进行连接
            connection.connect();
            DataOutputStream out = new DataOutputStream(connection.getOutputStream());
            //请求参数
            String data = "&s=" + URLEncoder.encode(request, "UTF-8")+ "&limit=" + URLEncoder.encode("10", "UTF-8")+ "&type="
                    + URLEncoder.encode("1", "UTF-8") + "&offset" + URLEncoder.encode("0", "UTF-8") + "&total=" + URLEncoder.encode("true", "UTF-8");
            //通过EdirText获取输入内容进行搜索；因为手机屏幕原因，将搜索返回数目控制为10条；搜索形式为单曲搜索；偏移量设置为0，即不进行偏移

            //获取输出流
            out.writeBytes(data);
            out.flush();
            out.close();
            //获取响应输入流对象
            if (connection.getResponseCode() == 200) {
                InputStreamReader is = new InputStreamReader(connection.getInputStream());
                BufferedReader bufferedReader = new BufferedReader(is);
                StringBuilder strBuffer = new StringBuilder();
                String line;
                //读取服务器返回信息
                while ((line = bufferedReader.readLine()) != null){
                    strBuffer.append(line);
                }
                result = strBuffer.toString();
                bufferedReader.close();
                is.close();
            }
        }catch (IOException e) {
            return true;
        } finally {
            if (connection != null) {
                connection.disconnect();
            }
        }
        return false;
    }
```

以上通过 http post 方法进行网络请求并向服务器提交请求数据，代码注释已经解释得很清楚了，就不再多做解释。

因为 Android 4.0 以后要求必须在子线程中进行网络请求，所以我们在子线程中调用 Http 请求方法和耗时操作。

```
private void search(){
        fatherspoetry.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                final Handler handler = new Handler() {
                    @Override
                    public void handleMessage(Message msg) {
                        if (msg.what == 1) {
                            //提示读取结果
                            Toast.makeText(SearchActivity.this, result, Toast.LENGTH_LONG).show();
                        }
                    }
                };
                new Thread() {
                    public void run() {
                        //请求网络
                        try {
                            sendHttpPostRequest(request);
                        } catch (IOException e) {
                            e.printStackTrace();
                        }
                        Message m = new Message();
                        m.what = 1;
                        // 发送消息到Handler
                        handler.sendMessage(m);
                    }
                }.start();
            }
        });
    }
```

### JSON 数据解析

接下来要做的就是解析服务器返回信息，一般返回的数据类型为 xml 或 json 格式，此处返回的为 json 格式，所以只讲解 json 的解析方式。

json 的解析方法有很多种，这里之讲解两种常用的，没错，就是 Java 官方提供的 org.json 和 Google 开源的  Gson 了。

首先要介绍一下 json 的数据格式，有 json 对象（JSONObject）和 json 数组（JSONArray）,对于两者的区别，我们先来看一段 json 数据。

> { "t_id": 64, "user_name": "建新", "user_number": 13710675396, "user_cardid": 639039431,  "user_passwd": "hhhhhh","user_money": 9999999999}

那么很明显上面是一段仅由 json 对象构成的 json 数据，解析起来很方便，使用 JSONObject 解析的代码如下

```
public void parseJson(String json) throws JSONException {
        JSONObject jsonObject = new JSONObject(json);
        String password = jsonObject.getString("user_passwd");
        String nickName = jsonObject.getString("user_name");
        int tId= jsonObject.getInt("t_id");
        int cardId = jsonObject.getInt("user_cardid");
        int money = jsonObject.getInt("user_money");
    }

```

但是当遇上较负责的 json 数据时可能你有点晕，如下面的 json 数据，对象中有数组，数组中有对象

> {"result":{"songs":[{"name":"玫瑰色的你","id":326695,"position":1,"alias":[],"status":0,"fee":8,"copyrightId":7001,"disc":"","no":1,"artists":[{"name":"张悬","id":10557,"picId":0,"img1v1Id":0,"briefDesc":"","picUrl":"http://p3.music.126.net/6y-UleORITEDbvrOLV0Q8A==/5639395138885805.jpg","img1v1Url":"http://p4.music.126.net/6y-UleORITEDbvrOLV0Q8A==/5639395138885805.jpg","albumSize":0,"alias":[],"trans":"","musicSize":0}],"album":{"name":"神的游戏","id":32311,"type":"专辑","size":10,"picId":18808245906527670,"blurPicUrl":"http://p4.music.126.net/klOSGBRQhevtM6c9RXrM1A==/18808245906527670.jpg","companyId":0,"pic":18808245906527670,"picUrl":"http://p4.music.126.net/klOSGBRQhevtM6c9RXrM1A==/18808245906527670.jpg","publishTime":1344528000000,"description":"","tags":"","company":"索尼音乐娱乐","briefDesc":"","artist":{"name":"","id":0,"picId":0,"img1v1Id":0,"briefDesc":"","picUrl":"http://p4.music.126.net/6y-UleORITEDbvrOLV0Q8A==/5639395138885805.jpg","img1v1Url":"http://p4.music.126.net/6y-UleORITEDbvrOLV0Q8A==/5639395138885805.jpg","albumSize":0,"alias":[],"trans":"","musicSize":0},"songs":[],"alias":["Games We Play"],"status":3,"copyrightId":7001,"commentThreadId":"R_AL_3_32311","artists":[{"name":"张悬","id":10557,"picId":0,"img1v1Id":0,"briefDesc":"","picUrl":"http://p3.music.126.net/6y-UleORITEDbvrOLV0Q8A==/5639395138885805.jpg","img1v1Url":"http://p4.music.126.net/6y-UleORITEDbvrOLV0Q8A==/5639395138885805.jpg","albumSize":0,"alias":[],"trans":"","musicSize":0}],"picId_str":"18808245906527670"},"starred":false,"popularity":100.0,"score":100,"starredNum":0,"duration":297613,"playedNum":0,"dayPlays":0,"hearTime":0,"ringtone":"600902000009458622","crbt":"b957432baaac42d8096f76ae357992ce","audition":null,"copyFrom":"","commentThreadId":"R_SO_4_326695","rtUrl":null,"ftype":0,"rtUrls":[],"copyright":1,"rtype":0,"rurl":null,"mvid":5102,"bMusic":{"name":null,"id":1225536788,"size":3572342,"extension":"mp3","sr":44100,"dfsId":1423867572222808,"bitrate":96000,"playTime":297613,"volumeDelta":-0.87},"mp3Url":"http://m2.music.126.net/mO4G5dQ7WPm3Le0lAy9efg==/1423867572222808.mp3","hMusic":{"name":null,"id":1225536786,"size":11907701,"extension":"mp3","sr":44100,"dfsId":3441471403612879,"bitrate":320000,"playTime":297613,"volumeDelta":-1.27},"mMusic":{"name":null,"id":1225536787,"size":5953873,"extension":"mp3","sr":44100,"dfsId":3441471403612880,"bitrate":160000,"playTime":297613,"volumeDelta":-0.84},"lMusic":{"name":null,"id":1225536788,"size":3572342,"extension":"mp3","sr":44100,"dfsId":1423867572222808,"bitrate":96000,"playTime":297613,"volumeDelta":-0.87}}],"songCount":6},"code":200}

看着就晕是不是，而且上面还是限制返回一首歌曲时返回的数据，但通常我们返回但是几十首，其实我们可以利用 [json 工具](http://www.bejson.com/jsonviewernew/)来简化它，简化后通过视图来查看，效果如下：

> {
  "result": {
    "songs": [
      {
        "name": "Safe And Sound",
        "id": 2001472,
        "position": 1,
        "alias": [
      
        ],
        "status": 0,
        "fee": 1,
        "copyrightId": 7003,
        "disc": "",
        "no": 1,
        "artists": [
          {
            "name": "Taylor Swift",
            "id": 44266,
            "picId": 0,
            "img1v1Id": 0,
            "briefDesc": "",
            "picUrl": "http://p4.music.126.net/6y-UleORITEDbvrOLV0Q8A==/5639395138885805.jpg",
            "img1v1Url": "http://p3.music.126.net/6y-UleORITEDbvrOLV0Q8A==/5639395138885805.jpg",
            "albumSize": 0,
            "alias": [
              
            ],
            "trans": "",
            "musicSize": 0
          }
        ],
        "album": {
          "name": "Safe & Sound",
          "id": 201785,
          "type": "EP/Single",
          "size": 1,
          "picId": 802643488329098,
          "blurPicUrl": "http://p3.music.126.net/7PejdCyItA2IRxDlbS_Nug==/802643488329098.jpg",
          "companyId": 0,
          "pic": 802643488329098,
          "picUrl": "http://p3.music.126.net/7PejdCyItA2IRxDlbS_Nug==/802643488329098.jpg",
          "publishTime": 1324569600007,
          "description": "",
          "tags": "",
          "company": "LionGate",
          "briefDesc": "",
          "artist": {
            "name": "",
            "id": 0,
            "picId": 0,
            "img1v1Id": 0,
            "briefDesc": "",
            "picUrl": "http://p3.music.126.net/6y-UleORITEDbvrOLV0Q8A==/5639395138885805.jpg",
            "img1v1Url": "http://p3.music.126.net/6y-UleORITEDbvrOLV0Q8A==/5639395138885805.jpg",
            "albumSize": 0,
            "alias": [
              
            ],
            "trans": "",
            "musicSize": 0
          },
          "songs": [
            
          ],
          "alias": [
            
          ],
          "status": -1,
          "copyrightId": 5003,
          "commentThreadId": "R_AL_3_201785",
          "artists": [
            {
              "name": "Taylor Swift",
              "id": 44266,
              "picId": 0,
              "img1v1Id": 0,
              "briefDesc": "",
              "picUrl": "http://p3.music.126.net/6y-UleORITEDbvrOLV0Q8A==/5639395138885805.jpg",
              "img1v1Url": "http://p4.music.126.net/6y-UleORITEDbvrOLV0Q8A==/5639395138885805.jpg",
              "albumSize": 0,
              "alias": [
                
              ],
              "trans": "",
              "musicSize": 0
            }
          ]
        },
        "starred": false,
        "popularity": 100.0,
        "score": 100,
        "starredNum": 0,
        "duration": 242129,
        "playedNum": 0,
        "dayPlays": 0,
        "hearTime": 0,
        "ringtone": "",
        "crbt": null,
        "audition": null,
        "copyFrom": "",
        "commentThreadId": "R_SO_4_2001472",
        "rtUrl": null,
        "ftype": 0,
        "rtUrls": [
          
        ],
        "copyright": 0,
        "hMusic": {
          "name": "Safe And Sound",
          "id": 21938700,
          "size": 9702842,
          "extension": "mp3",
          "sr": 44100,
          "dfsId": 2056086743957784,
          "bitrate": 320000,
          "playTime": 242129,
          "volumeDelta": 0.0483791
        },
        "mMusic": {
          "name": "Safe And Sound",
          "id": 21938701,
          "size": 4864442,
          "extension": "mp3",
          "sr": 44100,
          "dfsId": 2100067209069027,
          "bitrate": 160000,
          "playTime": 242129,
          "volumeDelta": 0.234826
        },
        "lMusic": {
          "name": "Safe And Sound",
          "id": 21938702,
          "size": 2928664,
          "extension": "mp3",
          "sr": 44100,
          "dfsId": 2076977464885633,
          "bitrate": 96000,
          "playTime": 242129,
          "volumeDelta": 0.0395871
        },
        "bMusic": {
          "name": "Safe And Sound",
          "id": 21938702,
          "size": 2928664,
          "extension": "mp3",
          "sr": 44100,
          "dfsId": 2076977464885633,
          "bitrate": 96000,
          "playTime": 242129,
          "volumeDelta": 0.0395871
        },
        "rtype": 0,
        "rurl": null,
        "mvid": 5466,
        "mp3Url": "http://m2.music.126.net/tbBk7yuG8Ow5MZDNlYbtrA==/2076977464885633.mp3"
      }
    ],
    "songCount": 197
  },
  "code": 200
}

看起来很多，但其实已经很清晰了，例如我们现在需要获取 mp3Url 这个数据，该如何去解析它呢？我们通过观察整个的 json 视图发现， mp3Url 这个数据有着以下的嵌套，result 对象 --> songs 数组 --> mp3Url 对象。既然理清了关系，那解析就很简单了，代码如下：

```
public void parseJson(String json) throws JSONException {
        JSONObject jsonObject = new JSONObject(json);
        JSONObject result = jsonObject.getJSONObject("result");
        JSONArray jsonArray = result.getJSONArray("songs");
        for (int i = 0; i < jsonArray.length(); i++) {
            JSONObject jsonObject1 = jsonArray.getJSONObject(i);
            mp3Url = jsonObject1.getString("mp3Url");
        }
    }
```

以上但是通过使用 org.json 来解析数据，这很方便，你只需要分析一下返回的数据，传入返回 json 数据后分别对 json 对象和 json 数组进行解析即可。但其实还有更简单的方法，那就是 Gson 了，让我们来一起看一下。

以第一段 json 数据为例，先创建一个 JavaBean 来获取 json 内的数据。代码如下：

```
public class Json {

	private String name;

	private String password;
	
    private int tId; 
    
	private int cardId; 

	private int money; 

    
    public void setName(String name){
        this.name = name;
    }

	 public String getName(){
        return this.name;
    }

	public void setPassword(String password){
        this.password = password;
    }

	 public String getPassword(){
        return this.password;
    }

	public void setTId(int tId){
        this.tId = tId;
    }

	 public int getTId(){
        return this.tId;
    }

	public void setCardId(int cardId){
        this.cardId = cardId;
    }

	 public int getCardId(){
        return this.cardId;
    }

	public void setMoney(int money){
        this.money = money;
    }

	 public int getMoney(){
        return this.money;
    }
}
```

导入 Gson 包

```
compile 'com.google.code.gson:gson:2.7'
```

利用 Gson 解析 json

```
Gson gson = new Gson();
Result result = gson.fromJson(jsonData, Result.class);
int tId = result.getTId();
```

没错，就是这么简单，关于 json 数据解析就讲到这里。

---

### MwdiaPlayer 获取、设置歌曲信息

[MwdiaPlayer](https://developer.android.com/reference/android/media/MediaPlayer.html) 是 Google 提供的用来控制 Android 多媒体播放的封装类，我们来看一下 Google 官方文档对这个类·的描述：

> MediaPlayer class can be used to control playback of audio/video files and streams. An example on how to use the methods in this class can be found in VideoView.

这段话的意思也就是说：

> MediaPlayer 类可用于控制音频/视频文件和流的播放。在 VideoView 中可以找到有关如何使用此类中的方法的示例。

也就是说我们可以通过 MediaPlayer 来播放音乐和视频，并且进行流操作。我们可以看到在对 MediaPlayer 的解释中提到了 [VideoView](https://developer.android.com/reference/android/widget/VideoView.html) ，很明显这是一个控件，但是它到底有什么用呢！这里我们就不做深入探究了，因为用不到，有兴趣的小伙伴可以自己看一下。

接下来介绍 MediaPlayer 的用法

 1. 创建 MediaPlayer 对象

```
/*
**这里有两种方式
*/
1. 通过 new 创建
MediaPlayer mp = new MediaPlayer();
2. 通过 create() 方法插件
MediaPlayer mp = MediaPlayer.create(this, R.raw.test);
```

这里需要注意一点，通过  new 创建对象还需要调用 setDataSource()方法来设置资源的路径，而通过 create 创建对象则无需再调用 setDataSource()方法，可以在方法内传入资源文件。

 2. 常用方法介绍

```
1. setLooping(boolean looping)//设置是否循环播放
2. setNextMediaPlayer(MediaPlayer next)//设置当前流媒体播放完毕,下一个播放的MediaPlayer
3. setVolume(float leftVolume, float rightVolume)//设置播放器的音量
4. start()//开始或恢复播放
5. stop()//停止播放
6. pause()//暂停播放
7. prepare()//准备播放（装载音频），调用此方法会使MediaPlayer进入Prepared状态
8. release()//释放媒体资源
9. reset()//重置MediaPlayer进入未初始化状态
10. seekTo(int msec)//指定的时间位置 
11. getCurrentPosition()//获取当前播放的位置
12. isLooping()//判断MediaPlayer是否正在循环播放
13. isPlaying()//判断MediaPlayer是否正在播放
```

 3. 播放本地音乐

```
public void play() {
		mMediaPlayer = MediaPlayer.create("/sdcard/a.mp3");
        mMediaPlayer.setAudioStreamType(AudioManager.STREAM_MUSIC);
        // 通过异步的方式装载媒体资源,避免UI卡顿
        mMediaPlayer.prepareAsync();
        mMediaPlayer.setOnPreparedListener(new MediaPlayer.OnPreparedListener() {
            @Override
            public void onPrepared(MediaPlayer mp) {
                mMediaPlayer.start();
            }
        });
}
```

别忘了添加访问内存卡的权限

```
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/> 
```

 4. 播放在线音乐

```
public void play() {
        mMediaPlayer = MediaPlayer.create(this, Uri.parse("http://m2.music.126.net/tbBk7yuG8Ow5MZDNlYbtrA==/2076977464885633.mp3"));
        mMediaPlayer.setAudioStreamType(AudioManager.STREAM_MUSIC);
        // 通过异步的方式装载媒体资源,避免UI卡顿
        mMediaPlayer.prepareAsync();
        mMediaPlayer.setOnPreparedListener(new MediaPlayer.OnPreparedListener() {
            @Override
            public void onPrepared(MediaPlayer mp) {
                mMediaPlayer.start();
            }
        });
    }
```

亦或者你想偷懒的话，可以这么写：

```
protected void playMusic() {
        mMediaPlayer = MediaPlayer.create(this,Uri.parse(mp3Url));
        mMediaPlayer.start();
    }
```

需要注意此处 mp3Url 为通过解析 json 数据获取的播放地址。

 5. 释放资源

正如前面所说的一样，MediaPlayer 是非常耗费系统资源的，所以我们需要做好资源回收工作，而最好的方法就是去主动回收了！通过重写 Activity 的 onDestroy() 方法并实现资源释放。

```
protected void onDestroy(){
        if (mMediaPlayer != null && mMediaPlayer.isPlaying()) {
            mMediaPlayer.stop();
            mMediaPlayer.release();
            mMediaPlayer = null;
        }
        super.onDestroy();
    }
```

这里要注意的一个问题在使用 start() 播放流媒体之前，需要装载流媒体资源，这是非常耗费系统资源的，所以最好使用 prepareAsync() 异步的方式装载流媒体资源，以避免UI卡顿。

别忘了添加访问的网络权限

```
<uses-permission android:name="android.permission.INTERNET"/>
```

 6. 单曲循环的实现

关于单曲循环的实现推荐使用以下两种方式，一是通过调用 setLooping() 方法实现，另一种是通过注册回调函数来实现，回调函数 setOnCompletionListener() 会在播放完后回调。

通过调用 setLooping() 方法实现

```
mMediaPlayer.setLooping(loop);
```

通过回调 setOnCompletionListener() 方法实现

```
mMediaPlayer.setOnCompletionListener(new MediaPlayer.OnCompletionListener() {
                    @Override
                    public void onCompletion(MediaPlayer mp) {
                        play();
                    }
                });
```

 7. 错误处理

因为 MediaPlayer 操作的是一个流媒体，所以无可避免的会出现一些错误，对于一段流媒体资源，前半段可以正常播放，而中间一段因为解析或者源文件错误等问题，造成中间一段无法播放问题，需要我们处理这个错误，否则会影响用户体验。可以为 MediaPlayer 注册回调函数 setOnErrorListener() 来设置出错之后的解决办法，一般重新播放或者播放下一个流媒体即可。

```
mMediaPlayer.setOnErrorListener(new MediaPlayer.OnErrorListener() {
                    @Override
                    public boolean onError(MediaPlayer mp, int what, int extra) {
                        play();
                        return false;
                    }
                });
```

当然了，这里播放进度和播放音量都可以通过手势来获取和更改，可以看我 [Android 自定义 View 之对 TouchEvent](http://blog.csdn.net/jim__charles/article/details/54691483) 的处理这一篇文章，就不再具体说明了，通过 setVolume() 和 seekTo() 方法即可实现。

---

### EditText 实现搜索框

我们几乎可以在所有应用里都能找到搜索框，且一般都在主页的 ToolBar 上来实现，虽然实现方式和 UI 规划都不一样，如网易云的搜索框

![这里写图片描述](http://img.blog.csdn.net/20170417100507489?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamltX19jaGFybGVz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

关于搜索框的实现主要有以下几点需要注意：

 1. 输入文字以后显示删除全部输入文字的删除键 
 
 2. 监听软键盘回车按钮并设置为搜索按钮 

 3. 监听输入框状态，实现搜索操作

下面对以上几点进行具体分析，首先通过自定义 View 来实现带删除键的搜索框，具体代码如下：

```
package com.androxue.coolcloud.widget;

import android.content.Context;
import android.graphics.drawable.Drawable;
import android.support.v7.widget.AppCompatEditText;
import android.text.Editable;
import android.text.TextWatcher;
import android.util.AttributeSet;
import android.view.MotionEvent;
import android.view.View;
import android.view.animation.Animation;
import android.view.animation.CycleInterpolator;
import android.view.animation.TranslateAnimation;

public class DeletableEditText extends AppCompatEditText {

    private Drawable mRightDrawable;
    private boolean isHasFocus;

    public DeletableEditText(Context context) {
        super(context);
        init();
    }

    public DeletableEditText(Context context, AttributeSet attrs) {
        super(context, attrs);
        init();
    }

    public DeletableEditText(Context context, AttributeSet attrs, int defStyle) {
        super(context, attrs, defStyle);
        init();
    }

    private void init() {
        // getCompoundDrawables:
        // Returns drawables for the left, top, right, and bottom borders.
        Drawable[] drawables = this.getCompoundDrawables();

        // 取得right位置的Drawable
        // 即我们在布局文件中设置的android:drawableRight
        mRightDrawable = drawables[2];

        // 设置焦点变化的监听
        this.setOnFocusChangeListener(new FocusChangeListenerImpl());
        // 设置EditText文字变化的监听
        this.addTextChangedListener(new TextWatcherImpl());
        // 初始化时让右边clean图标不可见
        setClearDrawableVisible(false);
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        switch (event.getAction()) {
            case MotionEvent.ACTION_UP:

                boolean isClean = (event.getX() > (getWidth() - getTotalPaddingRight()))
                        && (event.getX() < (getWidth() - getPaddingRight()));
                if (isClean) {
                    setText("");
                }
                break;

            default:
                break;
        }
        return super.onTouchEvent(event);
    }

    private class FocusChangeListenerImpl implements OnFocusChangeListener {
        @Override
        public void onFocusChange(View v, boolean hasFocus) {
            isHasFocus = hasFocus;
            if (isHasFocus) {
                boolean isVisible = getText().toString().length() >= 1;
                setClearDrawableVisible(isVisible);
            } else {
                setClearDrawableVisible(false);
            }
        }

    }

    // 当输入结束后判断是否显示右边clean的图标
    private class TextWatcherImpl implements TextWatcher {
        @Override
        public void afterTextChanged(Editable s) {
            boolean isVisible = getText().toString().length() >= 1;
            setClearDrawableVisible(isVisible);
        }

        @Override
        public void beforeTextChanged(CharSequence s, int start, int count,
                                      int after) {

        }

        @Override
        public void onTextChanged(CharSequence s, int start, int before,
                                  int count) {

        }

    }

    // 隐藏或显示右边clean的图标
    protected void setClearDrawableVisible(boolean isVisible) {
        Drawable rightDrawable;
        if (isVisible) {
            rightDrawable = mRightDrawable;
        } else {
            rightDrawable = null;
        }
        // 使用代码设置该控件left, top, right, and bottom处的图标
        setCompoundDrawables(getCompoundDrawables()[0],
                getCompoundDrawables()[1], rightDrawable,
                getCompoundDrawables()[3]);
    }

    // 显示动画
    public void setShakeAnimation() {
        this.startAnimation(shakeAnimation(5));

    }

    // CycleTimes动画重复的次数
    public Animation shakeAnimation(int CycleTimes) {
        Animation translateAnimation = new TranslateAnimation(0, 10, 0, 10);
        translateAnimation.setInterpolator(new CycleInterpolator(CycleTimes));
        translateAnimation.setDuration(1000);
        return translateAnimation;
    }
}

```

布局界面代码如下：

```
<com.androxue.coolcloud.widget.DeletableEditText
                android:id="@+id/search"
                android:layout_width="310dp"
                android:layout_height="wrap_content"
                android:drawableRight="@drawable/ic_delete"
                android:ems="10"
                android:hint="@string/search"
                android:imeOptions="actionSearch"
                android:inputType="text"
                android:singleLine="true" />
```

---

需要注意的是这里后面三个属性必须要有。

然后就是为搜索框添加监听器监听软键盘回车按钮并设置为搜索按钮了，直接上代码：

```
search.setOnKeyListener(new View.OnKeyListener() {//修改回车键功能
            @Override
            public boolean onKey(View v, int keyCode, KeyEvent event) {
                if (keyCode == KeyEvent.KEYCODE_ENTER && event.getAction() == KeyEvent.ACTION_DOWN) {
                    search();//实现搜索操作
                }
                return false;
            }
        });
```

然后再为输入框添加监听器

```
DeletableEditText search = (DeletableEditText) findViewById(R.id.search);
        search.setOnEditorActionListener(new TextView.OnEditorActionListener() {
            @Override
            public boolean onEditorAction(TextView v, int actionId, KeyEvent event) {
                if (actionId == EditorInfo.IME_ACTION_SEARCH){
                    //实现搜索操作
                    try {
                        sendHttpPostRequest();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
                return false;
            }
        });
```

最后放上几张UI，都是通过自定义View来实现的

![这里写图片描述](http://img.blog.csdn.net/20170423162529573?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamltX19jaGFybGVz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![这里写图片描述](http://img.blog.csdn.net/20170423162508338?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamltX19jaGFybGVz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![这里写图片描述](http://img.blog.csdn.net/20170413112115144?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamltX19jaGFybGVz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![这里写图片描述](http://img.blog.csdn.net/20170413112332426?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamltX19jaGFybGVz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

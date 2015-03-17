在Android Wear中的2D picker模式允许用户以页面展现的形式来选择。Wearable UI库能让你使用gridpage来轻松实现，并且可以水平和垂直滑动页面数据。


为了实现这种模式，需要在你的activity布局中添加GridViewPager 元素，并继承FragmentGridPagerAdapter 类来实现一个adapter提供页面内容。

![](https://developer.android.com/wear/images/07_uilib.png)

## 添加一个PageGrid ##


    <android.support.wearable.view.GridViewPager
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/pager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />


## 实现一个PageAdapter ##

    
    public class SampleGridPagerAdapter extends FragmentGridPagerAdapter {
     
    private final Context mContext;
     
    public SampleGridPagerAdapter(Context ctx, FragmentManager fm) {
    super(fm);
    mContext = ctx;
    }
     
    static final int[] BG_IMAGES = new int[] {
    R.drawable.debug_background_1, ...
    R.drawable.debug_background_5
    };
     
    // A simple container for static data in each page
    private static class Page {
    // static resources
    int titleRes;
    int textRes;
    int iconRes;
    ...
    }
     
    // Create a static set of pages in a 2D array
    private final Page[][] PAGES = { ... };
     
    // Override methods in FragmentGridPagerAdapter
    ...
    }



picker通过调用getFragment 和getBackground 方法来获取grid中相应位置的视图和背景。


    
    // Obtain the UI fragment at the specified position
    @Override
    public Fragment getFragment(int row, int col) {
    Page page = PAGES[row][col];
    String title =
    page.titleRes != 0 ? mContext.getString(page.titleRes) : null;
    String text =
    page.textRes != 0 ? mContext.getString(page.textRes) : null;
    CardFragment fragment = CardFragment.create(title, text, page.iconRes);
     
    // Advanced settings (card gravity, card expansion/scrolling)
    fragment.setCardGravity(page.cardGravity);
    fragment.setExpansionEnabled(page.expansionEnabled);
    fragment.setExpansionDirection(page.expansionDirection);
    fragment.setExpansionFactor(page.expansionFactor);
    return fragment;
    }
     
    // Obtain the background image for the page at the specified position
    @Override
    public ImageReference getBackground(int row, int column) {
    return ImageReference.forDrawable(BG_IMAGES[row % BG_IMAGES.length]);
    }



getRowCount 方法可以告知picker总共有多少行数据，getColumnCount 方法告知picker每行有多少列数据。

    
    // Obtain the number of pages (vertical)
    @Override
    public int getRowCount() {
    return PAGES.length;
    }
     
    // Obtain the number of pages (horizontal)
    @Override
    public int getColumnCount(int rowNum) {
    return PAGES[rowNum].length;
    }



adapter提供的每个页面都是一个fragment，在这个示例中每个页面是个cardFragment。但你可以在一个2d Picker中结合多种类型的页面，例如cards, action icons或者你的自定义布局。

不是所有行都需要数量一致的页面，注意上面示例中每行的列数都不同。你也可以使用GridViewPager来实现一个1D picker，只需要设置成一行或一列。


GridViewPager提供卡片的滚动支持，当卡片内容过长的时候。你可以通过getBackground()方法给每个页面提供一个背景图。当不同背景间切换的时候，GridViewPager会自动提供视觉差和渐进效果。



## 分配一个Adapter实例给pageGrid ##


    
    public class MainActivity extends Activity {
     
    @Override
    protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
    ...
    final GridViewPager pager = (GridViewPager) findViewById(R.id.pager);
    pager.setAdapter(new SampleGridPagerAdapter(this, getFragmentManager()));
    }
    }
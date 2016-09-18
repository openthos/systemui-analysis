####<center>OpenthosLauncher开发文档</center>
***
######概述：Launcher的开发在布局上和普通的App界面的开发很类似，在布局上并没有太多的复杂，但是在实现图标的选中拖动方面则需要选择特殊的监听器。总的来看，整个项目实现更多的在处理细节的问题。比如：文件的选中和没有选中，是在图标位置点击还是非图标位置点击。诸如此类，你很难去从百度获取有用信息，更多是自己在反复的测试中，发现问题从而增加逻辑的判断。

**从布局开始：MainActivity**
*整体逻辑描述：*
&ensp;&ensp;布局是一个桌面就类似与windows，在这个桌面上有文件和文件夹，还有快捷方式(目前是电脑和回收站)，布局xml中使用的RecyclerView，现在进行布局：
**适配器: HomeAdapter**
&ensp;&ensp;构造器：

    public class HomeAdapter extends RecyclerView.Adapter<HomeAdapter.HomeViewHolder>
    public HomeAdapter(List<HashMap<String, Object>> data, RecycleCallBack click) {
        this.data = data;
        this.mRecycleClick = click;
    }
这里的参数：data是数据-表示的是文件和文件夹，click是接口对象实现监听。
<font color = 'red'>内部类HomeViewHolder则是进行监听，后面有介绍</font>

**获取数据-默认的图标信息**

      private void initData() {
        Type[] defaultType = {Type.computer, Type.recycle};
        mDatas = new ArrayList<>();
        for (int i = 0; i < defaultName.length; i++) {
            HashMap<String, Object> map = new HashMap<>();
            map.put("name", defaultName[i]);
            map.put("isChecked", false);
            map.put("null", false);
            map.put("icon", defaultIcon.getResourceId(i, R.mipmap.ic_launcher));
            map.put("type", defaultType[i]);
            map.put("path", paths[i]);
            mDatas.add(map);
        }
        initDesktop();
    }
**获取信息的数量：**

    private int getNum() {
        DisplayMetrics dm = new DisplayMetrics();
        getWindowManager().getDefaultDisplay().getMetrics(dm);
        int widthPixels = dm.widthPixels;
        int widthNum = widthPixels / getResources().getDimensionPixelSize(R.dimen.icon_size);
        return widthNum * heightNum;
    }
在获取数量时，是通过DisplayMetrics来获取尺寸，获取的尺寸是整体宽度，然后和图标的尺寸进行比，来获取数量。
**获取数据-文件和文件夹(图标区域)**

     private void initDesktop() {
        int num = getNum();
        File doc = DiskUtils.getRoot();
        if (!doc.exists()) {           //ItemCallBack
        }
        File[] files = doc.listFiles();
        currentFiles = files;
        for (int i = 0; i < files.length; i++) {
            HashMap<String, Object> map = new HashMap<>();
            if (files[i].isDirectory()) {
                map.put("name", files[i].getName());           //文件夹的名字
                map.put("path", files[i].getAbsolutePath());   //文件夹的存储路径
                map.put("isChecked", false);                   // 是否选中
                map.put("null", false);                       //是否是空白 false 不是；
                map.put("icon", R.drawable.ic_app_file);       //文件夹图标
                map.put("type", Type.directory);               //文件夹类型  Type是一个枚举类

            } else {
                map.put("name", files[i].getName());
                map.put("path", files[i].getAbsolutePath());
                map.put("isChecked", false);
                map.put("null", false);
                map.put("icon", R.drawable.ic_app_text);
                map.put("type", Type.file);
            }
            if (mDatas.size() < num) {
                mDatas.add(map);
            }
        }
    }
**空白区域：**
因为在桌面上自然要区分有图标的区域和没有图标的区域，只有这样才能更好的处理点击事件：

      while (mDatas.size() < num) {
            HashMap<String, Object> map = new HashMap<>();
            map.put("name", "");
            map.put("isChecked", false);
            map.put("icon", -1);
            map.put("null", true);//是空白区域
            map.put("path", "");
            map.put("type", Type.blank);
            mDatas.add(map);
        }
数据的获取主要将桌面上的分为上面三部分：1.默认   2.文件  3.文件夹
数据获取则比较常规，将其放到map集合中，最后添加到mDatas中填充到适配器。
但是在获取数据信息的时候，注意获取

     'map.put("isChecked",false)'
因为要在适配器中判断是否已经选中。

     map.put("null", true);
这句主要是用于判断是否是图标区域，true就表示空白。在适配器中有所区分。
***
**监听器：**

    public interface RecycleCallBack {
    void itemOnClick(int position,View view);
    void onMove(int from,int to);}
在接口中封装两个监听器。RecyclerView的监听器需要自己来写，通常我们会选择接口回调。这里则是使用的接口的实现。
onMove监听器的实现:需求，在你拖动图标的时候，就例如：排列四个图标，你要实把第一个放到第四个的位置，并不是将第一个和第四个进行简单的交换位置，而是有一个整体的改变，2到1，3到2，4到3.这样，有个动画的效果。
具体的实现：

     public void onMove(int from, int to) {

        if ((Boolean) mDatas.get(from).get("null") != true) {
            if (to > 0 && from > 0) {
                synchronized (this) {
                    if (from > to) {
                        int count = from - to;
                        for (int i = 0; i < count; i++) {
                            Collections.swap(mDatas, from - i, from - i - 1);
                        }
                    }
                    if (from < to) {
                        int count = to - from;
                        for (int i = 0; i < count; i++) {
                            Collections.swap(mDatas, from + i, from + i + 1);
                        }
                    }
                    mAdapter.setData(mDatas);
                    mAdapter.notifyItemMoved(from, to);
                    mAdapter.pos = to;
                }
            }
        }
    }
但本项目中最主要的监听器还是实现OnTouchListener监听事件：
介绍ItemTouchHelper：它处理好了关于在RecyclerView上添加拖动排序与滑动删除的所有事情。
它是RecyclerView.ItemDecoration的子类，也就是说它可以轻易的添加到几乎所有的LayoutManager和Adapter中
它还可以和现有的item动画一起工作，提供受类型限制的拖放动画。
GitHup：[ItemTouchHelper](https://github.com/iPaulPro/Android-ItemTouchHelper-Demo)
本项目中两部分是：ItemCallBack extends ItemToucherHelper.Callback
步骤：
*1.设置：修改build.gradle,添加RecyclerView的依赖*
*2.创建ItemCallBack 继承 itemTouchhelper.Callback
&ensp;&ensp;重写方法：
基本的拖动排序和滑动删除需要重写的主要回调方法：
getMovementFlags(RecyclerView, ViewHolder)
onMove(RecyclerView， ViewHolder)
onSwiped(ViewHolder, int)
3.具体方法的介绍：

    public int getMovementFlags(RecyclerView recyclerView,
        RecyclerView.ViewHolder viewHolder) {
          int dragFlags = ItemTouchHelper.UP | ItemTouchHelper.DOWN;
          int swipeFlags = ItemTouchHelper.START | ItemTouchHelper.END;
          return makeMovementFlags(dragFlags, swipeFlags);
     }
--ItemTouchHelper可以让你轻易得到一个事件的方向。你需要重写getMovementFlags()方法来指定可以支持的拖放和滑动的方向。
     使用HelperItemTouchHelper.makeMovementFlags(int, int)来构造返回的flag。
     上下为拖动（drag）  左右为滑动（swipe）；
<hr>

    --长按的滑动 public boolean isLongPressDragEnable() {
        return true;
       }

     public boolean isItemViewSwipeEnabled() {
        return true;
     }在view任意位置触摸事件发生时启动滑动操作

***
4.适配器中的内部的监听器：
概述：*1.右键弹出菜单有两种①空白区域，这里设置在itemView中②非空白区域，设置在OnToucn监听器中。使用isExistMenue进行区别，避免弹出两个对话框。2.单击①itemView的点击(边界区域的处理)，去除之前的选中状态②点击图标可选中区域，去除之前选中状态（isClicked是指：图标区域有点击事件）③点击空白区域，去除之前的选中3.双击运行*

    public class HomeViewHolder extends RecyclerView.ViewHolder implements View.OnTouchListener {}
适配器中则进行初始化。

    itemView.setOnTouchListener(new View.OnTouchListener() {
                @Override
                public boolean onTouch(View v, MotionEvent event) {
                    if (event.getButtonState() == MotionEvent.BUTTON_SECONDARY) {          //表示右键，弹出对话框

                        if (isExistMene == false) {
                            MenuDialog dialog = new MenuDialog(itemView.getContext(), Type.blank, "/");
                            dialog.showDialog((int) event.getRawX(), (int) event.getRawY());
                        } else {
                            isExistMene = false;
                        }
                    }
                    //特殊处理边界,在这里比较特殊。故用isClicked进行区分。
                    if (event.getAction() == MotionEvent.ACTION_DOWN) {
                        if (isClicked != true && pos != -1) {
                            data.get(pos).put("isChecked", false);
                            pos = -1;
                            notifyDataSetChanged();
                        }
                        isClicked = false;

                        if (isRename == true) {
                            isRename = false;
                            notifyDataSetChanged();
                        }
                    }
                    return false;
                }
            });
这里进行的操作是对itemView进行的监听，其实就是对整个界面(并不是全部)的监听，右键弹出对话框和当单击(不是在原来的位置)进行处理：1.MotionEvent.BuTTON_SECONDARY右键弹出对话框，则就类似windows的右键菜单，在右键菜单中有位置的设置。2.MotionEvent_DOWN单击，就像是：在没有发生点击事件(isClicked != true)地方进行点击，之前如果有选中则，置为没有选中。
**在重写的方法：OnTouch监听里面**
主要实现：1.右键弹出菜单：注意设置isExistMene = true因为，就是和上面的那个弹出菜单避免重复弹出。
这里会根据MenuDialog中的type进行不同的弹出；
2.双击的实现：双击则就是两次单击，这样就需要排除右键的双击，还有两次单击的时间差要进行限制。
运行程序：

    PackageManager packageManager = item.getContext().getPackageManager();
                        Intent intent = packageManager.getLaunchIntentForPackage(OtoConsts.FILEMANAGER_PACKAGE);
                        item.getContext().startActivity(intent);
3.如果一个图标之前处于没有选中的状态，现在进行单击：

    if (pos != -1 && pos != getAdapterPosition() && (Boolean) data.get(pos).get("isChecked") == true) {
                                data.get(pos).put("isChecked", false);
                            }
就是当：pos != -1 不是初始位置，pos != getAdapterPosition()位置发生改变，（Boolean）data.get(pos).get("isChecked") == true表示之前的是选中。则就将其之前的(pos) 变为 isChecked == false
当前的则就需要变为 true:

    data.get(getAdapterPosition()).put("isChecked", true);
重新对pos进行赋值：    `pos = getAdapterPosition()`
4.单击空白的区域，进行对之前选中的进行去除。
***
重命名的实现：
在EditText的布局中：`android:selectAllOnFocus="true"`
当前组件在得到焦点的时候，自动选取该组件内的所有的文本内容；
**整体思路：**
*右键弹出框：选择重命名，在MenuDialog中发送一个handler到MainActivity中，在MainActivity中进行接收，设置mAdapter.isRename = ture ; 然后在HomeAdapter中进行对改变进行处理。主要包括：REFRESH.SORT.DELETE.NEWFOLDER等操作*
<font color = 'red'>具体代码</font>

     //重命名

    View.OnKeyListener keyListener = new View.OnKeyListener() {
        @Override
        public boolean onKey(View v, int keyCode, KeyEvent event) {
            if (keyCode == KeyEvent.KEYCODE_ENTER) {
                HashMap current = data.get(pos);
                current.put("name", ((EditText) v).getText());
                data.set(pos, current);
                v.setFocusable(false);
                v.clearFocus();
                notifyDataSetChanged();
                HashMap mCurrent = ((MainActivity) mRecycleClick).mDatas.get(pos);
                mCurrent.put("name", ((EditText) v).getText());
                ((MainActivity) mRecycleClick).mDatas.set(pos, mCurrent);
                isRename = false;
                return true;
            }
            return false;
        }
    };
<hr>

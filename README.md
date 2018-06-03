# NotePad
# 期中实验 #
## 实验一 ##
**进入已经加入时间戳的主页面**

![](https://i.imgur.com/gexN18N.png)

**时间戳所用技术与源码**
**其中更改了PROJECTION的具体参数：增加了修改时间的变量**

	 private static final String[] PROJECTION = new String[] {
           	 NotePad.Notes._ID, // 0
           	 NotePad.Notes.COLUMN_NAME_TITLE, // 1
           	 NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE,
           	 //NotePad.Notes.FLAG,
  	  };
	  
**利用Cursor查询到数据库的具体内容然后利用SimpleAdapter显示在页面上**

	  Cursor cursor = managedQuery(
                getIntent().getData(),            // Use the default content URI for the provider.
                PROJECTION,                       // Return the note ID and title for each note.
                null,                             // No where clause, return all records.
                null,                             // No where clause, therefore no where column values.
                NotePad.Notes.DEFAULT_SORT_ORDER  // Use the default sort order.
        );
        // The names of the cursor columns to display in the view, initialized to the title column
        String[] dataColumns = {  NotePad.Notes.COLUMN_NAME_TITLE , NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE} ;
        int[] viewIDs = { android.R.id.text1,android.R.id.text2};/////////////////////////////////////
        SimpleCursorAdapter adapter
                = new SimpleCursorAdapter(
                this,                             // The Context for the ListView
                R.layout.noteslist_item,          // Points to the XML for a list item
                cursor,                           // The cursor to get items from
                dataColumns,
                viewIDs
        );
        // Sets the ListView's adapter to be the cursor adapter that was just created.
        setListAdapter(adapter);
	
**增加了TextView的个数来显示时间戳：更改了显示样式以进一步美化界面**

	<LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:orientation="horizontal">

        <TextView
            android:id="@android:id/text1"
            android:layout_width="191dp"
            android:layout_height="50dp"
            android:background="@color/wheat"
            android:gravity="center_vertical"
            android:paddingLeft="10dp"
            android:textAppearance="?android:attr/textAppearanceLarge"
            android:textColor="@color/black" />
        <!--?android:attr/listPreferredItemHeight-->
        <TextView
            android:id="@android:id/text2"
            android:layout_width="306dp"
            android:layout_height="50dp"
            android:layout_gravity="center_horizontal"
            android:background="@color/sky"
            android:textAppearance="?android:attr/textAppearanceLarge"
            android:textColor="@color/white" />
    </LinearLayout>


## 实验二 ##

**搜索的实现新增了继承ListActivity的Search类，增加了具体的新的页面与动态搜索的功能。**
**仍然使用PROJECTION作为数据库显示的内容：**

	private static final String[] PROJECTION = new String[] {
            NotePad.Notes._ID, // 0
            NotePad.Notes.COLUMN_NAME_TITLE, // 1
            NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE,
   	 };
	 
**以下为搜索的函数的使用：**
**采用Cursor得到符合条件的数据，利用SimpleAdapter显示在页面之中。具体解释见代码注释**

    public boolean onQueryTextChange(String newText) {
        String selection = NotePad.Notes.COLUMN_NAME_TITLE + " Like ? ";//查询条件
        String[] selectionArgs = { "%"+newText+"%" };//查询条件参数，配合selection参数使用,%通配多个字符
        String[] dataColumns = { NotePad.Notes.COLUMN_NAME_TITLE, NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE} ;
        int[] viewIDs = { android.R.id.text1,android.R.id.text2};//用于显示带有时间戳的页面TextView
        cursor = managedQuery(
                getIntent().getData(),            // Use the default content URI for the provider.获取URI
                PROJECTION,                       // Return the note ID and title for each note.具体显示的数据库数据
                selection,                        // No where clause, return all records.搜索的条件
                selectionArgs,                    // No where clause, therefore no where column values.搜索加入的条件参数
                NotePad.Notes.DEFAULT_SORT_ORDER  // Use the default sort order.页面排列显示顺序
        );
        adapter = new SimpleCursorAdapter(
                this,                             // The Context for the ListView.当前上下文
                R.layout.noteslist_item,          // Points to the XML for a list item.显示搜索内容的布局
                cursor,                           // The cursor to get items from.搜索出的数据库内容
                dataColumns,			  //每行具体显示数据内容-COLUMN_NAME_TITLE与COLUMN_NAME_MODIFICATION_DATE
                viewIDs				  //每行具体显示布局TextView
        );
        setListAdapter(adapter);
        return true;
    }

**以下为主要操作的图标——编辑，搜索，保存复制的文本**

![](https://i.imgur.com/yhP18B5.png)

**点击按钮图标**
![](https://i.imgur.com/mRfJjYa.png)
**进入编辑页面**

![](https://i.imgur.com/RkBRefD.png)

**点击搜索图标进入搜索页面**

![](https://i.imgur.com/oDrT242.png)

**实行动态搜索，可以按照输入的字符进行动态搜索**

![](https://i.imgur.com/jvnl2AD.png)

**点击搜索内容进入具体内容页面**

![](https://i.imgur.com/r071hd4.png)

**点击图标**
![](https://i.imgur.com/hgUTtH4.png)
**进行保存编辑的文本**

**也可以在主页面选中文本进行相关操作——删除，复制或者更改标题**

![](https://i.imgur.com/rDfkdSf.png)

## 结构 ##

**具体页面见以上，以下粘贴系统函数结构**

![](https://i.imgur.com/l79P1It.png)

## 主要代码 ##

**时间戳代码**

	private final void updateNote(String text, String title) {
        Long now = Long.valueOf(System.currentTimeMillis());
        SimpleDateFormat formatter  =  new SimpleDateFormat ("yyyy/MM/dd HH:mm:ss");
		//获取当前时间
        Date curDate = new Date(now);
        String datetime = formatter.format(curDate);
        ContentValues values = new ContentValues();
        values.put(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE, datetime);
        if (mState == STATE_INSERT) {
            if (title == null) {
                int length = text.length();
                title = text.substring(0, Math.min(30, length));
                //限制长度
                if (length > 30) {
                    int lastSpace = title.lastIndexOf(' ');
                    if (lastSpace > 0) {
                        title = title.substring(0, lastSpace);
                    }
                }
            }
            // 修改数据库内容
            values.put(NotePad.Notes.COLUMN_NAME_TITLE, title);
        } else if (title != null) {
            // 修改数据库内容
            values.put(NotePad.Notes.COLUMN_NAME_TITLE, title);
        }
        // This puts the desired notes text into the map.
        values.put(NotePad.Notes.COLUMN_NAME_NOTE, text);
        getContentResolver().update(
                mUri,    // 更新的uri
                values,  // 更新的内容
                null,    // 更新的地方
                null     // 更新的条件
            );
    }


**搜索代码**

**详情见网址：**

https://github.com/xinyihaha/NotePad-master/blob/master/app/src/main/java/com/example/android/notepad/SearchActivity.java

**布局文件代码：**

https://github.com/xinyihaha/NotePad-master/blob/master/app/src/main/res/layout/list_search.xml

https://github.com/xinyihaha/NotePad-master/blob/master/app/src/main/res/layout/noteslist_item.xml


# NotePad
# 期中实验 #
## 实验一 ##
**进入已经加入时间戳的主页面**

![](https://i.imgur.com/gexN18N.png)


## 实验二 ##

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


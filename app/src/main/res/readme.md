# 期中实验 #
## 实验一 ##
**进入已经加入时间戳的主页面**

![主页面](HTTPS://I.IMGUR.COM/KVUX81T.PNG)

## 实验二 ##

**以下为主要操作的图标——编辑，搜索，保存复制的文本**

![操作](HTTPS://I.IMGUR.COM/JOHN9DT.PNG)

**点击按钮图标**
![编辑](HTTPS://I.IMGUR.COM/ME1KOJ7.PNG)
**进入编辑页面**

![编写页面](HTTPS://I.IMGUR.COM/OC5DLD0.PNG)

**点击搜索图标进入搜索页面**

![搜索1](HTTPS://I.IMGUR.COM/US0ZP3N.PNG)

**实行动态搜索，可以按照输入的字符进行动态搜索**

![搜索2](HTTPS://I.IMGUR.COM/OIMIMWD.PNG)

**点击搜索内容进入具体内容页面**

![详情页面](HTTPS://I.IMGUR.COM/4SZQJA9.PNG)

**点击图标**
![保存](HTTPS://I.IMGUR.COM/JO9AK5M.PNG)
**进行保存编辑的文本**

**也可以在主页面选中文本进行相关操作——删除，复制或者更改标题**

![选中](HTTPS://I.IMGUR.COM/PDESDHA.PNG)

## 结构 ##

**具体页面见以上，以下粘贴系统函数结构**

![函数结构](HTTPS://I.IMGUR.COM/KLSLYNB.PNG)

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

**布局文件代码：**

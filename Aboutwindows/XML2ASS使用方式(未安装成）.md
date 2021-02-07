# XML2ASS使用方式

### 安装方式

```
npm install canvas
npm install -g danmaku-to-ass
```



```
danmaku [参数] [文件列表]
```

默认输入为`.xml`文件时解析为B站弹幕，为`.json`文件（尚未实现）时解析为A站弹幕。

也可以提供一个由本工具生成的`.ass`文件（必须以`--inlude-raw=true`参数生成），以达到调整生成参数的目的，如以下命令可以将字号变小并覆盖原字幕文件：

```
danmaku --font-size=12,18,24 --out-dir=./subtitle ./subtitle
```

当然也可以使用`--out`参数单独处理一个文件，或指定不同的`--out-dir`保留旧文件。

### 参数

#### 必要参数

+ `--out {输出文件}`：使用该参数时将输出保存至参数指定的文件，与`--out-dir`二选一。
+ `--out-dir {输出目录}`：使用该参数可以将多个文件进行批量转换并保存到指定的目录下，如果使用该参数则不要使用`--out`参数。

#### 配置参数

+ `--config`：指定一个配置文件，格式见后文，指定该参数后其它配置参数都会失效。
+ `--font-size`：指定字号列表，必须是3个从小到大的数字并用逗号连接，比如`18,25,36`。
+ `--font-name`：指定字体名称。
+ `--color`：指定默认弹幕颜色，可以使用`ff3300`、`#fc9730`、`#fff`、`000`这几种格式。
+ `--outline-color`：指定边框颜色。
+ `--back-color`：指定阴影颜色。
+ `--outline`：边框的像素宽度。
+ `--shadow`：阴影的像素深度。
+ `--bold`：指定是否使用粗体，值为`true`时表示使用粗体。
+ `--opacity:` 指定字体的透明度，必须是0-1之间的小数。
+ `--padding:` 指定每条弹幕四周的空白，必须是4个数字并用逗号连接，比如`2,2,2,2`。
+ `--play-res-x`：视频播放区域的宽度。
+ `--play-res-y`：视频播放区域的高度，这个参数与宽度共同决定视频的长宽比例，具体数值不是很重要。
+ `--scroll-time`：滚动弹幕的持续时间，单位为秒。
+ `--fix-time`：固定弹幕（上方或下方）的持续时间，单位为秒。
+ `--bottom-space`：底部留空的区域大小，以防止弹幕覆盖原始字幕。
+ `--include-raw`：是否在生成的文件中保留原始信息，保留原始信息可以在后期重新使用本工具修改弹幕生成的参数，但会导致文件变大约1/3。该参数默认值为`true`，可提供`false`强制不保留原始信息。
+ `--merge-in`：指定n秒内出现相同弹幕合并为一条，参数值为一个数字，以秒为单位。
+ `--block`：提供正则来屏蔽弹幕，可以多次使用该参数，每次提供一个正则或内置规则。
+ `--block-file`：提供一个文件来屏蔽弹幕，文件中的每一行都是一个正则或内置规则，如果这个参数与`--block`一起出现，两者会被合并。这个参数在配置文件中无效。

#### 屏蔽规则

在使用`--block`或`--block-file`时，每一个屏蔽规则都是一个正则表达式或一个内置规则，比如：

```
.{4,}
呵呵
```

则表示屏幕所有长度大于4的弹幕以及包含关键字“呵呵”的弹幕。除此之外也可以使用以下内置规则：

+ `COLOR`：屏蔽所有彩色弹幕。
+ `TOP`：屏蔽所有顶部弹幕。
+ `BOTTOM`：屏幕所有底部弹幕。

需要注意的是，从已有的`.ass`文件生成新的字幕时，原有的屏蔽规则会失效，所有屏蔽规则均以此次执行命令的配置为准。

#### 参考配置文件

以下除`fontName`名均为默认值。

```
{
    "fontSize": [25, 36],
    "fontName": "黑体",
    "color": "#ffffff",
    "outlineColor": null,
    "backColor": null,
    "outline": 2,
    "shadow": 0,
    "bold": false,
    "padding": [2, 2, 2, 2],
    "playResX": 1280,
    "playResY": 720,
    "scrollTime": 8,
    "fixTime": 4,
    "opacity": 0.6,
    "bottomSpace": 60,
    "block": [],
    "includeRaw": true
}
```

## 程序调用

同样可以使用程序进行调用：

```
import convert from 'danmaku-to-ass';
import {readFileSync} from 'fs';

let text = readFileSync('av12345.xml', 'utf-8');
let ass = convert(text, {}, {source: 'bilibili', filename: 'av12345.xml'});
```

函数签名：

```
{string} convert({string} text, {Object} configOverrides, {Object} context);
```

参数说明：

+ `text`：弹幕文件内容，如果是Bilibili则为XML文本，Acfun为JSON文本。

+ `configOverrides`：覆盖默认配置的内容，见上文的配置文件参考。

+ ```
    context
    ```

    ：转换的上下文信息，需要2个属性：

    + `{string} source`：内容的来源类型，为`"bilibili"`或`"acfun"`（注意全小写）。
    + `{string} filename`：来源文件名，如果没有的话可以随便写一个，主要放在ass文件的信息部分。
---
title: Markdown_Language
description: 
published: true
date: 2022-05-20T06:50:12.572Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:56:12.314Z
---

## Bold and italic
Code：

```
*Bold*or_italic_
**bold**
***bold and italic***
~~strikethrough~~
```

Result：

*bold*or_italic_

**bold**

***bold and italic***

~~strikethrough~~

## Heading and sub-heading

```
# Heading （Already used by entry name; please use sub-heading for headings in text)
## 2nd level heading
### 3rd level heading
#### 4th level heading
##### 5th level heading
###### 6th level heading
```

As we use [TOC] to automatically generate table of contents,  the effect for headings is not shown here to avoid chaos of content. You can try it in your editor instead! Remember the 1st headings have the largest font, and the 6th heading have the smallest font.

## Hyperlink
Markdown support two kind of hyperlink: inline and reference. Inline hyperlink is the most used one.

### Inline-style

Syntax：

  [Link text](URL "title")

Put link text in "[ ]", URL in "( )". A title can be append to the URL in "( )". Title will be displayed when cursor is on the hyperlink. Note that there is a space between title  and  URL.

Code:

```
Welcome to [deepin](https://www.deepin.com/)
Welcome to [deepin](https://www.deepin.com/ "Deepin Official Website")
```

Result：

Welcome to [deepin](https://www.deepin.com/)

Welcome to [deepin](https://www.deepin.com/ "Deepin Official Website")

### Reference-style

Refencence-style hyperlink is generally used in academic paper. When you need to cite one link several times in a page, it is recommended to use reference-style hyperlink, for it is easy to manage these links

Syntax:

It contains two part:  for citing part in text, use

    [Link text][ Link ID]

for link part (in any where of the text), use

    [Link ID]:URL “title”

Note that there is a space between title  and  URL.

If you would like the link ID become the same as link text, use

    [Link text][] 

See following example for detailed format.

Code:

```
I usually visit [Google][1]、[Leanote][2] and [deepin][3]
[Leanote Note][2] is a good [website][]。
[1]:http://www.google.com "Google"
[2]:http://www.leanote.com "Leanote"
[3]:https://www.deepin.com/ "Deepin Official Website"
[website]:http://http://blog.leanote.com/freewalk
```

Result:

I usually visit [Google][1]、[Leanote][2] and [deepin][3]

[Leanote Note][2] is a good [website][]。

[1]:http://www.google.com "Google"

[2]:http://www.leanote.com "Leanote"

[3]:https://www.deepin.com/ "Deepin Official Website"

[website]:http://http://blog.leanote.com/freewalk

### Automatic link

Syntax:

Markdown support short link for web address and mail address. If you wrap them in "< >", Markdown parser will automatically convert them into hyperlink. The link text is the same as link address. For example:

Code:

```
<http://example.com/>
<address@example.com>
```

Result:

<http://example.com/>

<address@example.com>

## Anchor

Anchor is a kind of hyperlink that has effect within one web page. It is responsable for jumping between elements in one document. For example, an anchor is written in table of contents, and conducts you back to table of contents when a hyperlink pointing to this anchor is clicked; in contrast, an anchor in text can be reached by clicking the hyperlink that points to it.

Note:

1. Markdown Extra support only anchors that are closely after the title;
2. The result displaying area on the right side of Leanote Editor does not support jumping between anchor, so it is normal that anchors do no function before your article is published.

Syntax:

Insert the anchor (#) after the title you would like to jump to, then cite this anchor anywhere in the document.

Code:

```
## 0. Table of contents{#index}
Go to [table of contents](#index)
```

Result:

Go to [table of contents](#index)

## List

### Bullet list

Use *，+，-  before items within a bullet list.

Code:

```
- Bullet list 1
- Bullet list 2
- Bullet list 3
```

Result:

- Bullet list 1

- Bullet list 2

- Bullet list 3

### Numbered list

Add one number and a dot (.) before items within numbered list.

Code:

```
1. Numbered list 1
2. Numbered list 2
3. Numbered list 3
```

Result:

1. Numbered list 1

2. Numbered list 2

3. Numbered list 3

## Inserting image

Same as inserting hyperlink, and can also be done in two ways: inline or reference.

### Inline-style

Syntax：

    ![Picture AltText](URL_of_the_picture "Title")

The "AltText" represents the alternative text used to replace the picture in condition that the picture cannot be displayed for some reason. "Title" has the similar function of title in hyperlink, it is shown when cursor hovers over the picture. Both AltText and Title are not mandatory, but it is suggest to include them.

Code:

```
Flowers： 
![Flowers](http://ww2.sinaimg.cn/large/56d258bdjw1eugeubg8ujj21kw16odn6.jpg "Flowers")
```

Result:

Flowers：
![Flowers](http://ww2.sinaimg.cn/large/56d258bdjw1eugeubg8ujj21kw16odn6.jpg "Flowers")

### Reference-style

Syntax:

Add following text where you would like to insert a picture:

    ![Picture AltText][PictureID]

Append to the document:

    [PictureID]:URL "Title"

Code:

```
Flowers:
![Flowers][flower]
[flower]:http://ww2.sinaimg.cn/large/56d258bdjw1eugeubg8ujj21kw16odn6.jpg  "Flowers"
```

Result:

Flowers:

![Flowers][flower]
[flower]:<http://ww2.sinaimg.cn/large/56d258bdjw1eugeubg8ujj21kw16odn6.jpg>  "Flowers"

## Footnote

Syntax:

Append "[^NoteID]" to the text you want to mark. Then write footnote anywhere you like (normally at the end of the text). The "NoteID" should be added before this footnote.

Note: There must be one blank line between footnotes, otherwise Markdown parser may fail. After the convertion of Markdown, all footnotes will be moved to the end of document.

Code:

```
Markdown[^1]can be used to write document efficiently. You can use Leanote[^Le] to convert Markdown text to HTML[^2] directly.
[^1]:Markdown is a plain text markup language
[^2]:HTML: HyperText Markup Language
[^Le]:Open source note platform, supports blogs written in Markdown and plain text
```

Result:

Markdown[^1]can be used to write document efficiently. You can use Leanote[^Le] to convert Markdown text to HTML[^2] directly.
[^1]:Markdown is a plain text markup language
[^2]:HTML: HyperText Markup Language
[^Le]:Open source note platform, supports blogs written in Markdown and plain text

## Table

Syntax:

There are two ways to construct a table. Both require three basic part: one line contains header, one line contains seperator and one (or more) line contains the content.

Code:

Simple way:

```
Name|Sex|Score
-|-|-
Wang|Male|75
Zhang|Female|79
Li|Male|92
```

Native way:

```
Name|Sex|Score
-|-|-
Wang|Male|75
Zhang|Female|79
Li|Male|92
```

Use pipe character ("|") to seperate columns. In the second way, two extract "|" should be added to the start and the end of each line.

You can specify alignment mode to each column in the second line: "-" means centered alignment, ":-" means left justified and "-:" means right justified. Left justified mode is choosed by default.

Specify alignment mode for table:

```
Product|Price
-|-:
Leanote Account Basic|￥60 / year
Leanote Account Premuim|￥120 CNY / year
```

Result:

Name|Sex|Score
-|-|-
Wang|Male|75
Zhang|Female|79
Li|Male|92

Name|Sex|Score
-|-|-
Wang|Male|75
Zhang|Female|79
Li|Male|92

Product|Price
-|-:
Leanote Account Basic|￥60 / year
Leanote Account Premuim|￥120 CNY / year

## Code

This is a necessary function for programmer. There are two ways to insert code: one to use indent (Tab), the other to wrap with "`" character (generally, you can find it below Esc on your keyboard).

Syntax:

For inline code, suc as a word or a sentence, use ``` `code` ```.

For multiple line code, use indent or "` ```code``` `". You need to add a blank line before indented code. See example below for details.

### Inline-style

Code:

```
How to use `scanf()` in C?
```

Result:

How to use `scanf()` in C?

### Indented style

Use 4 space or 1 tab to indent your code. The code block will end before the line without indent, or the end of that file.

Code:

```
    #include <stdio.h>
    int main(void)
    {
        printf("Hello world\n");
    }
```

Result:

    #include <stdio.h>
    int main(void)
    {
        printf("Hello world\n");
    }

### Use six ` to wrap multi-line code

Code:

    ```
    #include <stdio.h>
    int main(void)
    {
        printf("Hello world\n");
    }
    ```

Result:

```
#include <stdio.h>
int main(void)
{
    printf("Hello world\n");
}
```

### HTML raw code

In code blocks, &,  < and > are converted to HTML entities, thus it is convenient to write example HTML code using Markdown by copying and pasting!

Code:

An example：

```
<div class="footer">
   © 2004 Foo Corporation
</div>
```

Another example:

```
<table>
    <tr>
        <th rowspan="2">On duty</th>
        <th>Mon</th>
        <th>Tue</th>
        <th>Wed</th>
    </tr>
    <tr>
        <td>Li Qiang</td>
        <td>Zhang Ming</td>
        <td>Wang Ping</td>
    </tr>
</table>
```

Result:

<div class="footer">
   © 2004 Foo Corporation
</div>

<table>
    <tr>
        <th rowspan="2">On duty</th>
        <th>Mon</th>
        <th>Tue</th>
        <th>Wed</th>
    </tr>
    <tr>
        <td>Li Qiang</td>
        <td>Zhang Ming</td>
        <td>Wang Ping</td>
    </tr>
</table>

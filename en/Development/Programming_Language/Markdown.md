---
title: Markdown
description: 
published: true
date: 2022-07-21T08:39:14.168Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:56:12.314Z
---

# Headings 

### Markdown Syntax :

```
# Heading level 1
## Heading level 2
### Heading level 3
#### Heading level 4
##### Heading level 5
###### Heading level 6
```

As we use [TOC] to automatically generate table of contents,  the effect for headings is not shown here to avoid chaos of content. You can try it in your editor instead! Remember the 1st headings have the largest font, and the 6th heading have the smallest font.

### Rendered Output :

# Heading level 1
## Heading level 2
### Heading level 3
#### Heading level 4
##### Heading level 5
###### Heading level 6

# Bold and italic

### Markdown Syntax :

```
*Italic* or _italic_
**Bold** or __bold__
***Bold and Italic*** or ___bold and italic___
```

### Rendered Output :

*Italic* or _italic_

**Bold** or __bold__

***bold and italic***

# Strikethrough
### Markdown Syntax :

```
~~strikethrough~~
```

### Rendered Output :

~~strikethrough~~

# Blockquotes

### Markdown Syntax :

```
> Deepin is the top Linux distribution from China
```

### Rendered Output :

> Deepin is the top Linux distribution from China

### Blockquotes with Multiple Paragraphs

### Markdown Syntax :
```
> Deepin is the top Linux distribution from China
>
> Deepin is the top Linux distribution from China
```

### Rendered Output :

> Deepin is the top Linux distribution from China
>
> Deepin is the top Linux distribution from China

### Nested Blockquotes
### Markdown Syntax :

```
> Deepin is the top Linux distribution from China
>
>> Deepin is the top Linux distribution from China
```

### Rendered Output :

> Deepin is the top Linux distribution from China
>
>> Deepin is the top Linux distribution from China

# Lists

## Unordered Lists

Use *，+，-  before items within a unordered list.

### Markdown Syntax :

```
- First item
- Second item
- Third item
- Fourth item
```
or
```
+ First item
+ Second item
+ Third item
+ Fourth item
```
or
```
* First item
* Second item
* Third item
* Fourth item
```
### Rendered Output :

- First item
- Second item
- Third item
- Fourth item

or

+ First item
+ Second item
+ Third item
+ Fourth item

or

* First item
* Second item
* Third item
* Fourth item

## Ordered Lists

Add one number and a dot (.) before items within ordered list.

### Markdown Syntax :

```
1. First item
2. Second item
3. Third item
4. Fourth item
```

### Rendered Output :

1. First item
2. Second item
3. Third item
4. Fourth item

# Hyperlink

Markdown support two kind of hyperlink: inline and reference. Inline hyperlink is the most used one.

## Inline-style
Put link text in "[ ]", URL in "( )". A title can be append to the URL in "( )". Title will be displayed when cursor is on the hyperlink. Note that there is a space between title and URL.

`[Link text](URL "title")`

### Markdown Syntax :

```
Welcome to [deepin](https://www.deepin.com/)
Welcome to [deepin](https://www.deepin.com/ "Deepin Official Website")
```

### Rendered Output :

Welcome to [deepin](https://www.deepin.com/)
Welcome to [deepin](https://www.deepin.com/ "Deepin Official Website")

## Reference-style

Refencence-style hyperlink is generally used in academic paper. When you need to cite one link several times in a page, it is recommended to use reference-style hyperlink, for it is easy to manage these links

### Markdown Syntax :

- It contains two part:  for citing part in text, use `[Link text][ Link ID]`

- for link part (in any where of the text), use `[Link ID]:URL “title”`

   > Note that there is a space between title  and  URL.

- If you would like the link ID become the same as link text, use `[Link text][] `

See following example for detailed format:

```
I usually visit [Google][1]、[Leanote][2] and [deepin][3]
[Leanote Note][2] is a good [website][]。
[1]:http://www.google.com "Google"
[2]:http://www.leanote.com "Leanote"
[3]:https://www.deepin.com/ "Deepin Official Website"
[website]:http://blog.leanote.com/freewalk
```

### Rendered Output :

I usually visit [Google][1]、[Leanote][2] and [deepin][3]

[Leanote Note][2] is a good [website][]。

[1]:http://www.google.com "Google"

[2]:http://www.leanote.com "Leanote"

[3]:https://www.deepin.com/ "Deepin Official Website"

[website]:http://blog.leanote.com/freewalk

# Automatic link

Markdown support short link for web address and mail address. If you wrap them in "< >", Markdown parser will automatically convert them into hyperlink. The link text is the same as link address.

### Markdown Syntax :

```
<https://bbs.deepin.org/>
<support@deepin.org>
```

### Rendered Output :

<https://bbs.deepin.org/>
<support@deepin.org>

# Anchor

Anchor is a kind of hyperlink that has effect within one web page. It is responsable for jumping between elements in one document. For example, an anchor is written in table of contents, and conducts you back to table of contents when a hyperlink pointing to this anchor is clicked; in contrast, an anchor in text can be reached by clicking the hyperlink that points to it.

> Note:
>  1. Markdown Extra support only anchors that are closely after the title;
>  2. The result displaying area on the right side of Leanote Editor does not support jumping between anchor, so it is normal that anchors do no function before your article is published.

### Markdown Syntax :

Insert the anchor (#) after the title you would like to jump to, then cite this anchor anywhere in the document.

```
#### Table of contents{#index}
Go to [table of contents](#index)
```

### Rendered Output :

#### Table of contents{#index}
Go to [table of contents](#index)

# Images

### Markdown Syntax :
```
 ![Picture AltText](URL_of_the_picture "Title")
```
   
The "AltText" represents the alternative text used to replace the picture in condition that the picture cannot be displayed for some reason. "Title" has the similar function of title in hyperlink, it is shown when cursor hovers over the picture. Both AltText and Title are not mandatory, but it is suggest to include them.

For example:

```
Deepin_logo： 
![Deepin_logo](/图片存储/deepin_logo.png "Deepin logo")
```

### Rendered Output :

Deepin_logo： 
![Deepin_logo](/图片存储/deepin_logo.png "Deepin logo")

# Footnote

### Markdown Syntax :

Append "[^NoteID]" to the text you want to mark. Then write footnote anywhere you like (normally at the end of the text). The "NoteID" should be added before this footnote.

> Note: There must be one blank line between footnotes, otherwise Markdown parser may fail. After the convertion of Markdown, all footnotes will be moved to the end of document.

For example:

```
Markdown[^1]can be used to write document efficiently. You can use Leanote[^Le] to convert Markdown text to HTML[^2] directly.
[^1]:Markdown is a plain text markup language
[^2]:HTML: HyperText Markup Language
[^Le]:Open source note platform, supports blogs written in Markdown and plain text
```

### Rendered Output :

Markdown[^1]can be used to write document efficiently. You can use Leanote[^Le] to convert Markdown text to HTML[^2] directly.
[^1]:Markdown is a plain text markup language
[^2]:HTML: HyperText Markup Language
[^Le]:Open source note platform, supports blogs written in Markdown and plain text

# Table

### Markdown Syntax :

There are two ways to construct a table.

1. Simple way: 
Both require three basic part: one line contains header, one line contains separator and one (or more) line contains the content.
```
Name|Sex|Score
-|-|-
Wang|Male|75
Zhang|Female|79
Li|Male|92
```
2. Native way:
Use pipe character ("|") to separate columns. In the second way, two extract "|" should be added to the start and the end of each line.
```
|Name|Sex|Score|
|-|-|-|
|Wang|Male|75|
|Zhang|Female|79|
|Li|Male|92|
```
3. Alignment
You can specify alignment mode to each column in the second line: ":-:" means centered alignment, ":-" means left justified and "-:" means right justified. Left justified mode is choosed by default.

Specify alignment mode for table:

```
Product|Price
-|-:
Leanote Account Basic|￥60 / year
Leanote Account Premuim|￥120 CNY / year
```

### Rendered Output :
1. simple way

Name|Sex|Score
-|-|-
Wang|Male|75
Zhang|Female|79
Li|Male|92

2. native way

|Name|Sex|Score|
|-|-|-|
|Wang|Male|75|
|Zhang|Female|79|
|Li|Male|92|

3. right alignment

Product|Price
-|-:
Leanote Account Basic|￥60 / year
Leanote Account Premuim|￥120 CNY / year

# Code

This is a necessary function for programmer. There are two ways to insert code: one to use indent (Tab), the other to wrap with "\`" character (generally, you can find it below <kbd>Esc</kbd> on your keyboard).

### Markdown Syntax :

For inline code, such as a word or a sentence, use \` code \`.

For multiple line code, use indent or "` ```code``` `". You need to add a blank line before indented code. See example below for details.

Use 4 space or 1 tab to indent your code. The code block will end before the line without indent, or the end of that file.

1. Inline style :

```
How to use `scanf()` in C?
```

2. Multiple line style :

```
    ```
    #include <stdio.h>
    int main(void)
    {
        printf("Hello world\n");
    }
    ```
```

3. Indent style :
```

Tab		#include <stdio.h>
Tab		int main(void)
Tab		{
Tab  	 		printf("Hello world\n");
Tab		}
    
```

### Rendered Output :

1. Inline style :

How to use `scanf()` in C?

2. Multiple line style :

```
#include <stdio.h>
int main(void)
{
   printf("Hello world\n");
}
```

3. Indent style :

		#include <stdio.h>
		int main(void)
		{
  	 		printf("Hello world\n");
		}


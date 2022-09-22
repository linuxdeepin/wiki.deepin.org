---
title: Tips to Know Before Translation
description: 
published: true
date: 2022-07-14T02:22:08.792Z
tags: 
editor: markdown
dateCreated: 2022-07-14T02:13:34.058Z
---


Thanks very much for your contributions to our internationalization project. To translate  better, please read the tips below before your translation. If you have any questions about source strings, please add a comment or issue on the translation page, we will reply soon. 

**Here are translation tips:**

**1**、When you see a word that has several meanings, you may don't know how to translate it. Do not worry, you can refer to the context below, instructions (if any), or comments (if any) to get its meaning.

![image11.png](/image11.png)



**2**、For translators who know Chinese,  it is suggested to use Chinese as the original language rather than English, so you can easily and clearly get the exact meaning. Here is how: Click the gear icon in the upper right corner of the translation page and select "Show source string in English (en)", change the language to Chinese(China)(zh_CN), then you will see the source strings in Chinese(Simplified).


![image22.png](/image22.png)

**3**、If there is %1 %2, %s, or %q in the sentence, it means an indeterminate variable and you don't need to translate it.

![image33.png](/image33.png)

**4**、If there is HTML code shown as blocks with 1, 2..., no need to translate them, just click to copy them.
![image44.png](/image44.png)

**5**、Some words are too professional, or too common, you don't need to translate as well, eg. FAT32, PDF.

**6**、If there are line breaks in the sentence, your translation should keep the same.

![image55.png](/image55.png)

**7**、How to translate the date and time, such as yyyy-MM-dd dddd (Example: 2022-12-30 Friday), and yyyy-MM-dd hh:mm (Example: 2022-12-30 09:29)?

You can change the order of yyyy, mm, dd, dddd, hh, mm to display the appropriate date format for different locales, but you cannot translate it into your words such as гггг-ММ-дд чч:мм. Please keep using yyyy, mm, dd,  dddd, hh, mm.


![image66.png](/image66.png)

**8**、The Save button is not enabled for an untranslated string.
Please check if the string contains plural forms, and translate all forms to enable the Save button. Once you fill in all translations, you should be able to click the "Save Translation" button.

![image88.png](/image88.png)

**9**、How to test the translation in Deepin OS？ You should convert the .ts language file into .qm format. 
- Download the translated file from Transifex.
- Install Qt Linguist. 
- Open the .ts file in Qt Linguist and save it in the .qm format. 
- Move the .qm file to file:///usr/share/deepin-appname/translations.
- Open deepin applications to test the translations.

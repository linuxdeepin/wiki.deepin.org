---
title: Things_To_Know_Before_Translating
description: 
published: true
date: 2022-05-07T07:52:12.332Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:43:21.865Z
---

Hello everyone, thanks very much for the contributions on our internationalization project. Here we prepared these NOTES for better translation:

1. When see a single word without any usage scenario,

eg. Empty

You may don't know how to translate. At this time, you can refer to the comments or context in Transifex.

2. The word does not mean what it normally means. This situation usually exists in Chinese-English-Chinese (Taiwan)

eg.  Confirm

such as Are you sure you want to uninstall Deepin Movie?     Cancel/Confirm

We can know "Confirm" means "Uninstall" in a button from the details.

So we suggest translators from Chinese (Taiwan) to use Chinese as the original language rather than English.
Here is how: Click the gear icon in the upper right corner of translation page and select "Show source String in English (en)", change the language to Chinese(China)(zh_CN), then you will see the strings in Chinese(Simplified).

3. If there is %1 %2, or %q in the sentence, it means an indeterminate variable and you don't need to translate it.

eg. Monitor %1

4. If there is an underline in a word, such as _Run, there are two solutions:

a. When the keyboard layout in target language is common, write the underline after the word.

eg. Run（_R）

b.When the keyboard layout in target language is uncommon, ignore the underline and only write the word.

eg. Run

5. When there are angle brackets or html code shown as blocks with 1, 2... (Deepin Website), you don’t need to translate them.

eg.  Reverting to previous display settings in ```<font color='white'>```%1```<font>```

6. Some words are too professional, or too common, you don't need to translate as well.

eg. MP4, PDF

7. If there are line breaks in the sentence, just keep the same.

eg.

Partition is detected to have been mounted.⏎

Are you sure you want to unmount it?

8. How to translate date and time?

eg.
yyyy-MM-dd dddd
yyyy-MM-dd hh:mm

You can change the order of yyyy,mm,dd,hh,mm to display the appropriate date format for different locales, but you cannot translate it into your words such as гггг-ММ-дд чч:мм. Please keep using yyyy,mm,dd,hh,mm.

9. Save button is not enabled for a untranslated string.

Please check if the string contains plural forms, and translate all forms to enable the Save button by pressing "1" and "Other". Once you fill all versions, you should be able to press the "Save Translation" button.

10. How to test translated file in Deepin OS？

You should convert the .ts language package in .qm format.
Download and install Qt Creator from Deepin Store. Open the .ts file in the Qt5 Linguist program and save it in the .qm format.
Move file to usr/share/deepin-app/translations, and then open deepin application to test the translations.

<br/>
If you have any questions about Transifex (https://www.transifex.com/linuxdeepin/) translation, please add a comment or issue in Transifex, we will reply soon.

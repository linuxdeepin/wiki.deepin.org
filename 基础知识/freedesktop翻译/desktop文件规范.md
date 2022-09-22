---
title: desktop文件规范
description: 对于linux常见桌面软件的.desktop文件的规范
published: true
date: 2022-07-21T02:22:04.289Z
tags: 
editor: markdown
dateCreated: 2022-07-21T02:22:01.484Z
---

## 文件命名

桌面条目文件应具有`.desktop` 扩展名，但应具有扩展名的文件`Type` `Directory`除外 `.directory`。根据扩展名确定文件类型非常容易和快速。当不存在文件扩展名时，桌面系统应通过“魔术检测”回退到识别。

对于应用程序，桌面文件的名称在扩展名之前的 `.desktop`部分应该是有效的 [D-Bus 众所周知的名称](https://dbus.freedesktop.org/doc/dbus-specification.html#message-protocol-names)。这意味着它是由点分隔的非空元素序列 (U+002E FULL STOP)，不能以数字开头，并且每个元素仅包含集合中的字符`[A-Za-z0-9-_]`：ASCII 字母、数字、破折号 (U +002D HYPHEN-MINUS）和下划线（U+005F LOW LINE）。

桌面条目的名称应遵循“反向 DNS”约定：它应以应用程序作者控制的反向 DNS 域名开头，小写。域名后面应该是应用程序的名称，它通常由连在一起的单词和首字母大写 (CamelCase) 组成。例如，如果所有者 `example.org`写入“Foo Viewer”，他们可能会选择名称`org.example.FooViewer`，从而生成一个名为`org.example.FooViewer.desktop` （与dbus命名方式一致）

允许但是不推荐使用破折号，因为在反向 DNS 名称的某些相关用途中不允许使用破折号，例如 D-Bus 对象路径和接口名称，以及 Flatpak 应用程序 ID。如果作者的域名包含破折号，建议用下划线代替：这不会引起歧义，因为下划线不允许出现在 DNS 域名中。

如果作者的域名包含以数字开头的标签（这在 D-Bus 知名名称中是不允许的），建议在桌面条目名称的该元素前加上下划线。例如，[7-zip.org](http://7-zip.org) 可能会发布一个名为 `org._7_zip.Archiver`.（规避数字开头）

### 桌面文件 ID

代表应用程序的每个桌面条目都由其*desktop file ID*标识，该文件 ID 基于其文件名。

要确定桌面文件的 ID，请使其完整路径相对于`$XDG_DATA_DIRS`（见[XDG 基本目录规范](/zh/基础知识/freedesktop翻译/XDG基本目录规范)）安装桌面文件的组件，删除“`applications/`”之前的所有前缀（包含application），并将“/”转换为“-”。

例如`/usr/share/applications/foo/bar.desktop`具有桌面文件 ID `foo-bar.desktop`。

如果多个文件具有相同的桌面文件 ID，则使用 `$XDG_DATA_DIRS` 优先顺序中的第一个。

例如，如果`$XDG_DATA_DIRS`包含默认路径 `/usr/local/share:/usr/share`，则 `/usr/local/share/applications/org.foo.bar.desktop`和 `/usr/share/applications/org.foo.bar.desktop`都具有相同的桌面文件 ID `org.foo.bar.desktop`，但只会使用第一个。

如果`foo-bar.desktop`和`foo/bar.desktop`都存在，则不确定选择哪个。

如果桌面文件没有安装在`$XDG_DATA_DIRS`某个组件的应用程序子目录中，那么它就没有ID。

## 文件的基本格式


desktop文件是以UTF-8编码的。一个文件被解释为一系列由换行符分隔的行。在文件中，重视大小写。

兼容的实现必须不从文件中删除任何字段，即使他们不支持这些字段。这些字段必须保持在某个列表中，如果文件被 "重写"，它们将被包括在内。这确保即使另一个系统访问和更改文件，任何特定于桌面的扩展都将被保留。

### 注释

以`#`开头的行和空行被认为是注释，将被忽略，但是它们应该在桌面条目文件的读写过程中被保留下来。

注释行是不被解释的，可以包含任何字符（LF除外）。然而，我们鼓励对包含非ASCII字符的注释行使用UTF-8

### grup


名称为groupname的组头是一个格式的行。

[组名］
组名可以包含所有的ASCII字符，除了[ 和 ] 以及控制字符。

多个组不能有相同的名称。

在组头之后直到新组头的所有{key,value}对都属于该组。

桌面条目文件的基本格式要求有一个名为桌面条目的组头。文件中可能还有其他组，但这是最重要的组，需要明确地支持。这个组也应该被用作自动检测MIME类型的 "魔钥"。在桌面条目文件中，这个组前面应该没有任何东西，但可能有一个或多个注释。

### Entries

文件中的条目是{key,value}对，格式为：键=值
等号前后的空格应被忽略；=号是实际的分隔符。

只有字符A-Z，a-z，0-9-可以用于键名。

由于大小写的关系，键名和NAME是不等同的。

同一组中的多个键不能有相同的名字。不同组中的键可以有相同的名称。

### 例子：

```text
[Desktop Entry]  #
X-Deepin-CreatedBy=com.deepin.dde.daemon.Launcher
X-Deepin-AppID=code
Name=Visual Studio Code
Comment=Code Editing. Redefined.
GenericName=Text Editor
Exec=/opt/apps/com.visualstudio.code/files/code/code --unity-launch %F
Icon=com.visualstudio.code
Type=Application
StartupNotify=false
StartupWMClass=Code
Categories=TextEditor;Development;IDE;
MimeType=text/plain;inode/directory;application/x-code-workspace;
Actions=new-empty-window;
Keywords=vscode;

[Desktop Action new-empty-window]
Name=New Empty Window
Exec=/opt/apps/com.visualstudio.code/files/code/code --new-window %F
Icon=com.visualstudio.code

```

这就提供了一个很好的例子：有分组 有entry类型

## 可能的值类型

识别的值类型是`string`、 `localestring`、 `iconstring`、 `boolean`和 `numeric`。

- type 的值`string`可以包含除控制字符之外的所有 ASCII 字符。
- type 的值`localestring`是用户可显示的，并以 UTF-8 编码。
- type 的值`iconstring`是图标的名称；这些可能是绝对路径，或者是使用[图标主题规范](http://freedesktop.org/wiki/Standards/icon-theme-spec)中描述的算法定位的图标的符号名称。此类值不是用户可显示的，并且以 UTF-8 编码。
- type 的值`boolean`必须是字符串 `true`或`false`.
- type 的值必须是本地环境中的说明符 `numeric`识别的有效浮点数。 `%f``scanf``C`

转义序列`\s`, `\n`, `\t`,`\r`和 `\`支持 `string`,`localestring`和 类型的值，分别`iconstring`表示 ASCII 空格、换行符、制表符、回车和反斜杠。

一些键可以有多个值。在这种情况下，键的值被指定为复数形式：例如，`string(s)`。多个值应该用分号分隔，并且键的值可以选择用分号终止。尾随的空字符串必须始终以分号结尾。这些值中的分号需要使用`\;`.

## 键的国际化（翻译）值

类型为`localestring`和`iconstring`的键可以用[LOCALE]作为后缀，其中LOCALE是该条目的locale类型。LOCALE必须是`lang_COUNTRY.ENCODING@MODIFIER`的形式，其中_`COUNTRY, .ENCODING`, 和`@MODIFIER`可以省略。如果出现后缀的键，同样的键也必须出现，但没有后缀。

当读入桌面条目文件时，键的值是通过匹配LC_MESSAGES类别的当前POSIX语言和该键所有出现的LOCALE后缀来选择的，.ENCODING部分被剥离。

匹配过程如下。如果LC_MESSAGES的形式是lang_COUNTRY.ENCODING@MODIFIER，那么它将匹配形式为lang_COUNTRY@MODIFIER的键。如果这样的键不存在，它将试图匹配lang_COUNTRY，然后是lang@MODIFIER。然后，将尝试与lang本身进行匹配。最后，如果没有找到匹配的键，将使用没有指定locale的所需键。匹配时，LC_MESSAGES值的编码将被忽略。

如果LC_MESSAGES没有MODIFIER字段，那么将不会匹配带有修改器的键。同样的，如果LC_MESSAGES没有COUNTRY字段，那么将不会匹配有国家的键。如果LC_MESSAGES只有一个lang字段，那么它将直接与一个具有类似值的键进行匹配。下表列出了各种LC_MESSAGES值的可能匹配情况，以及它们被匹配的顺序。注意，ENCODING字段没有显示。

|`LC_MESSAGES`价值|按匹配顺序排列的可能键|
|-|-|
|`lang__COUNTRY@MODIFIER`|`lang_COUNTRY@MODIFIER` 默认值 `lang_COUNTRY lang@MODIFIER  lang`|
|`lang__COUNTRY`|`lang_COUNTRY_`, *`lang`*, 默认值|
|`lang@MODIFIER`|`lang@MODIFIER`, *`lang`*, 默认值|
|*`lang`*|*`lang`*， 默认值|


例如，如果`LC_MESSAGES`类别的当前值为`sr_YU@Latn`并且桌面文件包括：

`name=Foo` `name[sr_YU]=...` `name[sr@Latn]=...` `name[sr]=...`

然后使用`Name`keyed by的值`sr_YU`。

尽管类型的图标名称是`iconstring`可本地化的，但它们不是给用户看的，因此通常不应由翻译工具处理。大多数应用程序不需要本地化它们的图标；例外情况可能包括包含文本或文化特定符号的图标（比如宗教或者xxx禁忌）。

### 例子：

```
[Desktop Entry]
Categories=Utility;TextEditor;
Comment=Simple and easy to use text editor
Exec=deepin-editor %F
Icon=deepin-editor
Name=Text Editor
GenericName=Text Editor
StartupNotify=false
Type=Application
X-Deepin-AppID=deepin-editor
X-MultipleArgs=false
X-Deepin-CreatedBy=com.deepin.dde.daemon.Launcher
X-Deepin-Vendor=deepin
# Translations:
# Do not manually modify!
Comment[ar]=محرر النصوص بسيط وسهل الاستخدام
Comment[bo]=སྟབས་བདེ་ལ་སྤྱོད་བདེ་བའི་ཡིག་ཆ་རྩོམ་སྒྲིག་ཆས།
Comment[br]=An aozer testennoù simpl hag aes d'ober gantañ
Comment[ca]=Editor de text simple i fàcil d'usar
Comment[cs]=Jednoduchý a snadno se používající textový editor
Comment[da]=Simpel og letanvendelig teksteditor
Comment[de]=Einfacher, leicht zu bedienender Texteditor
Comment[es]=Editor de texto simple y fácil de usar
Comment[et]=Lihtne ja  kergesti kasutatav tekstiredaktor
Comment[fi]=Yksinkertainen ja helppokäyttöinen tekstieditori
Comment[fr]=Éditeur de texte simple et facile à utiliser
Comment[gl_ES]=Editor de texto sinxelo e fácil de usar
Comment[hr]=Jednostavan i lagan za korištenje uređivač teksta
Comment[hu]=Letisztult és könnyen használható szövegszerkesztő
Comment[it]=Editor di testo semplice e facile da usare. Localizzazione italiana a cura di Massimo A. Carofano.
Comment[ko]=단순하고 사용하기 쉬운 텍스트 편집기
Comment[lt]=Paprastas ir lengvas naudoti tekstų redaktorius
Comment[ms]=Penyunting teks yang ringkas dan mudah digunakan
Comment[nl]=Eenvoudig te gebruiken tekstbewerker
Comment[pl]=Prosty i łatwy w użyciu edytor tekstu
Comment[pt]=Editor de texto simples e fácil de usar
Comment[pt_BR]=Um editor de texto simples e fácil de usar
Comment[ru]=Простой и удобный текстовый редактор
Comment[sq]=Përpunues tekstesh i thjeshtë dhe i kollajtë për t’u përdorur
Comment[sr]=Уењђивач текста који је једноставан за употребу
Comment[tr]=Basit ve kullanımı kolay metin düzenleyici
Comment[ug]=ئاددىي، قوللىنىشچان تېكىست تەھرىرلىگۈچ
Comment[uk]=Простий і доступний текстовий редактор
Comment[zh_CN]=简单好用的文本编辑器
Comment[zh_HK]=簡單好用的文本編輯器
Comment[zh_TW]=簡單好用的文字編輯器
GenericName[ar]=محرر النصوص
GenericName[bo]=ཡིག་ཆ་རྩོམ་སྒྲུག་ཆས།
GenericName[br]=Aozer testennoù
GenericName[ca]=Editor de text
GenericName[cs]=Textový editor
GenericName[da]=Teksteditor
GenericName[de]=Text Editor
GenericName[es]=Editor de texto
GenericName[et]=Tekstiredaktor
GenericName[fi]=Tekstieditori
GenericName[fr]=Éditeur de texte
GenericName[gl_ES]=Editor de texto
GenericName[hr]=Uređivač teksta
GenericName[hu]=Szövegszerkesztő
GenericName[it]=Editor di Testo
GenericName[ko]=텍스트 편집기
GenericName[lt]=Tekstų redaktorius
GenericName[ms]=Penyunting Teks
GenericName[nl]=Tekstbewerker
GenericName[pl]=Edytor tekstu
GenericName[pt]=Editor de texto
GenericName[pt_BR]=Editor de Textos
GenericName[ru]=Текстовый редактор
GenericName[sq]=Përpunues Tekstesh
GenericName[sr]=Уређивач Текста
GenericName[tr]=Metin Düzenleyici
GenericName[ug]=تېكىست تەھرىرلىگۈچ
GenericName[uk]=Текстовий редактор
GenericName[zh_CN]=文本编辑器
GenericName[zh_HK]=文本編輯器
GenericName[zh_TW]=文字編輯器
MimeType=text/english;text/plain;text/x-makefile;text/x-c++hdr;text/x-c++src;text/x-chdr;text/x-csrc;text/x-java;text/x-moc;text/x-pascal;text/x-tcl;text/x-tex;application/x-shellscript;text/x-patch;text/x-adasrc;text/x-chdr;text/x-csrc;text/css;application/x-desktop;text/x-patch;text/x-fortran;text/html;text/x-java;text/x-tex;text/x-makefile;text/x-objcsrc;text/x-pascal;application/x-perl;application/x-perl;application/x-php;text/vnd.wap.wml;text/x-python;application/x-ruby;text/sgml;application/xml;model/vrml;image/svg+xml;application/json;
Name[ar]=محرر نصوص ديبين
Name[bo]=གཏིང་ཟབ་ཡིག་ཆ་རྩོམ་སྒྲིག་ཆས།
Name[br]=Aozer testennoù Deepin
Name[ca]=Editor de text del Deepin
Name[cs]=Deepin textový editor
Name[da]=Deepin teksteditor
Name[de]=Deepin Text Editor
Name[es]=Editor de texto
Name[et]=Deepin tekstiredaktor
Name[fi]=Deepin tekstieditori
Name[fr]=Éditeur de texte Deepin
Name[gl_ES]=Editor de texto Deepin
Name[hr]=Deepin uređivač teksta
Name[hu]=Deepin® Szövegszerkesztő
Name[it]=Editor di Testo di Deepin
Name[ko]=Deepin 텍스트 편집기
Name[lt]=Deepin tekstų redaktorius
Name[ms]=Penyunting Teks Deepin
Name[nl]=Deepin Tekstbewerker
Name[pl]=Edytor Tekstu Deepin
Name[pt]=Editor de texto Deepin
Name[pt_BR]=deepin Editor de Textos
Name[ru]=Текстовый Редактор Deepin
Name[sq]=Përpunues Tekstesh Deepin
Name[sr]=Дипин Уређивач Текста
Name[tr]=Deepin Metin Düzenleyici
Name[ug]=Deepin تېكىست تەھرىرلىگۈچ
Name[uk]=Текстовий редактор Deepin
Name[zh_CN]=文本编辑器
Name[zh_HK]=Deepin 文本編輯器
Name[zh_TW]=Deepin 文字編輯器
```



## 可识别的桌面输入键

键是可选的或必需的。如果一个键是可选的，它可能存在也可能不存在于文件中。但是，如果不是，则标准的实施不应失败，它必须提供一些合理的默认值。

某些键仅在另一个特定键也存在并设置为特定值的上下文中才有意义。如果特定键不存在或未设置为特定值，则不应使用这些键。例如，`Terminal`只有当 key 的值为 时才能使用`Type`key `Application`。

如果 REQUIRED 键仅在另一个键设置为特定值的上下文中有效，那么它必须仅在另一个键设置为特定值时才存在。例如，`URL`当且仅当键的值为 时，键必须`Type` 存在`Link`。

一些示例键：`Name[C]`, `Comment[it]`.

**表 2. 标准键**

|key|描述|值类型|请求？|类型|
|-|-|-|-|-|
|`Type`|该规范定义了 3 种类型的桌面条目：（ `Application`类型 1）、 `Link`（类型 2）和`Directory`（类型 3）。为了允许将来添加新类型，实现应该忽略具有未知类型的桌面条目。|string|是的||
|`Version`|桌面条目符合的桌面条目规范的版本。与此版本规范确认的条目应使用`1.5`. 请注意，版本字段不需要存在。|string|不|1-3|
|`Name`|应用程序的特定名称，例如“Mozilla”。|localestring|是的|1-3|
|`GenericName`|应用程序的通用名称，例如“Web Browser”。|localestring|不|1-3|
|`NoDisplay`|`NoDisplay`表示“此应用程序存在，但不在菜单中显示”。这对于例如将此应用程序与 MIME 类型相关联可能很有用，这样它就可以从文件管理器（或其他应用程序）启动，而无需为其提供菜单项（这有很多很好的理由，包括例如 `netscape -remote`, 或`kfmclient openURL`种东西）。|bool|不|1-3|
|`Comment`|条目的工具提示，例如“查看 Internet 上的站点”。该值不应与 和 的值 `Name`重复`GenericName`。|localestring|不|1-3|
|`Icon`|在文件管理器、菜单等中显示的图标。如果名称是绝对路径，将使用给定的文件。如果名称不是绝对路径，将使用[图标主题规范](http://freedesktop.org/wiki/Standards/icon-theme-spec)中描述的算法来定位图标。|iconstring|不|1-3|
|`Hidden`|`Hidden`应该叫`Deleted`. 这意味着用户删除了（在他的级别）存在的东西（在更高级别，例如在系统目录中）。`.desktop`就该用户而言，它严格等同于根本不存在的文件。这也可以用于“卸载”现有文件（例如由于重命名） - 通过`make install`安装其中包含的文件`Hidden=true`。|bool|不|1-3|
|`OnlyShowIn`,`NotShowIn`|标识应该显示/不显示给定桌面条目的桌面环境的字符串列表。<br><br>默认情况下，应显示桌面文件，除非存在 OnlyShowIn 键，在这种情况下，默认情况下不显示文件。<br><br>如果`$XDG_CURRENT_DESKTOP`设置了，则它包含一个以冒号分隔的字符串列表。按顺序考虑每个字符串。如果在其中找到匹配的条目， `OnlyShowIn`则会显示桌面文件。如果在其中找到条目，`NotShowIn`则不显示桌面文件。如果没有任何字符串匹配，则采取默认操作（如上）。<br><br>`$XDG_CURRENT_DESKTOP`应该由登录管理器根据`DesktopNames`在会话文件中找到的值进行设置。会话文件中的条目具有以通常方式分隔的多个值：使用分号。<br><br>同一桌面名称可能不会同时出现在 `OnlyShowIn`一个`NotShowIn`组中。|string|不|1-3|
|`DBusActivatable`|一个布尔值，指定此应用程序是否支持 D-Bus 激活。如果缺少此键，则默认值为`false`。如果值为，`true` 则实现应忽略该`Exec`键并发送 D-Bus 消息以启动应用程序。有关其工作原理的更多信息，请参阅[D-Bus 激活](https://specifications.freedesktop.org/desktop-entry-spec/latest/ar01s08.html)。应用程序仍应在其桌面文件中包含 Exec= 行，以便与不理解 DBusActivatable 密钥的实现兼容。|bool|不||
|`TryExec`|磁盘上用于确定程序是否实际安装的可执行文件的路径。如果路径不是绝对路径，则在 $PATH 环境变量中查找文件。如果文件不存在或不可执行，则该条目可能会被忽略（例如，不在菜单中使用）。|string|不|1|
|`Exec`|要执行的程序，可能带有参数。有关此 [`Exec`](https://specifications.freedesktop.org/desktop-entry-spec/latest/ar01s07.html)[密钥](https://specifications.freedesktop.org/desktop-entry-spec/latest/ar01s07.html)如何工作的详细信息，请参阅密钥。如果未设置为 ，则`Exec`需要密钥。即使是 ，也应该指定与不理解的实现兼容。 `DBusActivatable``true``DBusActivatable``true``Exec``DBusActivatable`|string|不|1|
|`Path`|如果 entry 是 type `Application`，则运行程序的工作目录。|string|不|1|
|`Terminal`|程序是否在终端窗口中运行。|bool|不|1|
|`Actions`|应用程序操作的标识符。这可用于告诉应用程序执行不同于默认行为的特定操作。[应用程序操作](https://specifications.freedesktop.org/desktop-entry-spec/latest/ar01s11.html) 部分描述了操作 的工作方式。|strings|不|1|
|`MimeType`|此应用程序支持的 MIME 类型。|strings|不|1|
|`Categories`|条目应显示在菜单中的类别（有关可能的值，请参见[桌面菜单规范](http://www.freedesktop.org/Standards/menu-spec)）。|strings|不|1|
|`Implements`|此应用程序实现的接口列表。默认情况下，桌面文件不实现任何接口。有关其工作原理的更多信息，请参阅[接口](https://specifications.freedesktop.org/desktop-entry-spec/latest/ar01s09.html)。|strings|不||
|`Keywords`|除了其他元数据之外，还可以使用字符串列表来描述此条目。这可能很有用，例如有助于通过条目进行搜索。这些值不用于显示，不应与 `Name`or的值重复`GenericName`。|localestring|不|1|
|`StartupNotify`|如果为 true，则已知应用程序将在设置了 DESKTOP_STARTUP_ID 环境变量时发送“删除”消息。如果为 false，则知道应用程序根本无法使用启动通知（不显示任何窗口，甚至在使用 StartupWMClass 时也会中断等）。如果不存在，合理的处理取决于实现（假设为 false，使用 StartupWMClass 等）。（有关详细信息，请参阅[启动通知协议规范](http://www.freedesktop.org/Standards/startup-notification-spec)）。|boolean|不|1|
|`StartupWMClass`|如果指定，则已知应用程序将使用给定字符串映射至少一个窗口作为其 WM 类或 WM 名称提示（有关详细信息， 请参阅[启动通知协议规范）。](http://www.freedesktop.org/Standards/startup-notification-spec)|string|不|1|
|`URL`|如果条目是链接类型，则要访问的 URL。|string|是的|2|
|`PrefersNonDefaultGPU`|如果为真，则应用程序更喜欢在功能更强大的独立 GPU 上运行（如果可用），我们在本规范中将其描述为“默认 GPU 以外的 GPU”，以避免需要定义独立 GPU 是什么以及在什么情况下它可能被认为比默认 GPU 更强大。这个键只是一个提示，可能不存在支持，具体取决于实现。|boolean|不|1|
|`SingleMainWindow`|如果为 true，则应用程序只有一个主窗口，并且不支持打开另一个主窗口。此键用于向实现发出信号，以避免提供 UI 来启动应用程序的另一个窗口。这个键只是一个提示，可能不存在支持，具体取决于实现。|boolean|不|1|


## `Exec` key

`Exec`key必须包含命令行 。命令行由一个可执行程序（可选地后跟一个或多个参数）组成。可执行程序可以使用其完整路径指定，也可以仅使用可执行程序的名称指定。如果未提供完整路径，则在桌面环境使用的 $PATH 环境变量中查找可执行文件。可执行程序的名称或路径不得包含等号 ("=")。参数由空格分隔。

参数可以全部引用。如果参数包含保留字符，则必须引用该参数。引用参数的规则也适用于所提供的可执行程序的可执行名称或路径。

引用必须通过将参数括在双引号之间并在双引号字符、反引号字符 ```、美元符号 `$` 和反斜杠字符 `\` 前面加上一个额外的反斜杠字符来转义。实现必须在扩展域代码和将参数传递给可执行程序之前撤消引用。保留字符有空格 `  `、制表符、换行符、双引号、单引号 `'`、反斜杠字符 `\`、大于号 `>`、小于号 `<`、波浪号 `~`、竖线 `|`、和号 `&`、分号 `;`、美元符号 `$`、星号 `*`、问号 `?`

请注意，字符串类型值的一般转义规则规定反斜杠字符也可以转义为 `\\`，并且此转义规则在引用规则之前应用。因此，要在桌面条目文件的引用参数中明确表示文字反斜杠字符，需要使用四个连续的反斜杠字符`\\\\`。同样，桌面条目文件中带引号的参数中的文字美元符号用 `\\$` 明确表示。

已经定义了许多特殊的域代码，当在命令行中遇到这些代码时，文件管理器或程序启动器将对其进行扩展。域代码由百分比字符 `%` 后跟一个字母字符组成。文字百分比字符必须转义为`%%`. 不推荐使用的域代码应从命令行中删除并忽略。域代码仅展开一次，用于替换域代码的字符串不应检查域代码本身。

包含本规范中未列出的域代码的命令行是无效的，不得处理，特别是实现可能不会引入对本规范中未列出的域代码的支持。扩展，如果有的话，应该通过一个新的键来引入。

除非本规范明确指示，否则实现必须注意不要将域代码扩展为多个参数。这意味着可以包含空格的名称字段、文件名和其他替换必须在展开后作为单个参数传递给可执行程序。

尽管`Exec`键被定义为具有字符串类型的值，它仅限于 ASCII 字符，但域代码扩展可能会在参数中引入非 ASCII 字符。实现必须注意传递给可执行程序的参数中的所有字符都根据适用的语言环境设置正确编码。

识别的域代码如下：

|代码|描述|
|-|-|
|`%f`|单个文件名（包括路径），即使选择了多个文件。读取桌面条目的系统应该认识到所讨论的程序无法处理多个文件参数，并且如果程序无法处理其他文件参数，它应该可能为每个选定的文件生成并执行程序的多个副本。如果文件不在本地文件系统上（即在 HTTP 或 FTP 位置上），则文件将被复制到本地文件系统并`%f`扩展为指向临时文件。用于不理解 URL 语法的程序。|
|`%F`|文件列表。用于可以一次打开多个本地文件的应用程序。每个文件都作为单独的参数传递给可执行程序。|
|`%u`|单个 URL。本地文件可以作为 file: URL 或作为文件路径传递。|
|`%U`|URL 列表。每个 URL 作为单独的参数传递给可执行程序。本地文件可以作为 file: URL 或作为文件路径传递。|
|`%i`|桌面条目的`Icon`键扩展为两个参数，首先 `--icon`是键的值 `Icon`。如果`Icon`键为空或丢失，则不应扩展为任何参数。|
|`%c`|`Name`桌面条目 中相应键中列出的应用程序的翻译名称。|
|`%k`|桌面文件的位置作为 URI（例如，如果从 vfolder 系统获取）或本地文件名，如果位置未知，则为空。|


## D-Bus 激活

支持由 D-Bus 启动的应用程序必须实现以下接口（以 D-Bus 自省 XML 格式给出）：

```XML
  <接口名称='org.freedesktop.Application'>
    <方法名称='激活'>
      <arg type='a{sv}' name='platform_data' 方向='in'/>
    </方法>
    <方法名='打开'>
      <arg type='as' name='uris' direction='in'/>
      <arg type='a{sv}' name='platform_data' 方向='in'/>
    </方法>
    <方法名称='ActivateAction'>
      <arg type='s' name='action_name' direction='in'/>
      <arg type='av' name='parameter' direction='in'/>
      <arg type='a{sv}' name='platform_data' 方向='in'/>
    </方法>
  </接口>
    
```

应用程序必须按照介绍部分中的命名建议命名其桌面文件（例如，文件名必须是 like `org.example.FooViewer.desktop`）。应用程序必须具有可激活的 D-Bus 服务，其众所周知的名称与`.desktop`删除部分的桌面文件名相同（对于我们的示例， `org.example.FooViewer`）。上述接口必须在如下确定的对象路径上实现：从应用程序的众所周知的 D-Bus 名称开始，将所有点更改为斜线并在斜线前缀。如果找到破折号 (' `-`')，请将其转换为下划线 (' `_`')。对于我们的示例，这是`/org/example/FooViewer`.

`Activate`当应用程序在没有要打开的文件的情况下启动时调用 该方法。

`Open`当应用程序使用文件启动时调用 该方法。字符串数组是 UTF-8 格式的 URI 数组。

激活[桌面操作](https://specifications.freedesktop.org/desktop-entry-spec/latest/ar01s11.html)时调用 该`ActivateAction`方法。参数是动作的名称 。`action-name`

所有方法都接受一个`platform-data`参数，该参数的使用方式与环境变量的使用方式类似。规范描述的当前字段是：

- `desktop-startup-id``DESKTOP_STARTUP_ID`：这应该是一个与环境变量中存储的值相同的字符串，由[启动通知协议规范](http://www.freedesktop.org/Standards/startup-notification-spec)指定。
- `activation-token`：这应该是与存储在`XDG_ACTIVATION_TOKEN`环境变量中的值相同的字符串，由 Wayland 的 [XDG 激活](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/main/staging/xdg-activation/xdg-activation-v1.xml)协议指定。

## 接口

`Implements`密钥可用于声明桌面文件实现的一个或多个接口 。

每个接口名称必须遵循用于 D-Bus 接口名称的规则，但除此之外，它们没有特殊含义。例如，在这里列出一个接口并不一定意味着该应用程序实现了该 D-Bus 接口，甚至不意味着存在这样的 D-Bus 接口。完全由定义特定接口的实体来定义实现它的含义。

虽然完全由接口的设计者来决定给定接口名称的含义，但这里有一些推荐的“最佳实践”：

- 接口应要求应用程序是 DBusActivatable，包括要求应用程序的桌面文件使用 D-Bus“反向 DNS”约定命名
- 接口名称应对应于应用程序在导出 org.freedesktop.Application 接口的同一对象路径上导出的 D-Bus 接口
- 如果接口希望允许有关实现的详细信息，则应指定实现者在其桌面文件中添加一个与接口同名的组（例如：“[org.freedesktop.ImageAcquire]”）

尽管有建议，接口可以指定几乎任何可以想象的要求，包括诸如“当通过 Exec 行启动时，应用程序应该呈现一个带有 _FOO_IDENTIFIER 属性集的窗口，此时 X 客户端消息将被发送到那个窗口”。另一个例子是“这个接口的所有实现都应该被标记为 NoDisplay，并且在启动时不会显示任何窗口，并且会在不确认的情况下删除所有用户的文件”。

接口定义者在设计他们的接口时应该注意保持向后和向前兼容性的问题。

## 注册 MIME 类型

该`MimeType`键用于指示应用程序知道如何处理的 MIME 类型。预计对于某些应用程序，此列表可能会变得很长。应用程序应该能够使用`Exec`密钥中列出的命令合理地打开这些类型的文件。

此字段中的 MIME 类型或桌面文件中的任何形式的优先级不应有优先级。应用程序的优先级在文件外部处理`.desktop`。

## 其他应用程序操作

Application 类型的桌面条目可以包含一个或多个操作。操作代表调用应用程序的另一种方式。应用程序启动器应在应用程序上下文中向用户公开它们（例如，作为子菜单）。这用于构建所谓的“快速列表”或“跳跃列表”。

### 动作标识符

每个动作都由一个字符串标识，遵循与键名相同的格式（参见[“条目”一节](https://specifications.freedesktop.org/desktop-entry-spec/latest/ar01s03.html#entries)）。每个标识符都与一个必须存在于`.desktop` 文件中的操作组相关联。动作组是一个名为 的组`Desktop Action %s`，其中`%s`是动作标识符。

为键中未提及的操作标识符设置操作组是无效的`Actions`。这样的行动组必须被实施者忽略。

### 操作键

每个操作组都支持以下键。如果操作组中不存在 REQUIRED 键，则实施者应忽略此操作。

**表 3. 特定于操作的键**

|钥匙|描述|值类型|请求？|
|-|-|-|-|
|`Name`|将显示给用户的标签。由于动作总是显示在特定应用程序的上下文中（即作为启动器的子菜单），因此它只需要在一个应用程序中明确且不应包含应用程序名称。|语言环境字符串|是的|
|`Icon`|与操作一起显示的图标。如果名称是绝对路径，则将使用给定的文件。如果名称不是绝对路径，将使用[图标主题规范](http://freedesktop.org/wiki/Standards/icon-theme-spec)中描述的算法来定位图标。实现可以选择忽略它。|图标字符串|不|
|`Exec`|执行此操作的程序，可能带有参数。有关此 [`Exec`](https://specifications.freedesktop.org/desktop-entry-spec/latest/ar01s07.html)[密钥](https://specifications.freedesktop.org/desktop-entry-spec/latest/ar01s07.html)如何工作的详细信息，请参阅密钥。如果在主桌面条目组中未设置为，`Exec`则需要该键。即使 是，也应该指定与不理解的实现兼容 。 `DBusActivatable``true``DBusActivatable``true``Exec``DBusActivatable`|细绳|不|


### 实施说明

应用程序操作应得到实施者的支持。但是，如果它们不受支持，实现者可以简单地忽略 `Actions`键和关联的`Desktop Action`操作组，并继续使用该`Desktop Entry`组：描述和调用应用程序的主要方式是通过 `Desktop Entry`组中的 Name、Icon 和 Exec 键。

预计显示应用程序列表的其他桌面组件（例如软件安装程序）不会为这些操作提供任何用户界面。因此，应用程序必须只包含作为通用启动器有意义的操作。


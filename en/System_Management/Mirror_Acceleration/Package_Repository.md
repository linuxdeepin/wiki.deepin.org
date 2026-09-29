---
title: Package Repository
description: 
published: true
date: 2026-09-29T09:47:46.701Z
tags: 
editor: markdown
dateCreated: 2026-09-29T09:47:46.701Z
---

<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/myml/mirrors@master/releases_zh_mark.css" />

- [Package Repository](./Package_Repository.md)
- [ISO Repository](./ISO_Repository.md)

## Official Package Repository

[https://community-packages.deepin.com/deepin/](https://community-packages.deepin.com/deepin/)

The official deepin 25 repository. The source line is `deb https://community-packages.deepin.com/deepin/beige/ crimson main commercial community`.

## How to switch the deepin 25 mirror source

Right after a new version is released, everyone updating at once congests the official repository and downloads slow down. If you are in a hurry, temporarily switch to one of the community mirrors below — it usually speeds things up noticeably.

```
# Huawei Cloud
deb https://mirrors.huaweicloud.com/deepin/beige/ crimson main commercial community
# HUST
deb https://mirrors.hust.edu.cn/deepin/beige/ crimson main commercial community
# Netease
deb https://mirrors.163.com/deepin/beige/ crimson main commercial community
```

The three lines above are examples only. Any mirror in the table below works just as well — simply replace the address part.

1. **Edit the source file:** `sudo nano /etc/apt/sources.list`
2. **Add a source:** paste one of the lines above as a **single line** into the editor; keep one source per line and do not let it wrap, otherwise errors are easy to make.
3. **Save and exit:** press `Ctrl+O` to write, Enter to confirm, then `Ctrl+X` to quit.
4. **Refresh:** run `sudo apt update` to apply the new source, then update as usual.

## Community Package Repository (deepin 25)

Sincerely thank the following universities, open-source communities and companies for providing deepin with mirror services! Every address below points at the repository root `…/deepin/beige/`, tested against the **crimson** (deepin 25) release.

Mirrors are listed alphabetically by country/region, with mirrors in China listed last and sorted by name. Pick a mirror close to you for the best speed.

<table>
  <tbody>
    <tr>
      <td><img src="https://www.deepin.org/wp-content/uploads/2026/09/australia-e1790671505139.png" alt="logo"> Australia</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>AARNet</td>
      <td><a href="http://mirror.aarnet.edu.au/deepin/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td><a href="https://mirror.aarnet.edu.au/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td><img src="https://www.deepin.org/wp-content/uploads/2026/09/bangladesh-e1790671519486.png" alt="logo"> Bangladesh</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>XeonBD</td>
      <td><a href="http://mirror.xeonbd.com/deepin/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td><img src="https://www.deepin.org/wp-content/uploads/flag/1473231824Belgium.jpg" alt="logo"> Belgium</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Belnet</td>
      <td><a href="http://ftp.belnet.be/mirror/deepin/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td><a href="ftp://ftp.belnet.be/mirror/deepin/beige/" target="_blank" rel="noopener noreferrer">ftp</a></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td><img src="https://www.deepin.org/wp-content/uploads/flag/1473231840Brazil.jpg" alt="logo"> Brazil</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Federal University of Parana (UFPR)</td>
      <td><a href="http://deepin.c3sl.ufpr.br/deepin/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td><a href="https://deepin.c3sl.ufpr.br/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td><img src="https://www.deepin.org/wp-content/uploads/flag/1473231864Bulgaria.jpg" alt="logo"> Bulgaria</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>IPACCT</td>
      <td><a href="http://deepin.ipacct.com/deepin/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td><a href="ftp://deepin.ipacct.com/deepin/beige/" target="_blank" rel="noopener noreferrer">ftp</a></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Netix Ltd</td>
      <td><a href="http://mirrors.netix.net/deepin/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td><a href="https://mirrors.netix.net/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td><a href="ftp://mirrors.netix.net/deepin/beige/" target="_blank" rel="noopener noreferrer">ftp</a></td>
      <td></td>
    </tr>
    <tr>
      <td><img src="https://www.deepin.org/wp-content/uploads/flag/1473231953Denmark.jpg" alt="logo"> Denmark</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>dotsrc.org</td>
      <td><a href="http://mirror.dotsrc.org/deepin/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td><a href="https://mirror.dotsrc.org/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td><a href="ftp://mirrors.dotsrc.org/deepin/" target="_blank" rel="noopener noreferrer">ftp</a></td>
      <td></td>
    </tr>
    <tr>
      <td><img src="https://www.deepin.org/wp-content/uploads/2016/12/france.jpg" alt="logo"> France</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>IRCAM</td>
      <td><a href="http://mirrors.ircam.fr/pub/deepin/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td><a href="https://mirrors.ircam.fr/pub/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td><img src="https://www.deepin.org/wp-content/uploads/flag/1473231998Germany.jpg" alt="logo"> Germany</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>xTom Germany</td>
      <td><a href="https://mirrors.xtom.de/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Alpix</td>
      <td><a href="http://mirror.alpix.eu/deepin/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td><a href="https://mirror.alpix.eu/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>FAU Erlangen-Nürnberg</td>
      <td><a href="http://ftp.fau.de/deepin/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td><a href="https://ftp.fau.de/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>GWDG</td>
      <td><a href="http://ftp.gwdg.de/pub/linux/linuxdeepin/deepin/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td><a href="ftp://ftp.gwdg.de/pub/linux/linuxdeepin/deepin/beige/" target="_blank" rel="noopener noreferrer">ftp</a></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>University of Erlangen-Nürnberg</td>
      <td><a href="http://ftp.uni-erlangen.de/deepin/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td><img src="https://www.deepin.org/wp-content/uploads/2026/09/indonesia-e1790671528838.png" alt="logo"> Indonesia</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Datautama Net Id Company</td>
      <td><a href="http://kartolo.sby.datautama.net.id/deepin/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td><img src="https://www.deepin.org/wp-content/uploads/flag/1473232055Italy.jpg" alt="logo"> Italy</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>GARR</td>
      <td><a href="http://deepin.mirror.garr.it/mirrors/deepin/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td><a href="https://deepin.mirror.garr.it/mirrors/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td><img src="https://www.deepin.org/wp-content/uploads/flag/1473232068Japan.jpg" alt="logo"> Japan</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>JAIST</td>
      <td><a href="http://ftp.jaist.ac.jp/pub/Linux/deepin/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td><img src="https://www.deepin.org/wp-content/uploads/flag/1473232015Holland.jpg" alt="logo"> Netherlands</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>xTom Netherlands</td>
      <td><a href="https://mirrors.xtom.nl/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>NLUUG</td>
      <td><a href="http://ftp.nluug.nl/os/Linux/distr/deepin/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td><a href="ftp://ftp.nluug.nl/pub/os/Linux/distr/deepin/beige/" target="_blank" rel="noopener noreferrer">ftp</a></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Studenten Net Twente</td>
      <td><a href="http://ftp.snt.utwente.nl/pub/os/linux/deepin/beige" target="_blank" rel="noopener noreferrer">http</a></td>
      <td><a href="ftp://ftp.snt.utwente.nl/pub/os/linux/deepin/beige" target="_blank" rel="noopener noreferrer">ftp</a></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td><img src="https://www.deepin.org/wp-content/uploads/flag/1473232154Russian.jpg" alt="logo"> Russia</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Yandex Linux mirror</td>
      <td><a href="http://mirror.yandex.ru/mirrors/deepin/packages/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td><img src="https://www.deepin.org/wp-content/uploads/flag/1473232178Slovakia.jpg" alt="logo"> Slovakia</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Rainside</td>
      <td><a href="http://tux.rainside.sk/deepin/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td><a href="ftp://tux.rainside.sk/deepin/beige/" target="_blank" rel="noopener noreferrer">ftp</a></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td><img src="https://www.deepin.org/wp-content/uploads/2016/12/Spain.jpg" alt="logo"> Spain</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>DeepinES</td>
      <td><a href="https://mirror.deepines.com/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td><img src="https://www.deepin.org/wp-content/uploads/flag/1473232216Sweden.jpg" alt="logo"> Sweden</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Academic Computer Club Umeå University</td>
      <td><a href="http://ftp.acc.umu.se/mirror/linuxdeepin/packages/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>c0urier.net</td>
      <td><a href="http://mirrors.c0urier.net/linux/deepin/packages/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td><a href="https://mirrors.c0urier.net/linux/deepin/packages/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Lysator ACS</td>
      <td><a href="http://ftp.lysator.liu.se/pub/deepin/packages/beige" target="_blank" rel="noopener noreferrer">http</a></td>
      <td><a href="https://ftp.lysator.liu.se/pub/deepin/packages/beige" target="_blank" rel="noopener noreferrer">https</a></td>
      <td><a href="ftp://ftp.lysator.liu.se/pub/deepin/packages/beige" target="_blank" rel="noopener noreferrer">ftp</a></td>
      <td></td>
    </tr>
    <tr>
      <td><img src="https://www.deepin.org/wp-content/uploads/2020/06/Switzerland.jpg" alt="logo"> Switzerland</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Hostsuisse (ENIDAN Technologies GmbH)</td>
      <td><a href="http://mirror.hostsuisse.com/deepin/packages/beige" target="_blank" rel="noopener noreferrer">http</a></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td><img src="https://www.deepin.org/wp-content/uploads/2018/10/Ukraine.jpg" alt="logo"> Ukraine</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>IP-Connect</td>
      <td><a href="https://deepin.ip-connect.vn.ua/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td><a href="ftp://deepin.ip-connect.vn.ua/mirror/deepin/beige/" target="_blank" rel="noopener noreferrer">ftp</a></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td><img src="https://www.deepin.org/wp-content/uploads/flag/1473231717America.jpg" alt="logo"> United States of America</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Linux Kernel Archives</td>
      <td><a href="http://mirrors.kernel.org/deepin/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Princeton University</td>
      <td><a href="http://mirror.math.princeton.edu/pub/deepin/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>CICKU</td>
      <td><a href="https://mirrors.cicku.me/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td><img src="https://www.deepin.org/wp-content/uploads/flag/1473231703China.jpg" alt="logo"> China</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Aliyun</td>
      <td><a href="http://mirrors.aliyun.com/deepin/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td><a href="https://mirrors.aliyun.com/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Beijing Foreign Studies University</td>
      <td><a href="http://mirrors.bfsu.edu.cn/deepin/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td><a href="https://mirrors.bfsu.edu.cn/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>CERNET Open Source Mirror</td>
      <td><a href="https://mirrors.cernet.edu.cn/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Chongqing University of Posts and Telecommunications</td>
      <td><a href="https://mirrors.cqupt.edu.cn/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Harbin Institute of Technology</td>
      <td><a href="http://mirrors.hit.edu.cn/deepin/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td><a href="https://mirrors.hit.edu.cn/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Huawei Cloud</td>
      <td><a href="https://mirrors.huaweicloud.com/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Huazhong University of Science and Technology</td>
      <td><a href="http://mirrors.hust.edu.cn/deepin/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td><a href="https://mirrors.hust.edu.cn/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Jilin University</td>
      <td><a href="https://mirrors.jlu.edu.cn/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Lanzhou University</td>
      <td><a href="https://mirror.lzu.edu.cn/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>MirrorZ</td>
      <td><a href="https://mirrors.mirrorz.org/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Nanjing University</td>
      <td><a href="http://mirrors.nju.edu.cn/deepin/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td><a href="https://mirrors.nju.edu.cn/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Nanyang Institute of Technology</td>
      <td><a href="https://mirror.nyist.edu.cn/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Netease</td>
      <td><a href="http://mirrors.163.com/deepin/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td><a href="https://mirrors.163.com/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Qilu University of Technology</td>
      <td><a href="https://mirrors.qlu.edu.cn/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Shanghai Jiao Tong University</td>
      <td><a href="http://ftp.sjtu.edu.cn/deepin/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td><a href="https://mirror.sjtu.edu.cn/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td><a href="ftp://ftp.sjtu.edu.cn/deepin/beige/" target="_blank" rel="noopener noreferrer">ftp</a></td>
      <td></td>
    </tr>
    <tr>
      <td>ShanghaiTech University</td>
      <td><a href="https://mirrors.shanghaitech.edu.cn/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Sun Yat-sen University</td>
      <td><a href="https://mirror.sysu.edu.cn/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Tsinghua University</td>
      <td><a href="http://mirrors.tuna.tsinghua.edu.cn/deepin/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td><a href="https://mirrors.tuna.tsinghua.edu.cn/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>University of Science and Technology of China</td>
      <td><a href="http://mirrors.ustc.edu.cn/deepin/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td><a href="https://mirrors.ustc.edu.cn/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Volcano Engine</td>
      <td><a href="https://mirrors.volces.com/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Zhejiang University</td>
      <td><a href="http://mirrors.zju.edu.cn/deepin/beige/" target="_blank" rel="noopener noreferrer">http</a></td>
      <td><a href="https://mirrors.zju.edu.cn/deepin/beige/" target="_blank" rel="noopener noreferrer">https</a></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>

## How to provide a mirror

| Repository | Sync Command | Disk Space |
| --- | --- | --- |
| Packages | rsync -av --delete-after rsync.deepin.com::deepin/ /var/www/deepin/ | 600GB or more |
| ISO Images | rsync -av --delete-after rsync.deepin.com::releases/ /var/www/deepin-cd/ | 120GB or more |

**Notes:**

* Every mirror on this page was re-verified on September 29, 2026; unreachable mirrors, expired domains and mirrors that no longer carry deepin images have been removed.
* Some mirrors have directory indexing disabled (a browser request returns 403), but their repositories and ISO files are still downloadable and work fine with apt.
* Every mirror above was verified to serve the `crimson` repository metadata (`Release` or `InRelease`); a few publish only `InRelease`.
* "CERNET Open Source Mirror" is a dispatcher: requests are redirected to the nearest member mirror on the education network (for example Sun Yat-sen University or Shandong University).
* You may move the /var/www/ path in the addresses above to the root directory of your server;
* Please add a daily cron job so that the deepin mirror you provide stays up to date;
* We recommend syncing the deepin package repository first, and the deepin ISO repository afterwards;
* Please do not put any other files (for example unofficial packages) in the deepin mirror directories, to avoid confusion;
* If you have any suggestions or comments, please contact [support@deepin.org](mailto:support@deepin.org).
* You can also submit a mirror at [wiki:Mirrors](/en/System_Management/Mirror_Acceleration/ISO_Repository).
browser-skill
中断
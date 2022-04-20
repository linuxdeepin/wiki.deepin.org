[[zh:时间和时区]]


## Summary

An operating system determine the exact time according to the following parameters:

* Time value
* Time standards
* Timezone
* Adjustment of summer time (adopted in some countries / regions)

## Basic concepts

### Classification of clocks

There are two clocks in Linux system: one is the system clock, and the other is real time clock (RTC).

The real time clock is powered by a battery on the motherboard, and can be adjust through BIOS settings. It holds the value of year, month, day, hour, minute and second, and no other information (like usage of UTC or local time, or summer time).

The System clock is the clock in Linux kernel. After the kernel has been loaded, it reads the RTC from hardware and calculate the time for system clock using configurations from /etc/adjtime. After that, the system clock run independently from the RTC, and is maintained by clock interruption. All commands and functions in Linux adopt the system clock, as it preservers the exact time as well as settings of timezone and summer time.

The time value held by Linux kernel is in fact an integer, which equals to the total seconds elapsed from January 1, 1970. Because Linux systems of 32-bit use a 32-bit integer to represent the time, they may malfunction in the year 2038 as it is the furthest moment that a 32-bit integer can represent.

### Time standard

There are two time standard in Linux system: the local time and the Universal Time Coordinated(UTC).

The UTC is a standard independent of timezones. Though the definiton of the UTC is different from that of the GMT, they adopt a identical time value.

The local time, on the other hand, is closely related to the selected timezone.

The operating system determines the time standard to use. By default, Windows uses local times, Mac OS uses the UTC, and the Unix-like system have both. It is recommended to use the UTC for hardware clock in Linux and unify the time value for all operating system on the computer, so that Linux will automatically adjust the summer time while the other operating systems will not.

To check the current time settings, execute in terminal:

    timedatectl status | grep local

Use command "hwclock" to set hardware clock using local times:

    timedatectl set-local-rtc 1

To set hardware clock using the UTC:

    timedatectl set-local-rtc 0

The above commands will also update file "/etc/adjtime".

After the RTC driver has been installed, the operating system may set hardware clock, according to the platform, the kernel version and compilation configurations. If so, the hardware clock is assumed to be of UTC standard, and the content of "sys/class/rtc/rtcN/hctosys" (where N=0,1,2,..) will be set to "1". After that, the upstart scripts will also re-adjust the system clock according to file "/etc/adjtime". This may cause wield behavior when the hardware clock is of local time.

It is generally unsafe to set system time backward.

### RTC clock

Real-time clock (RTC) is a kind of electronic device that outputs the real time like mechanical clocks. It is implemented in form of integrated circuit, is it is also known as clock chip.

The task of time management for most operating system consists of:

* Set the system clock according to the hardware clock during startup

* Adjust the system clock through NTP daemon when running

* Write back to the hardware clock during shutdown

## Settings

### Using graphical interface

Open Control Center -> Time and date, where you can view the current date and time, as well as adjust the time and the timezone.

### Using command

There are four commands commonly used for viewing and adjust time in Linux: date, hwclock, clock and timedatectl. The usage of "clock" is similar to that of "hwclock", except "clock" has extra support of Alpha platform.

#### Date

##### timedatectl

Syntax:

    timedatectl [Options] {Command}

To set the hardware clock (using local time) and the system clock, execute in terminal:

        timedatectl status

To set the system time:

        timedatectl set-time "2012-10-30 18:17:16"
       
##### date

Syntax:

    date [-u] [-d datestr] [-s datestr] [--utc] [--universal] [--date=datestr] [--set=datestr] [--help]
       [--version] [+FORMAT] [MMDDhhmm[[CC]YY][.ss]]

Only administrators have permission to adjust date and time with "date" command. Normal users can only view the time.

This command with simply display the system time if no option is given.

       $ date
       Thu Aug 17 11:09:17 CST 2017

To set the date as 12:00 in June 18, 2010:

       # date -s "20100618 12:00:00"
       The June 18 12:00:00 CST 2010

To set the date as June 18, 2010:

       # date -s 20100618
       The June 18 00:00:00 CST 2010

To set the time as 12:00:00:

       # date -s 12:00:00
       Thu Aug 17 12:00:00 CST 2017

##### hwclock/clock

Syntax:

    hwclock [--adjust][--debug][--directisa][--hctosys][--show][--systohc][--test]
       [--utc][--version][--set --date=<DateAndTime>]

Options:

       --adjust hwclock Each time changing the hardware clock, system will record the delta of the current hardware clock in file /etc/adjtime. Use this option to automatically adjust current hardware clock according to the previous delta
       --debug Show details during executing
       --directisa hwclock Read / write hardware clock using direct I/O command, instead of accessing file "/dev/rtc"
       --hctosys Synchronize the system clock with the hardware clock
       --set --date=<DateAndTime> Set the hardware clock
       --show Show current hardware clock
       --systohc Synchronize the hardware clock with the system clock
       --test Do a test, without changing any clock
       --utc Use the UTC standard for time conversion

To show the hardware clock:

        hwclock --show

or

        clock --show

To set the hardware clock:

        hwclock --set --date="07/07/06 10:19"  ## The date format is: Month/Day/Year Hour:Minute:Second

or

        clock --set --date="07/07/06 10:19"  ## The date format is: Month/Day/Year Hour:Minute:Second

To synchronize the system clock with  the hardware clock manually:

        hwclock --hctosys ## Read the time from the hardware clock and write it to the system clock

or
        clock –hctosys

To synchronize the system clock with  the hardware clock manually:

        hwclock --systohc ## Read the time from the system clock and write it to the hardware clock

or
        clock –systohc

#### Timezone

##### date

To show the current timezone

       $ date -R
       Thu, 17 Jun 2010 00:01:36 +0800

In the output above, "+0800" indicates the timezone of "+8 hours".

##### tzselect

For example, to set the timezone as Asia/Shangai:

       $ tzselect
       Please identify a location so that time zone rules can be set correctly.
       Please select a continent or ocean.
       1) Africa
       2) Americas
       3) Antarctica
       4) Arctic Ocean
       5) Asia
       6) Atlantic Ocean
       7) Australia
       8) Europe
       9) Indian Ocean
       10) Pacific Ocean
       11) none - I want to specify the time zone using the Posix TZ format.
       #? Input 5 for "Asia"
       Please select a country.
        1) Afghanistan           18) Israel                35) Palestine
        2) Armenia               19) Japan                 36) Philippines
        3) Azerbaijan            20) Jordan                37) Qatar
        4) Bahrain               21) Kazakhstan            38) Russia
        5) Bangladesh            22) Korea (North)         39) Saudi Arabia
        6) Bhutan                23) Korea (South)         40) Singapore
        7) Brunei                24) Kuwait                41) Sri Lanka
        8) Cambodia              25) Kyrgyzstan            42) Syria
        9) China                 26) Laos                  43) Taiwan
       10) Cyprus                27) Lebanon               44) Tajikistan
       11) East Timor            28) Macau                 45) Thailand
       12) Georgia               29) Malaysia              46) Turkmenistan
       13) Hong Kong             30) Mongolia              47) United Arab Emirates
       14) India                 31) Myanmar (Burma)       48) Uzbekistan
       15) Indonesia             32) Nepal                 49) Vietnam
       16) Iran                  33) Oman                  50) Yemen
       17) Iraq                  34) Pakistan
       #? Input 9 for "China"
       Please select one of the following time zone regions.
       1) east China - Beijing, Guangdong, Shanghai, etc.
       2) Heilongjiang
       3) central China - Gansu, Guizhou, Sichuan, Yunnan, etc.
       4) Tibet & most of Xinjiang Uyghur
       5) southwest Xinjiang Uyghur
       #? Input 1 for "Beijing time"
       The following information has been given:
       China
       east China - Beijing, Guangdong, Shanghai, etc.
       Therefore TZ='Asia/Shanghai' will be used.
       Local time is now:
       Fri Jul 7 10:32:18 CST 2006.
       Universal Time is now: Fri Jul 7 02:32:18 UTC 2006.
       Is the above information OK?
       1) Yes
       2) No
       #? Input 1 for "Yes"

##### timedatectl

Syntax:

    timedatectl [Options] {Command}

To check the timezone used:

        timedatectl status

To show available timezones:

        timedatectl list-timezones

Modify the timezone:

        timedatectl set-timezone <Zone>/<SubZone>

## Frequently asked question

### Disable UTC time for system

deepin disabled the UTC time. If it is enabled, a time difference (usually +8 hours, for Chinese users) will be yielded between deepin and Windows on the same machine. This problem can not be solved by modifying configuration files.

If you really need to do so, execute in terminal:

        sudo gedit /etc/default/rcS

and change the line

    UTC=yes

to

    UTC=no

Then save your changes and reboot the system.

### Use NTP to synchronize the time

Network Time Protocol (NTP) is a protocol used to synchronize times among computers. It provides high accuracy in synchronization between clients and servers (or other clock source like quartz clock or GPS), and the error is less than 1 millisecond (on LAN) or dozens of milliseconds (on WAN). It can also be used with encryption to avoid protocol attacks.

To use NTP to synchronize the time, install ntp package first:

        sudo apt-get install ntp

NTP should now function normally. To configure NTP service:

        sudo gedit /etc/ntp.conf

Restart NTP service for changes to take effect:

        sudo service ntp restart

To seen if NTP service is doing synchronization:

        sudo ntpq -c lpeer

To view the synchronization log:

        sudo tail -f /var/log/syslog


## References

[ubuntu中文百科:老旧文章/禁止系统日期时间设为UTC](http://wiki.ubuntu.org.cn/%E8%80%81%E6%97%A7%E6%96%87%E7%AB%A0/%E7%A6%81%E6%AD%A2%E7%B3%BB%E7%BB%9F%E6%97%A5%E6%9C%9F%E6%97%B6%E9%97%B4%E8%AE%BE%E4%B8%BAUTC)

[Linux时间设置全攻略](http://hi.baidu.com/dujiaopeng/item/419185353fa60f322e0f81b2)

[Arch Wiki: time](https://wiki.archlinux.org/index.php/Time)
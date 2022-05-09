---
title: 待分类/Video_card
description: 
published: true
date: 2022-05-07T07:48:27.942Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:44:12.379Z
---

##Introduction
A video card (also called a display card, graphics card, display adapter or graphics adapter) is an expansion card which generates a feed of output images to a display (such as a computer monitor). Frequently, these are advertised as discrete or dedicated graphics cards, emphasizing the distinction between these and integrated graphics.

##History
Standards such as MDA, CGA, HGC, Tandy, PGC, EGA, VGA, MCGA, 8514 or XGA were introduced from 1982 to 1990 and supported by a variety of hardware manufacturers.

The majority of modern video cards are built with either AMD-sourced or Nvidia-sourced graphics chips.[1] Until 2000, 3dfx Interactive was also an important, and often groundbreaking, manufacturer. Most video cards offer various functions such as accelerated rendering of 3D scenes and 2D graphics, MPEG-2/MPEG-4 decoding, TV output, or the ability to connect multiple monitors (multi-monitor). Video cards also have sound card capabilities to output sound – along with the video for connected TVs or monitors with integrated speakers.

Graphics cards weren't very useful for early computers, since they didn't have the capability to run graphic-based games or high-resolution videos as modern computers do now.

Within the industry, video cards are sometimes called graphics add-in-boards, abbreviated as AIBs,[1] with the word "graphics" usually omitted.

##Dedicated vs integrated graphics

Classical desktop computer architecture with a distinct graphics card over PCI Express. Typical bandwidths for given memory technologies, missing are the memory latency. Zero-copy between GPU and CPU is not possible, since both have their distinct physical memories. Data must be copied from one to the other to be shared.

Integrated graphics with partitioned main memory: a part of the system memory is allocated to the GPU exclusively. Zero-copy is not possible, data has to be copied, over the system memory bus, from one partition to the other.

Integrated graphics with unified main memory, to be found AMD "Kaveri" or PlayStation 4 (HSA).
As an alternative to the use of a video card, video hardware can be integrated into the motherboard or the CPU. Both approaches can be called integrated graphics. Motherboard-based implementations are sometimes called "on-board video" while CPU-based implementations are known as accelerated processing units (APUs). Almost all desktop computer motherboards with integrated graphics allow the disabling of the integrated graphics chip in BIOS, and have a PCI, or PCI Express (PCI-E) slot for adding a higher-performance graphics card in place of the integrated graphics. The ability to disable the integrated graphics sometimes also allows the continued use of a motherboard on which the on-board video has failed. Sometimes both the integrated graphics and a dedicated graphics card can be used simultaneously to feed separate displays. The main advantages of integrated graphics include cost, compactness, simplicity and low energy consumption. The performance disadvantage of integrated graphics arises because the graphics processor shares system resources with the CPU. A dedicated graphics card has its own random access memory (RAM), its own cooling system, and dedicated power regulators, with all components designed specifically for processing video images. Upgrading to a dedicated graphics card offloads work from the CPU and system RAM, so not only will graphics processing be faster, but the computer's overall performance may also improve.

Both of the dominant CPU makers, AMD and Intel, are moving to APUs. One of the reasons is that graphics processors are powerful parallel processors, and placing them on the CPU die allows their parallel processing ability to be harnessed for various computing tasks in addition to graphics processing. (See Heterogeneous System Architecture, which discusses AMD's implementation.) APUs are the newer integrated graphics technology and, as costs decline, will probably be used instead of integrated graphics on the motherboard in most future low and mid-priced home and business computers. As of late 2013, the best APUs provide graphics processing approaching mid-range mobile video cards[2] and are adequate for casual gaming. Users seeking the highest video performance for gaming or other graphics-intensive uses should still choose computers with dedicated graphics cards. (See Size of market and impact of accelerated processing units on video card sales, below.)

Beyond the enthusiast segment is the market for professional video cards for workstations used in the special effects industry, and in fields such as design, analysis and scientific research. Nvidia is a major player in the professional segment. In November, 2013, AMD introduced a so-called "Supercomputing" graphics card "designed for data visualization in finance, oil exploration, aeronautics and automotive, design and engineering, geophysics, life sciences, medicine and defense."[3]

##Power demand
As the processing power of video cards has increased, so has their demand for electrical power. Current high-performance video cards tend to consume a great deal of power. For example, the thermal design power (TDP) for the GeForce GTX TITAN is 250 Watts.[4] While CPU and power supply makers have recently moved toward higher efficiency, power demands of GPUs have continued to rise, so the video card may be the biggest electricity user in a computer.[5][6] Although power supplies are increasing their power too, the bottleneck is due to the PCI-Express connection, which is limited to supplying 75 Watts.[7] Modern video cards with a power consumption over 75 Watts usually include a combination of six-pin (75 W) or eight-pin (150 W) sockets that connect directly to the power supply. Providing adequate cooling becomes a challenge in such computers. Computers with multiple video cards may need power supplies in the 1000–1500 W range. Heat extraction becomes a major design consideration for computers with two or more high end video cards.

##Size
Video cards for desktop computers come in one of two size profiles, which can allow a graphics card to be added even to small-sized PCs. Some video cards are not of usual size, and are thus categorized as being low profile.[8][9] Video card profiles are based on width only, with low-profile cards taking up less than the width of a PCIe slot. Length and thickness can vary greatly, with high-end cards usually occupying two or three expansion slots, and with dual-GPU cards -such as the Nvidia GeForce GTX 690- generally exceeding 10" in length.[10]

##Multi-card scaling
Some graphics cards can be linked together to allow scaling of the graphics processing across multiple cards. This is done using either the PCIe bus on the motherboard, or, more commonly, a data bridge. Generally, the cards must be of the same model to be linked, and most low power cards are not able to be linked in this way.[11] AMD and Nvidia both have proprietary methods of scaling, CrossFireX for AMD, and SLI for Nvidia. Cards from different chipset manufacturers, architectures cannot be used together for multi card scaling. If a graphics card has different sizes of memory, the lowest value will be used, with the higher values being disregarded. Currently, scaling on consumer grade cards can be done using up to four cards.[12][13][14]

##Graphic drivers
A Graphic driver usually supports one or multiple cards, and has to be specifically written for an operating system.

##Industry
As of 2016, the primary suppliers of the GPUs (video chips or chipsets) used in video cards are AMD and Nvidia. In the third quarter of 2013, AMD had a 35.5% market share while Nvidia had a 64.5% market share,[16] according to Jon Peddie Research. In economics, this industry structure is termed a duopoly. AMD and Nvidia also build and sell video cards, which are termed graphics add-in-board (AIBs) in the industry. (See Comparison of Nvidia graphics processing units and Comparison of AMD graphics processing units.) In addition to marketing their own video cards, AMD and Nvidia sell their GPUs to authorized AIB suppliers, which AMD and Nvidia refer to as "partners".[1] The fact that Nvidia and AMD compete directly with their customer/partners complicates relationships in the industry. The fact that AMD and Intel are direct competitors in the CPU industry is also noteworthy, since AMD-based video cards may be used in computers with Intel CPUs. Intel's move to APUs may weaken AMD, which until now has derived a significant portion of its revenue from graphics components. As of the second quarter of 2013, there were 52 AIB suppliers.[1] These AIB suppliers may market video cards under their own brands, or produce video cards for private label brands or produce video cards for computer manufacturers. Some AIB suppliers such as MSI build both AMD-based and Nvidia-based video cards. Others, such as EVGA, build only Nvidia-based video cards, while XFX, now builds only AMD-based video cards. Several AIB suppliers are also motherboard suppliers. The largest AIB suppliers, based on global retail market share for graphics cards, include Taiwan-based Palit Microsystems, Hong Kong-based PC Partner (which markets AMD-based video cards under its Sapphire brand and Nvidia-based video cards under its Zotac brand), Taiwan-based computer-maker Asustek Computer (Asus), Taiwan-based Micro-Star International (MSI), Taiwan-based Gigabyte Technology,[17] Brea, California, USA-based EVGA (which also sells computer components such as power supplies) and Ontario, California USA-based XFX. (The parent corporation of XFX is based in Hong Kong.)

##Size of market and impact of accelerated processing units on video card sales
Video card shipments totaled 14.5 million units in the third quarter of 2013, a 17% fall from Q3 2012 levels.[16] The traditional PC market is shrinking as tablet computers and smartphones gain share. Years ago, the move to integrated graphics on the motherboard greatly reduced the market for low end video cards. Now, AMD and Intel's accelerated processing units, which combine graphics processing with CPU functions on the CPU die itself, are putting further pressure on video card sales.[17] AMD introduced a line of combined processors which it calls the AMD A-Series APU Processors (A4, A6, A8, A10) while Intel, rather than marketing an exclusive line of APUs, introduced its "4th Generation Intel® Core™ Processors", some of which are APUs. Those processors are described as offering "Superb visuals and graphics performance–without the cost of a separate graphics card."[18] They are branded as having Intel HD Graphics or Intel Iris Pro Graphics. As an example, the Intel Core i7 4750HQ with Iris Pro Graphics 5200, an accelerated processing unit for notebook computers, allows users with mid-range graphics requirements to use a notebook computer without a video card. In a September, 2013 review of the Intel Core i7 4750HQ accelerated processing unit (which is closely related to the Intel processor with HD Graphics 5000 used in the MacBook Air,) the website hardware.info stated: "With its latest generation of integrated graphics, Intel set out to rival the performance of the mid-range mobile Nvidia GeForce GT 650M graphics card. And the tests leave no doubt about it, both 3DMark and the gaming benchmarks confirm that the [Intel] Iris Pro Graphics 5200 is on the same level of or slightly below that of the GT 650M."[2] (The GeForce GT 650M is not sold through retail channels, but an EVGA desktop GTX 650 was selling for around $120 in late 2013.[19]) Although the review notes that Intel's accelerated processing unit is not yet cost competitive, the technology is approaching competitiveness, at least with mid-range mobile dedicated video. (A video benchmarking website that tabulates user-submitted benchmarks shows Intel Iris Pro Graphics 5200, based on a very small sample of 8 submissions, scoring a G3D Mark of 912,[20] versus 1296 for the Nvidia GeForce GT 650M,[21] with higher scores being better. If the benchmark is linear, that puts the Iris Pro Graphics 5200's performance at about 70% of the GeForce GT 650M Intel was targeting. AMD's A10-5750M mobile APU with Radeon HD 8650G graphics scores 858 on this graphics benchmark.[22]) With anticipated price reductions, it is predicted that APUs will eventually replace low to mid-range dedicated video implementations. That will leave only the high-end enthusiast and professional market segments for video card vendors.

##Parts

A Radeon HD 7970 with the heatsink removed, showing the major components of the card.
A modern video card consists of a printed circuit board on which the components are mounted. These include:

###Graphics Processing Unit
Main article: graphics processing unit
A graphics processing unit (GPU), also occasionally called visual processing unit (VPU), is a specialized electronic circuit designed to rapidly manipulate and alter memory to accelerate the building of images in a frame buffer intended for output to a display. Because of the large degree of programmable computational complexity for such a task, a modern video card is also a computer unto itself.

###Heat sink
A heat sink is mounted on most modern graphics cards. A heat sink spreads out the heat produced by the graphics processing unit evenly throughout the heat sink and unit itself. The heat sink commonly has a fan mounted as well to cool the heat sink and the graphics processing unit. Not all cards have heat sinks, for example, some cards are liquid cooled, and instead have a waterblock; additionally, cards from the 1980s and early 1990s did not produce much heat, and did not require heatsinks.

###Video BIOS
The video BIOS or firmware contains a minimal program for initial set up and control of the video card. It may contain information on the memory timing, operating speeds and voltages of the graphics processor, RAM, and other details which can sometimes be changed. The usual reason for doing this is to overclock the video card to allow faster video processing speeds, however, this has the potential to irreversibly damage the card with the possibility of cascaded damage to the motherboard.

The modern Video BIOS does not support all the functions of the video card, being only sufficient to identify and initialize the card to display one of a few frame buffer or text display modes. It does not support YUV to RGB translation, video scaling, pixel copying, compositing or any of the multitude of other 2D and 3D features of the video card.

###Video memory

The memory capacity of most modern video cards ranges from 1 GB to 12 GB.[23] Since video memory needs to be accessed by the GPU and the display circuitry, it often uses special high-speed or multi-port memory, such as VRAM, WRAM, SGRAM, etc. Around 2003, the video memory was typically based on DDR technology. During and after that year, manufacturers moved towards DDR2, GDDR3, GDDR4, GDDR5 and GDDR5X. The effective memory clock rate in modern cards is generally between 1 GHz to 10 GHz.

Video memory may be used for storing other data as well as the screen image, such as the Z-buffer, which manages the depth coordinates in 3D graphics, textures, vertex buffers, and compiled shader programs.

###RAMDAC
The RAMDAC, or random-access-memory digital-to-analog converter, converts digital signals to analog signals for use by a computer display that uses analog inputs such as cathode ray tube (CRT) displays. The RAMDAC is a kind of RAM chip that regulates the functioning of the graphics card. Depending on the number of bits used and the RAMDAC-data-transfer rate, the converter will be able to support different computer-display refresh rates. With CRT displays, it is best to work over 75 Hz and never under 60 Hz, in order to minimize flicker.[24] (With LCD displays, flicker is not a problem.[citation needed]) Due to the growing popularity of digital computer displays and the integration of the RAMDAC onto the GPU die, it has mostly disappeared as a discrete component. All current LCD/plasma monitors and TVs and projectors with only digital connections, work in the digital domain and do not require a RAMDAC for those connections. There are displays that feature analog inputs (VGA, component, SCART, etc.) only. These require a RAMDAC, but they reconvert the analog signal back to digital before they can display it, with the unavoidable loss of quality stemming from this digital-to-analog-to-digital conversion.[25] With VGA standard being phased out in favor of digital, RAMDACs are beginning to disappear from video cards.[citation needed]

jebeli vam mamu

###Output interfaces

Video In Video Out (VIVO) for S-Video (TV-out), Digital Visual Interface (DVI) for High-definition television (HDTV), and DB-15 for Video Graphics Array (VGA)
The most common connection systems between the video card and the computer display are:

####Video Graphics Array (VGA) (DE-15)

Video Graphics Array (VGA) (DE-15).
Main article: Video Graphics Array
Also known as D-sub, VGA is an analog-based standard adopted in the late 1980s designed for CRT displays, also called VGA connector. Some problems of this standard are electrical noise, image distortion and sampling error in evaluating pixels. Today, the VGA analog interface is used for high definition video including 1080p and higher. While the VGA transmission bandwidth is high enough to support even higher resolution playback, there can be picture quality degradation depending on cable quality and length. How discernible this quality difference is depends on the individual's eyesight and the display; when using a DVI or HDMI connection, especially on larger sized LCD/LED monitors or TVs, quality degradation, if present, is prominently visible. Blu-ray playback at 1080p is possible via the VGA analog interface, if Image Constraint Token (ICT) is not enabled on the Blu-ray disc.

####Digital Visual Interface (DVI)

Digital Visual Interface (DVI-I).
Main article: Digital Visual Interface
Digital-based standard designed for displays such as flat-panel displays (LCDs, plasma screens, wide high-definition television displays) and video projectors. In some rare cases high end CRT monitors also use DVI. It avoids image distortion and electrical noise, corresponding each pixel from the computer to a display pixel, using its native resolution. It is worth noting that most manufacturers include a DVI-I connector, allowing (via simple adapter) standard RGB signal output to an old CRT or LCD monitor with VGA input.

####Video In Video Out (VIVO) for S-Video, Composite video and Component video
Main article: S-Video
Included to allow connection with televisions, DVD players, video recorders and video game consoles. They often come in two 10-pin mini-DIN connector variations, and the VIVO splitter cable generally comes with either 4 connectors (S-Video in and out + composite video in and out), or 6 connectors (S-Video in and out + component PB out + component PR out + component Y out [also composite out] + composite in).

####High-Definition Multimedia Interface (HDMI)

High-Definition Multimedia Interface (HDMI)
Main article: HDMI
HDMI is a compact audio/video interface for transferring uncompressed video data and compressed/uncompressed digital audio data from an HDMI-compliant device ("the source device") to a compatible digital audio device, computer monitor, video projector, or digital television.[26] HDMI is a digital replacement for existing analog video standards. HDMI supports copy protection through HDCP.

####DisplayPort

DisplayPort
Main article: DisplayPort
DisplayPort is a digital display interface developed by the Video Electronics Standards Association (VESA). The interface is primarily used to connect a video source to a display device such as a computer monitor, though it can also be used to transmit audio, USB, and other forms of data.[27] The VESA specification is royalty-free. VESA designed it to replace VGA, DVI, and LVDS. Backward compatibility to VGA and DVI by using adapter dongles enables consumers to use DisplayPort fitted video sources without replacing existing display devices. Although DisplayPort has a greater throughput of the same functionality as HDMI, it is expected to complement the interface, not replace it.[28][29]

####Other types of connection systems
Composite video	Analog systems with resolution lower than 480i use the RCA connector. The single pin connector carries all resolution, brightness and color information, making it the lowest quality dedicated video connection.[30]
Composite-video-cable.jpg
Component video	It has three cables, each with RCA connector (YCBCR for digital component, or YPBPR for analog component); it is used in older projectors, video-game consoles, DVD players.[31] It can carry SDTV 480i and EDTV 480p resolutions, and HDTV resolutions 720p and 1080i, but not 1080p due to industry concerns about copy protection. Contrary to popular belief it looks equal to HDMI for the resolutions it carries,[32] but for best performance from Blu-ray, other 1080p sources like PPV, and 4K Ultra HD, a digital display connector is required.

###Motherboard interfaces
Main articles: Bus (computing) and Expansion card
Chronologically, connection systems between video card and motherboard were, mainly:

S-100 bus: Designed in 1974 as a part of the Altair 8800, it was the first industry-standard bus for the microcomputer industry.
ISA: Introduced in 1981 by IBM, it became dominant in the marketplace in the 1980s. It was an 8 or 16-bit bus clocked at 8 MHz.
NuBus: Used in Macintosh II, it was a 32-bit bus with an average bandwidth of 10 to 20 MB/s.
MCA: Introduced in 1987 by IBM it was a 32-bit bus clocked at 10 MHz.
EISA: Released in 1988 to compete with IBM's MCA, it was compatible with the earlier ISA bus. It was a 32-bit bus clocked at 8.33 MHz.
VLB: An extension of ISA, it was a 32-bit bus clocked at 33 MHz. Also referred to as VESA.
PCI: Replaced the EISA, ISA, MCA and VESA buses from 1993 onwards. PCI allowed dynamic connectivity between devices, avoiding the manual adjustments required with jumpers. It is a 32-bit bus clocked 33 MHz.
UPA: An interconnect bus architecture introduced by Sun Microsystems in 1995. It had a 64-bit bus clocked at 67 or 83 MHz.
USB: Although mostly used for miscellaneous devices, such as secondary storage devices and toys, USB displays and display adapters exist.
AGP: First used in 1997, it is a dedicated-to-graphics bus. It is a 32-bit bus clocked at 66 MHz.
PCI-X: An extension of the PCI bus, it was introduced in 1998. It improves upon PCI by extending the width of bus to 64 bits and the clock frequency to up to 133 MHz.
PCI Express: Abbreviated PCIe, it is a point to point interface released in 2004. In 2006 provided double the data-transfer rate of AGP. It should not be confused with PCI-X, an enhanced version of the original PCI specification.
In the attached table[33] is a comparison between a selection of the features of some of those interfaces.
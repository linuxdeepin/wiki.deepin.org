## Definition
Random-access memory (RAM /ræm/) is a form of computer data storage which stores frequently used program instructions to increase the general speed of a system. A random-access memory device allows data items to be read or written in almost the same amount of time irrespective of the physical location of data inside the memory. 

<br>In contrast, with other direct-access data storage media such as hard disks, CD-RWs, DVD-RWs and the older drum memory, the time required to read and write data items varies significantly depending on their physical locations on the recording medium, due to mechanical limitations such as media rotation speeds and arm movement.</br>

<br>RAM contains multiplexing and demultiplexing circuitry, to connect the data lines to the addressed storage for reading or writing the entry. Usually more than one bit of storage is accessed by the same address, and RAM devices often have multiple data lines and are said to be '8-bit' or '16-bit' etc. devices.</br>

<br>In today's technology, random-access memory takes the form of integrated circuits. RAM is normally associated with volatile types of memory (such as DRAM memory modules), where stored information is lost if power is removed, although non-volatile RAM has also been developed.[1] Other types of non-volatile memories exist that allow random access for read operations, but either do not allow write operations or have other kinds of limitations on them. These include most types of ROM and a type of flash memory called NOR-Flash.</br>

<br>Integrated-circuit RAM chips came into the market in the early 1970s, with the first commercially available DRAM chip, the Intel 1103, introduced in October 1970.</br>

##Types 
###Static RAM (SRAM) 
###Dynamic RAM (DRAM).
<br> In SRAM, a bit of data is stored using the state of a six transistor memory cell. This form of RAM is more expensive to produce, but is generally faster and requires less dynamic power than DRAM. In modern computers, SRAM is often used as cache memory for the CPU. DRAM stores a bit of data using a transistor and capacitor pair, which together comprise a DRAM memory cell. The capacitor holds a high or low charge (1 or 0, respectively), and the transistor acts as a switch that lets the control circuitry on the chip read the capacitor's state of charge or change it. As this form of memory is less expensive to produce than static RAM, it is the predominant form of computer memory used in modern computers.</br>

<br>Both static and dynamic RAM are considered volatile, as their state is lost or reset when power is removed from the system. By contrast, read-only memory (ROM) stores data by permanently enabling or disabling selected transistors, such that the memory cannot be altered. Writeable variants of ROM (such as EEPROM and flash memory) share properties of both ROM and RAM, enabling data to persist without power and to be updated without requiring special equipment. These persistent forms of semiconductor ROM include USB flash drives, memory cards for cameras and portable devices, etc. ECC memory (which can be either SRAM or DRAM) includes special circuitry to detect and/or correct random faults (memory errors) in the stored data, using parity bits or error correction code.</br>

<br>In general, the term RAM refers solely to solid-state memory devices (either DRAM or SRAM), and more specifically the main memory in most computers. In optical storage, the term DVD-RAM is somewhat of a misnomer since, unlike CD-RW or DVD-RW it does not need to be erased before reuse. Nevertheless, a DVD-RAM behaves much like a hard disc drive if somewhat slower.</br>

##Memory Cell
The memory cell is the fundamental building block of computer memory. The memory cell is an electronic circuit that stores one bit of binary information and it must be set to store a logic 1 (high voltage level) and reset to store a logic 0 (low voltage level). Its value is maintained/stored until it is changed by the set/reset process. The value in the memory cell can be accessed by reading it.

<br>In SRAM, the memory cell is a type of flip-flop circuit, usually implemented using FETs. This means that SRAM requires very low power when not being accessed, but it is expensive and has low storage density.</br>

<br>A second type, DRAM, is based around a capacitor. Charging and discharging this capacitor can store a '1' or a '0' in the cell. However, this capacitor will slowly leak away, and must be refreshed periodically. Because of this refresh process, DRAM uses more power, but it can achieve greater storage densities and lower unit costs compared to SRAM.</br>

-DRAM Cell (1 Transistor and one capacitor)

-SRAM Cell (6 Transistors)
##Addressing
To be useful, memory cells must be readable and writeable. Within the RAM device, multiplexing and demultiplexing circuitry is used to select memory cells. Typically, a RAM device has a set of address lines A0... An, and for each combination of bits that may be applied to these lines, a set of memory cells are activated. Due to this addressing, RAM devices virtually always have a memory capacity that is a power of two.

<br>Usually several memory cells share the same address. For example, a 4 bit 'wide' RAM chip has 4 memory cells for each address. Often the width of the memory and that of the microprocessor are different, for a 32 bit microprocessor, eight 4 bit RAM chips would be needed.</br>

<br>Often more addresses are needed than can be provided by a device. In that case, external multiplexors to the device are used to activate the correct device that is being accessed.</br>

##Memory Hierarchy
One can read and over-write data in RAM. Many computer systems have a memory hierarchy consisting of processor registers, on-die SRAM caches, external caches, DRAM, paging systems and virtual memory or swap space on a hard drive. This entire pool of memory may be referred to as "RAM" by many developers, even though the various subsystems can have very different access times, violating the original concept behind the random access term in RAM. Even within a hierarchy level such as DRAM, the specific row, column, bank, rank, channel, or interleave organization of the components make the access time variable, although not to the extent that access time to rotating storage media or a tape is variable. The overall goal of using a memory hierarchy is to obtain the highest possible average access performance while minimizing the total cost of the entire memory system (generally, the memory hierarchy follows the access time with the fast CPU registers at the top and the slow hard drive at the bottom).

<br>In many modern personal computers, the RAM comes in an easily upgraded form of modules called memory modules or DRAM modules about the size of a few sticks of chewing gum. These can quickly be replaced should they become damaged or when changing needs demand more storage capacity. As suggested above, smaller amounts of RAM (mostly SRAM) are also integrated in the CPU and other ICs on the motherboard, as well as in hard-drives, CD-ROMs, and several other parts of the computer system.</br>

###Virtual Memory
Most modern operating systems employ a method of extending RAM capacity, known as "virtual memory". A portion of the computer's hard drive is set aside for a paging file or a scratch partition, and the combination of physical RAM and the paging file form the system's total memory. (For example, if a computer has 2 GB of RAM and a 1 GB page file, the operating system has 3 GB total memory available to it.) When the system runs low on physical memory, it can "swap" portions of RAM to the paging file to make room for new data, as well as to read previously swapped information back into RAM. Excessive use of this mechanism results in thrashing and generally hampers overall system performance, mainly because hard drives are far slower than RAM.

###RAM Disk
Software can "partition" a portion of a computer's RAM, allowing it to act as a much faster hard drive that is called a RAM disk. A RAM disk loses the stored data when the computer is shut down, unless memory is arranged to have a standby battery source.

###Shadow RAM
Sometimes, the contents of a relatively slow ROM chip are copied to read/write memory to allow for shorter access times. The ROM chip is then disabled while the initialized memory locations are switched in on the same block of addresses (often write-protected). This process, sometimes called shadowing, is fairly common in both computers and embedded systems.

<br>As a common example, the BIOS in typical personal computers often has an option called “use shadow BIOS” or similar. When enabled, functions relying on data from the BIOS’s ROM will instead use DRAM locations (most can also toggle shadowing of video card ROM or other ROM sections). Depending on the system, this may not result in increased performance, and may cause incompatibilities. For example, some hardware may be inaccessible to the operating system if shadow RAM is used. On some systems the benefit may be hypothetical because the BIOS is not used after booting in favor of direct hardware access. Free memory is reduced by the size of the shadowed ROMs.</br>

##Recent Developments
Several new types of non-volatile RAM, which will preserve data while powered down, are under development. The technologies used include carbon nanotubes and approaches utilizing Tunnel magnetoresistance. Amongst the 1st generation MRAM, a 128 KiB (128 × 210 bytes) chip was manufactured with 0.18 µm technology in the summer of 2003.[citation needed] In June 2004, Infineon Technologies unveiled a 16 MiB (16 × 220 bytes) prototype again based on 0.18 µm technology. There are two 2nd generation techniques currently in development: thermal-assisted switching (TAS)[8] which is being developed by Crocus Technology, and spin-transfer torque (STT) on which Crocus, Hynix, IBM, and several other companies are working.[9] Nantero built a functioning carbon nanotube memory prototype 10 GiB (10 × 230 bytes) array in 2004. Whether some of these technologies will be able to eventually take a significant market share from either DRAM, SRAM, or flash-memory technology, however, remains to be seen.

<br>Since 2006, "solid-state drives" (based on flash memory) with capacities exceeding 256 gigabytes and performance far exceeding traditional disks have become available. This development has started to blur the definition between traditional random-access memory and "disks", dramatically reducing the difference in performance.</br>

<br>Some kinds of random-access memory, such as "EcoRAM", are specifically designed for server farms, where low power consumption is more important than speed.</br>

##Memory Wall
The "memory wall" is the growing disparity of speed between CPU and memory outside the CPU chip. An important reason for this disparity is the limited communication bandwidth beyond chip boundaries, which is also referred to as bandwidth wall. From 1986 to 2000, CPU speed improved at an annual rate of 55% while memory speed only improved at 10%. Given these trends, it was expected that memory latency would become an overwhelming bottleneck in computer performance.

<br>CPU speed improvements slowed significantly partly due to major physical barriers and partly because current CPU designs have already hit the memory wall in some sense. Intel summarized these causes in a 2005 document.</br>

“First of all, as chip geometries shrink and clock frequencies rise, the transistor leakage current increases, leading to excess power consumption and heat... Secondly, the advantages of higher clock speeds are in part negated by memory latency, since memory access times have not been able to keep pace with increasing clock frequencies. Third, for certain applications, traditional serial architectures are becoming less efficient as processors get faster (due to the so-called Von Neumann bottleneck), further undercutting any gains that frequency increases might otherwise buy. In addition, partly due to limitations in the means of producing inductance within solid state devices, resistance-capacitance (RC) delays in signal transmission are growing as feature sizes shrink, imposing an additional bottleneck that frequency increases don't address.”

<br>The RC delays in signal transmission were also noted in Clock Rate versus IPC: The End of the Road for Conventional Microarchitectures which projects a maximum of 12.5% average annual CPU performance improvement between 2000 and 2014.</br>

<br>A different concept is the processor-memory performance gap, which can be addressed by 3D integrated circuits that reduce the distance between the logic and memory aspects that are further apart in a 2D chip.[13] Memory subsystem design requires a focus on the gap, which is widening over time.[14] The main method of bridging the gap is the use of caches; small amounts of high-speed memory that houses recent operations and instructions nearby the processor, speeding up the execution of those operations or instructions in cases where they are called upon frequently. Multiple levels of caching have been developed in order to deal with the widening of the gap, and the performance of high-speed modern computers are reliant on evolving caching techniques.[15] These can prevent the loss of processor performance, as it takes less time to perform the computation it has been initiated to complete.[16] There can be up to a 53% difference between the growth in speed of processor speeds and the lagging speed of main memory access.</br>
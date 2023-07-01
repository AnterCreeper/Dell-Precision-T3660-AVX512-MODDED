# Dell-Precision-T3660-AVX512-MODDED  
#### A description on how to mod the bios to re-enable the AVX512  
一份说明关于如何修改 Dell Precision T3660 工作站 BIOS固件 以 跳过 对AVX512的禁用
BootGuard?

From the investigation, the BIOS has no design about switches of enabling or disabling AVX512. So it is done by a procedure program. So here is some information about cracking it.
经过分析，固件没有设计AVX512的选项（无论隐藏与否），因此AVX512的禁用是由程序写死，于是产生此教程。

警告：考虑到各种可能，请务必备份！请务必备份！本人不对结果负责！  
CAUTION! BACKUP AND LOOK BEHIND!
注意此机器共有两片BIOS闪存，一大一小共两个。
There are two flash together, one is 32MB and another is 16MB.

方法：
Method:
1. 检查微码版本，确认其小于等于0x15。
1. Check the microcode version, make sure that it should be lower or equal than 0x15.
   
2. 物理上检查CPU的Batch Code, 确认其物理上支持AVX512。本人的CPU为i5-12500 六大核, 步进为5。有小核的请关掉小核。
2. Check the Batch Code on the Surface of the CPU by physically view it's top IHS, and make sure that it should be the old one which is not fused and support AVX512. Mine is i5-12500, with 6 cores. The stepping is 0x5. If yours have several atom efficiency cores, make sure to disable them.
   
3. 直接使用物理编程器，导出全部数据。电压为3.3V,但是最好按照芯片的说明书检查一下。
3. Use a Programmer to dump the data from those two flash. The voltage should be 3.3V, but you may check it from the chip's datasheet.

////////////////////////////////////
4. dump use UEFITool? 

4. 分析 SiInitFsp
   B9 AF 00 00 00 mov eax 0xAF
   0F 30 wrmsr
0xAF is an undocumented MSR Register, and Bit0 for disabling both AVX2 and AVX-512 features, Bit1 is for AVX-512 disabling. So, if you have 0xAF=0x2, then only AVX-512 is disabled.

5. 分析 一共有两个树，一个是AVX禁用，而有 83 C8 02   or ecx 2 的是AVX512禁用。修改之，为 83 C8 00 or ecx 0。

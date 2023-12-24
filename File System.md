## RAID
man mdadm 
	SYNOPSIS
		   mdadm [mode] <raiddevice> [options] <component-devices>
	
	DESCRIPTION
		   RAID  devices  are virtual devices created from two or more real block devices.  This allows
		   multiple devices (typically disk drives or partitions thereof) to be combined into a  single
		   device  to  hold (for example) a single filesystem.  Some RAID levels include redundancy and
		   so can survive some degree of device failure.


## LVM
we can't  expect how many space we will use on which we are about partition disk. so,  some bad thing occur in the feature.   e.g `/` is about fullfill but others partition remain lots of space. 
How to resist this ?  it's time to  `LVM` 

一般而言，在生产环境中无法精确地评估每个硬盘分区在日后的使用情况，因此会导致
原先分配的硬盘分区不够用。比如，伴随着业务量的增加，用于存放交易记录的数据库目录
的体积也随之增加；因为分析并记录用户的行为从而导致日志目录的体积不断变大，这些都
会导致原有的硬盘分区在使用上捉襟见肘。而且，还存在对较大的硬盘分区进行精简缩容的
情况。
我们可以通过部署 LVM 来解决上述问题。部署 LVM 时，需要逐个配置物理卷、卷组和
逻辑卷。常用的部署命令如表 7-2 所示。



¾ 在进行路由选择前处理数据包（PREROUTING）；
¾ 处理流入的数据包（INPUT）；
¾ 处理流出的数据包（OUTPUT）；
¾ 处理转发的数据包（FORWARD）；
¾ 在进行路由选择后处理数据包（POSTROUTING）。

what is diff betw rej and drop? 
when you `ping` 
	Reject    -> unreachable
	Drop      ->  timeout 




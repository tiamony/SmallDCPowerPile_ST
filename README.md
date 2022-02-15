﻿[12/09 2021] 
程序版本：V1.0.0

蔚来小功率3.3KW双向直流充电桩初始版本。

[02/09 2022] 
程序版本：V1.0.1

修改：
1. Station_V2L从发3秒改为一直发送到接收到BRM报文；
2. BRM中RESERVED信号蔚来车型会发送0xBA，设备通过该信号判断是否是蔚来车型
3. VCU在收到Station_V2L后开始发送Veh_RES报文，直至本次流程结束
4. Station_V2L在每次开始流程时必须发送
5. 当设备放电时，Station_V2L发送后3m内未收到Veh_RES报文，继续往下走流程，当收到BRM中RESERVED信号为0xBA，则结束流程，CST报桩端故障
6. 当设备放电时，Station_V2L发送后3m内未收到Veh_RES报文，继续往下走流程，当收到BRM中RESERVED信号不为0xBA，则认为是其他车继续流程
7. 当设备放电时，Station_V2L发送后3m内收到Veh_RES中VCUV2LDischargingSts为4，则停止流程
8. 当设备充电时，收到Veh_RES报文且VCUV2LDischargingSts不为4，则停止流程。

[02/15 2022] 
程序版本：V1.0.2

问题：
    充电过程中断掉插头在设备休眠前再插上插头我记得是可以再次充电的。昨天测试再插上插头后设备既不休眠也不充电。
修改：
    1.把这种情况当做可恢复的故障来处理，AC端输入电压< 170V 持续1.5S触发输入端掉电故障，当电压升高到>200V时持续1.5S故障恢复，进入空闲检测（若检测到插枪会再次进入充电流程）。
    2.因掉电解锁功能（电子锁）逻辑上存在冲突，注释掉次部分程序，当掉电故障发生时，逻辑上模块辅源持续2.5S以上即可给电子锁解锁。
        






# SDIO&FATFS

[STM32L4新版HAL库SDIO（DMA）、FatFs使用教程（前言）_zl199203的博客-CSDN博客](https://blog.csdn.net/zl199203/article/details/83513704)

### 纯SDIO

[STM32L4新版HAL库SDIO（DMA）、FatFs使用教程（一）_zl199203的博客-CSDN博客](https://blog.csdn.net/zl199203/article/details/83513768)

## SDIO+DMA

[STM32L4新版HAL库SDIO（DMA）、FatFs使用教程（二）_zl199203的博客-CSDN博客](https://blog.csdn.net/zl199203/article/details/83514030)

- 需要注意在写数据与读数据之间要加入一段时间的延时

![SDIO&FATFS%20dcdcbf9fb78047809c386cbf5b05e717/CleanShot_2021-04-11_at_11.45.022x.png](CleanShot_2021-04-11_at_11.45.022x.png)

## FATFS

[STM32L4新版HAL库SDIO（DMA）、FatFs使用教程（三）_zl199203的博客-CSDN博客](https://blog.csdn.net/zl199203/article/details/83514105)

- 官方的Bug已经解决了，不需要自己添加代码了

## FATFS+RTOS

[STM32L4新版HAL库SDIO（DMA）、FatFs使用教程（四）_zl199203的博客-CSDN博客](https://blog.csdn.net/zl199203/article/details/83514223)

- 官方的Bug已经解决了，不需要自己添加代码了
- 如果发生卡死现象，调整RTOS→kernel settings→MINIMAL_STACK_SIZE即可
    
    ![SDIO&FATFS%20dcdcbf9fb78047809c386cbf5b05e717/CleanShot_2021-04-11_at_11.48.202x.png](CleanShot_2021-04-11_at_11.48.202x.png)
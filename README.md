# Real-Time-Implementation-of-Stereo-Vision
This script is a trial version of replication of RuiFan's paper [Real-Time Implementation of Stereo Vision Based on Optimised Normalised Cross-Correlation and Propagated Search Range on a GPU](https://ieeexplore.ieee.org/document/8261486)

If you find this code useful in your research, please consider citing:
```sh
@article{fan_dahnoun_2017,
title={Real-time implementation of stereo vision based on optimised normalised cross-correlation and propagated search range on a GPU}, 
DOI={10.1109/ist.2017.8261486}, 
journal={2017 IEEE International Conference on Imaging Systems and Techniques (IST)}, 
author={Fan, Rui and Dahnoun, Naim},
year={2017}}
```
## Requirement
Environment request: python 3.7.6
```sh
pip install <below_packages>
```
1. numpy
2. sklearn (not mandatory)
3. cv2
4. matplotlib
5. scipy (not mandatory)
6. numba (suggested)

My laptop has a `i7-4702HQ`  CPU, detailed hardware information
```sh
H/W path                 Device           Class          Description
====================================================================
                                          system         Dell Precision M3800 (D
/0                                        bus            Dell Precision M3800
/0/0                                      memory         64KiB BIOS
/0/2f                                     processor      Intel(R) Core(TM) i7-47
/0/2f/4                                   memory         256KiB L1 cache
/0/2f/5                                   memory         1MiB L2 cache
/0/2f/6                                   memory         6MiB L3 cache
/0/7                                      memory         8GiB System Memory
/0/7/0                                    memory         4GiB SODIMM DDR3 Synchr
/0/7/1                                    memory         4GiB SODIMM DDR3 Synchr
/0/100                                    bridge         Xeon E3-1200 v3/4th Gen
/0/100/1                                  bridge         Xeon E3-1200 v3/4th Gen
/0/100/1/0                                display        GK107GLM [Quadro K1100M
/0/100/2                                  display        4th Gen Core Processor 
/0/100/3                                  multimedia     Xeon E3-1200 v3/4th Gen
/0/100/4                                  generic        Intel Corporation

架构：           x86_64
CPU 运行模式：   32-bit, 64-bit
字节序：         Little Endian
CPU:             8
在线 CPU 列表：  0-7
每个核的线程数： 2
每个座的核数：   4
座：             1
NUMA 节点：      1
厂商 ID：        GenuineIntel
CPU 系列：       6
型号：           60
型号名称：       Intel(R) Core(TM) i7-4702HQ CPU @ 2.20GHz
步进：           3
CPU MHz：        897.292
CPU 最大 MHz：   3200.0000
CPU 最小 MHz：   800.0000
BogoMIPS：       4390.13
虚拟化：         VT-x
L1d 缓存：       32K
L1i 缓存：       32K
L2 缓存：        256K
L3 缓存：        6144K
NUMA 节点0 CPU： 0-7
```
## To Use
### numba
Please visit [numba](https://github.com/numba/numba) to see full version of instruction.
To turn on numba speed-up, please uncomment the line above each function block
```sh
@jit(nopython=True)  <----
def func()
    ```
    ```
    return
```

## Structure
### Step1. Feature extraction and remove outliers(cell_1)

**Input:** left, right images

**Return:** matches and filtered_match

### Step2. Perspective transformation code(cell_2)

**Input:**  filtered_match

**Return:** slope and intercept, shifted image pairs

### Step3. Disparity Old (cell_3)
The old version contains a straight calculation of **Normalized Cross Correlation**(NCC), the function was built under function (1) of the original paper. Meanwhile, integral image of mean and SD is calculated for index calculation under paper's suggestion.

**Input:**  shifted image pairs

**Return:** Disparity
the calculation speed of the disparity part takes **30ms**
(UPDATED UNTIL 8/19/2020)

### Step3. Disparity New (cell_4)

The new version contains a faster calculation of NCC with the help of cv2, the function was built under function (8) of the original paper. The 'faster' conclusion comes from the performance comparison in cell_6 and cell_7 (Please uncomment #line 73-74 in cell_7 ).

**Input:**  shifted image pairs

**Return:** Disparity
the calculation speed of the disparity part takes **50ms**, the overall performance is not as good as the old version.
(UPDATED UNTIL 8/19/2020)

| Runtime (ms) | 8/19/2020 |  |  |
| ------ | ------ |------ |------ |
| Old NCC |  30|------ |------ |
| New NCC | 50 |------ |------ |

## Know Issues 
**Q: Why numba?**

A: 
* Speed of code run using numba is comparable to that of similar code in C, C++ or Fortran.
* Fastmath, Parallel,  Intel SVML supported.
See also:

[A ~5 minute guide to Numba](https://numba.pydata.org/numba-doc/latest/user/5minguide.html) 

[Performance Tips](https://numba.pydata.org/numba-doc/latest/user/performance-tips.html#performance-tips)

[Speed Up your Algorithms Part 2— Numba](https://towardsdatascience.com/speed-up-your-algorithms-part-2-numba-293e554c5cc1)

**Q: Once turn on all comments marked on numba line (speed up all), especially the main `Left_Disparity_Map()`, the error of numba would pop up.**

A: While concatenate the search range under `line 11` of the `Algorithm 2: Left disparity map estimation`, some recieved temporal disparity list may empty. We have to provide the explicit dtype to numba. Works still need to be done.


## To do
1. Keep programming until parallel calculation and @jit works for full code.
2. Write the code in C++ or CUDA C.

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

### Step2. Perspective transformation(cell_2)

**Input:**  filtered_match

**Return:** slope and intercept, shifted image pairs

### Step3. Disparity Old(cell_3)
The old version contains a straight calculation of **Normalized Cross Correlation**, the function was built under function (1) of the original paper. Meanwhile, integral image of mean and SD is calculated for index calculation under paper's suggestion.

**Input:**  shifted image pairs

**Return:** Disparity
the calculation speed of the disparity part takes 30ms

### Step3. Disparity New(cell_4)

The new version contains a faster calculation of **Normalized Cross Correlation** with the help of cv2, the function was built under function (8) of the original paper. The 'faster' conclusion comes from the performance comparison in cell_6 and cell_7 (Please uncomment #line 73-74 in cell_7 ).

**Input:**  shifted image pairs

**Return:** Disparity
the calculation speed of the disparity part takes 50ms, the overall performance is not good as the old version.

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
To turn off, please comment the line above each function block
```sh
# @jit(nopython=True)  <----
def func()
    ```
    ```
    return
```

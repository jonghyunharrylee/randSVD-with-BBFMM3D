##Quick Start Guide for mexBBFMM3D, a MATLAB interface for <a href="http://www.sciencedirect.com/science/ article/pii/S0021999109004665"> Black Box Fast Multipole Method (BBFMM3D)  
==========
###Function
Perform fast (linear) multiplication of a kernel matix Q with a vector or matrix: P = QH

###Overview
The Fast Multipole Method (FMM) is an algorithm that performs fast multiplication of an N x N dense matrix Q(x,y) where N is the number of unknown values at points (x,y) in a 2D domain, with a matrix H of size N x m (N>>m). The direct multiplication approach has complexity O(N^2), while the BBFMM2D has linear complexity O(N). As an example, for a matrix of size 10,000x10,000 multiplied to 10,000 x 100, the BBFMM2D takes 1.4 seconds, while the direct approach takes 8.1 seconds, on a single processor laptop. The table below shows computation time on a single core CPU.

|   N      |  Time in seconds  |     
| -------: |:-----------------:|   
| 10,000   |                1.4|
| 100,000  |               11.4|  
| 1,000,000 |             126.2 |

BBFMM3D performs the multiplication by an approximation that relies on Chebyshev interpolation. For details about the method please see Fong and Darve, 2009. The method has an approximation error that can be controlled by input parameters, which can be adjusted depending on the desired accuracy.  The package BBFMM3D is written in C++ (https://github.com/ruoxi-wang/BBFMM3D). mexBBFMM3D provides a MATLAB interface for the package BBFMM3D. The corresponding code for two dimensional cases can be found at https://github.com/judithyueli/mexBBFMM2D.


###Disclaimer

This is a quick-start guide with instructions on how to set up and use mexBBFMM3D in MATLAB, with two example m-file that can be used to perform matrix-vector and matrix-matrix multiplication for regular and irregular grids. A reasonably good knowledge of MATLAB is assumed and minimal understanding of the FMM theory is needed.  

For a more involved description of the code and the method please see [here](https://github.com/ruoxi-wang/BBFMM3D), and for a full description of the algorithm see [Fong and Darve 2006] in section [__Reference__](#ref).

In this guide, we will use the example of the multiplication of a Gaussian covariance matrix Q (termed as Gaussian kernel) with a matrix H. The method can also be applied for other smooth kernels (see section [__Appendix__](#ref_app)).

Please cite the following paper if you use this code:

Fong, William, and Eric Darve. "The black-box fast multipole methodshod." Journal of Computational Physics 228, no. 23 (2009): 8712-8725. You can see details <a href="http://www.sciencedirect.com/science/article/pii/S0021999109004665">here</a>.

###2. DIRECTORIES AND FILES


	./example1.m		:	Example of how to use mexBBFMM3D for isotropic, regular grid
        ./example2.m		:	Example of how to use mexBBFMM3D for anisotropic, irregular large scale grid 
	./make.m		:	Makefile 
	./include/		:	Relevant header files  
	./mexFMM3D.cpp	        :	mex function  
	./eigen/		:	Eigen Library  
	./BBFMM3D/		: 	BBFMM3D library
	./README.md		:	This file  
	./Troubleshooting.md    :       Instructions for troubleshooting compilation problems
	
###Quick start guide

####Step 1:  Download the code and supporting software

Supporting software includes the fft library (http://www.fftw.org/fftw-2.1.5.tar.gz) and Matlab SDK files. 

####Step 1a:  Check if you have MEX and MATLAB Symbolic Math Toolbox set up in Matlab

This package relies on MATLAB MEX functions and MATLAB Symbolic Math Toolbox. In order to use MEX functions, you should setup mex.

Setup MEX by typing the following MATLAB command
```
      mex -setup  
```

Attention Mac users: If you have upgraded to Xcode 7, see [Answer](http://www.mathworks.com/matlabcentral/answers/246507-why-can-t-mex-find-a-supported-compiler-in-matlab-r2015b-after-i-upgraded-to-xcode-7-0) by MathWorks Support Team on 28 Dec 2015.

Once mex is set up successfully, to ensure that  MEX is installed, try the following commands:
```
copyfile([matlabroot,'/extern/examples/mex/arraySize.c'],'./','f')     
mex -v arraySize.c     
arraySize(5000) 
```

The last two lines of the screen output should read  

```
Dimensions: 5000x5000
Size of  array in kilobytes: 24414
```

If you have trouble with setting up mex, see __Trouble Shooting.md__

####Step 1b:  Download the code from https://github.com/amaliak/randSVD-with-BBFMM3D

A folder called `randSVD-with-BBFMM3D-master` will be downloaded. Copy the folder `randSVD-with-BBFMM3D-master` to your specific working directory. In MATLAB, set the current folder to be the directory where the downloaded folder was copied. You will see the folders
`BBFMM3D\` and `Eigen\`, as well as an m-file called (`compilemex.m`) and two example m-files (`example1.m` and `example2.m`)  that we will use in this quick start quide. These m-files will be used to compile, set-up, test and use the `BBFMM3D`. The user only needs to change the input to the example m-files. No modifications will be needed to other m-files or the contents of the folder `BBFMM3D` which includes the c++ source code. 

Note: For Step 2 you have to operate in the main directory of mexBBFMM3D, which contains `make.m`, For Step 3 or 4, you can call the generated MEX-files (e.g., `ExecName.mexmaci64`) by moving them to your own working directory, or add the main directory of mexBBFMM3D to the path.

####Step 2: Compile the MEX file










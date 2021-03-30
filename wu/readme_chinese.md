用 户 手 册

                                   "G R O W T H 3.0"                                                           
          
                            重力异常三维反演地下密度结构的软件 
     
                作者 Antonio G. Camacho 
                支持 Jose Fernandez                                      
                   Instituto de Geociencias (IGEO), CSIC-UCM
                   Calle del Doctor Severo Ochoa, 7
                   Ciudad Universitaria.
                   28040 Madrid (Spain)            
                   e-mails:  antonio_camacho@mat.ucm.es, jft@mat.ucm.es




1. 简介

对重力资料进行三维反演的计算工具. 

该方法的主要特点如下: 

  (1) 求解三维空间问题, 
  (2) 接受非网格非平面低精度不准确数据, 
  (3) 可以进行无约束和有约束的计算, 
  (4) 根据数据自动确定区域重力异常线性趋势, 
  (5) 自动计算密度对比度值 
  (6) 正负密度异常同时反演, 
  (7) 由多个独立单元组成的不规则形状结构的反演, 
  (8) 能够反演向下密度增加的情况, 
  (9) 能够反演层状结构, 
 (10) 半自动数据反演程序. 

软件由两部分组成: 

 (a) “GROWTH” 文件执行反演过程得到三维密度异常模型,  
 (b) “VIEW” 文件用于直观展示输入数据、反演模型和建模残差.

该工具的当前版本(3.0)是在早期的代码(版本1和2)基础上开发的，现在包含了一些新功能: 

 (1) 只有两个代码文件的更紧凑的包  
 (2) 密度对比度值可选择性计算
 (3) 兼容Windows 10 Pro
 (4) 能够反演向下密度增加的情况, 
 (5) 能够反演出来层状结构, 
 (6) 能够反演出场源横向平滑的情况,
 (7) 整个反演过程伴随着图像显示,
 (8) 生成的三维模型有更简单的文件结构,
 (9) 所有参数提供默认值，并保证运行的连续性.


主要输入文件GRA.DAT包含观测位置坐标信息，及重力异常值(详见 #2.2).
该方法 (详见 [1]  [2]) 用于确定具有明显(固定或可变，正或负)密度对比的多个异常体的几何结构.用大量规则单元组成的三维网格表示地下结构。通过反演方法用密度异常填充其中部分单元，从而再现地下密度异常结构。

该软件由两个Fortran 77代码组成:  GROWTH.FOR 和 VIEW.FOR. 

   GROWTH.FOR 主反演程序. 
   VIEW.FOR   提供图形显示.



2. "GROWTH.FOR": 三维重力反演.


2.1. 作用:  根据已有重力数据(GRA.DAT)通过计算得到异常体模型来实现三维重力反演. 该程序通过不断迭代使异常结构不断拟合重力数据观测值.
                
           
2.2. 输入.
                                                            
2.2.1. 输入文件:

-> 文件 "GRA.DAT": 最基本的输入文件"GRA.DAT" (默认情况下), 包含对应重力异常测定得到的: x坐标 (m), y坐标 (m), 海拔 (m) (整个软件包中，海拔值和深度值在海平面以上为正值，海平面以下为负值), 重力异常值 (uGal) (1 uGal=10-8 m/s2). 两个可选列: 每个观测点异常的重力误差估计值 (uGal), 单位密度 (1 kg/m3) 的地形校正值 (uGal). 误差估计值会在计算过程中采用，并选择性的用于数据的初始相对加权. 地形校正值会被选择性的用于重新估计地形密度值. 两个互补值都可以用任意数字填充，并且在这个过程中被忽略. 数据的最后一行需要用一行0值填充.

   例:

212876.1 3079402.0  560.30  9094 30  2917
212903.9 3079303.6  571.02  9041 30  3012
213149.2 3079854.5  533.59  5136 30  2824
213128.0 3080707.0  524.20  2059 30  2885
212667.0 3081849.6  403.01  1718 30  2370
..........................................
201634.6 3073315.1  283.73 44390 30  4110
0 0 0 0 0 0  

                 
2.2.2. 图形用户界面（GUI）的对话框窗体中的多个参数的值 (GUI): 

第一步 (左侧窗格): 

- 输入输出文件名 (提供默认值). 
  可以使用路径. 在每个步骤完成时，需要点击 “Run/OK”. 
  程序成功加载运行后会显示计算量.

第二步: 几何参数 (提供默认值).

- 模型顶部的深度(m). 还是低于海平面为负值.
- 模型底部的最大深度(m) Negative values below sea level as usual.
- 结构单元的平均长度(m). 浅层结构单元的长度较小，深层结构单元的长度较大.

第三步 (底部): 重复. 

 如果生成的单元格数m的量太大 (>95,000 此版本) 则点击复选框 “Re-run” 能够重新进行运算.
 单击 “Run”, 生成的三维单元体模型默认保存在文件“GRI.DAT”中.

第四步 (中间和右侧窗格): 用于反演模型的参数(提供了默认值或示例值).

a) Regional trend.
- Mean value g0 (µGal). This is an assumed regional constant value or mean offset value for the Bouguer anomaly data. If the “Adj” button is not pressed, the inversion is based on the given input value, which is assumed as fixed. By pressing the corresponding “Adj” button (default option), this parameter is considered as unknown to the program and an optimal value is adjusted as part of the inversion. The offset parameter has a particular bearing on the deeper parts of the model domain. We suggest performing an initial inversion by automatically adjusting a value, and then re-running inversions by varying this value in order to probe changes in the deep parts of the resultant 3D density models.
- gx, gy (µGal/km). These are slopes of the linear regional trend. The values may either be obtained from a regional survey and then introduced here as fixed values or conversely, they can be regarded as unknowns (pressing “Adj” button, default option) to be adjusted simultaneously in the inversion process.

b) Inversion parameters.
- Coefficient r for random search. This is an integer value (>=1) that allows for an exploratory process with a more or less random character. For a value of 1, the program carries out a systematic exploration. For a value r>1, the program tests 100/r % cells, randomly selected, thus allowing for a faster (but less precise) solution.
- Balance factor. This positive value corresponds to the parameter for balance between “fitness” and “smoothness” of the model. The value is dependent on the quality of the data and on the complexity of the subsurface structure. For low values, the inversion results in the generation of an excessively complex model of the subsurface structure with very good fit to the data. For high values, the inversion produces a simple model but with poor fit to the data. The operator may try different values of it close to the default value and draw conclusions by means of the third program (“VIEW”, see below) and evaluation of the auto-correlation distribution.

c) Terrain density fit. 
This is an option to estimate a revised value (kg/m3) to the terrain density used in the gravity anomaly calculations. It should be applied when the topographic relief is highly variable over the survey area (large and different terrain coefficients for unit density) to ensure a reliable solution. The corrective term is determined simultaneously during the gravity inversion according to the same minimization constraints. In this case a new parameter is required: the terrain filtering radius (meters). This is a distance value used to carry out an initial filtering of the terrain coefficients.

d) Parameters for density distribution.
- Prescribed positive and negative limits for density contrast (kg/m3). Optionally, pressing button “Adj”, the program determines a density contrast values (positive and negative) satisfying the minimization conditions. 
- Lateral smoothing "a": This positive value defines the pattern of density transition across the boundaries (geometrical smoothness) of the resultant anomalous bodies. For low values of a, the boundaries are abrupt and the bodies approximate homogeneous geometries (a=0) , such as cubes or spheres, with a single density value. For high values of a, the boundaries appear smoothed and the bodies are more diluted, with density values close to the extreme values towards the centre of the body and decreasing values towards their periphery.
- Flattening coefficient "f": It is connected with the downward continuous increase in density for the background medium. For a flattening coefficient f close to zero the inverse approach produces a 3D model with cup shape, corresponding to a homogeneous background medium. For a high value of this coefficient f, the inverse model becomes flattened, with small vertical size and bell shape, as corresponding to a background medium with high downward increase in density. For instance, for volcanic areas, with possible intrusive bodies vertically elongated, a f value close to 2 or 3 can be tried. But for sedimentary areas, dominated by sub-horizontal layers and increased compaction as a function of depth, a f value larger than 6 could be more suitable.
- Upward weighting u: It is an optional parameter to get shallower models, based on an unbalanced weighting in the fit. Top regions have higher weigth than deep regions. It is very useful for models characterized by a shallow main low density structure. This is the case of basin structures and other sedimentary structures. Value cero for u corresponds to neutral weighting. Higher value as 1,2, ... produce shallower models. 
- Sub-horizontal discontinuity layers: This check box accounts for the particular case of structures composed by several discrete layers with known average depth and fixed density. This case requires a precise a priori information of the medium structure to get suitable results. In the case of select this option, a new dialog window appears to input the a priori values for the layers.



2.3. RUNNING

Once all the required parameters are introduced, the program initiates the inversion. 
The evolution of the numbers of filled cells, number of outliers, density contrast values, standard deviation, regional values, etc. are shown on-screen. A graphic presentation (plan and elevation views) shows also the model growth.

The process ends when it is not possible to aggregate a new cell within the model constrains. Then , a message of either correct execution or error appears. Moreover, a box with some characteristic parameters of the resulting model and another box with a graphical presentation of the mass vs. depth distribution are displayed.

In particular, resulting values for (a) autocorrelation, (b) mass inversion, and (c) mass/depth slope can help to decide modification of the input modelling parameters. For instance, a high autocorrelation value of the resulting residual values suggests that the assumed balance factor is too low and there is a remaining signal. A re-running with a lower balance factor would be interesting. A high resulting value of mass inversion suggest that there is some portion of the model with downward density decrease. A re-runnig of the inversion approach with a higher value of flattening would be interesting. Surely, the last conclusion could be also obtained by inspection of the mass vs. depth distribution picture.



2.4. OUTPUT                                                              
                                                               
-> file "MOD.DAT": inversion model as 3-D partition of the subsurface volume into cells of anomalous density. The content and general format of this ASCII file is: 
 
    nc, side, step
    x(1),y(1),z(1),tx(1),ty(1),tz(1),d(1)   
    x(2),y(2),z(2),tx(2),ty(2),tz(2),d(2)          
    ..........................................
    x(nc),y(nc),z(nc),tx(nc),ty(nc),tz(nc),d(nc),ds(nc),at(j),sc(j)

    where:                                                         

    nc:  total number of cells (parallelepiped) of the model                                  
    side: adopted mean side (in metres) for the cells in the 3D partition                                    
    step: distance (in metres) for step in the further correlation analysis. 
                                         
    x(j),y(j),z(j): coordinates (in metres) UTM and depth for the center of the j-th cell of the model.
    tx(j),ty(j),tz(j): horizontal sides along x,y,and z of the j-th cell of the model.
    d(j): anomalous density contrast (kg/m3) of the j-th cell

    ds(j): anomalous density contrast (kg/m3) of the j-th cell without including the medium effects.
    at(j): mean quadratic atraction ugal for density=10 Tm/m3 in the j-th cell
    sc(j): scale factor when filling of j-th cell
    j=1,...,nc

At the end of this file MOD.DAT, several characteristic parameters of the inversion model are displayed: mean density contrast of the anomalous model, mean density contrast of the negative anomalies, mean density contrast of the positive anomalies, extreme density contrasts, depth of the full anomalous, negative and positive structures, etc.

      example:
 
  97629    280   1497
 199817 3070514    1161    224    144    224      0    798     66     0.0
 199817 3070646    1161    224    120    224      0    798     64     0.0
 199817 3070759    1161    224    106    224      0    798     64     0.0
 199817 3070862    1161    224    100    224      0    798     64     0.0
 . . . . . . . . . . . .  . . . . . . . . . . . . . . . . . . . . . . . . .
 225025 3082597   -9968   1203    557   1203      0   5586    -61     0.0
 225025 3083167   -9968   1203    582   1203      0   5586    -62     0.0
-------------------------(-)-------(+)-----(anom)-----(min)-----(max)-----
Mean Dens.(kg/m3) :     -400.     502.       500.     -20.       20.
Depth of cells(m) :    -1247.    -4727.    -1935.      540.    -7472.
Weighted depths   :    -1476.    -5159.    -5113.
Number of cells   :       610      9735     10345
Anomal. mass (kg) :  0.734E+13 0.340E+15 0.347E+15 (rel. 0.000E+00)
Side of cells (m) :
    Min=  181      Max=  241      Average=  413      Ref=  280.
Num.of stations= 179    Scale fact. =    0.990
    Outliers(>3.5)=  0 ( 0.0%)
Gravity anomaly dispersion (uGal)
    Obs.= 13581.   Reg.=  7032.   Loc.= 25532.   Res.=  1827.
Modelling parameters
    Lambda fac=   80.0      Up weighting= 0.0
    Scale fact: in=   2543. fin= 20.00
    A priori data weighting Y(1)/N(0):0
    Random exploration coef.: 10   (1 sistem.)
Trend =    6946.(1) uGal + ( -1085.(0)   -92.(0) ) uGal/km
    Slope= 1089. uGal/km    Azim.= N  85. E   (1:adj,0:fix)
    Incremental coord. from (xm,ym,zm)=   203920  3072960   540 m
Additional terrain density=    0. (0) kg/m3
    Terrain filtering radius=  5986 m
Residues (uGal):
    Mean=   6    Min.= -4844   Max.= 4125
    Weight. StDv= 1827.3    Consist. Stdv= 1791.3
    Fit.=  48737. uGal
    Mean Sensitivity for cells=   0.3 uGal for 100 kg/m3
    Autocorrelation =  0.24     Correlat. step=  1497 m
    StDv Data = 25532. uGal     SD Resid/SD Data = 0.07
Dens. pattern:
    Lateral smoothing (1-10)= 0.00
    Flattening= 0.0      Downward Incr.:   0 kg/m3 each km
    Sub-horiz.discontin. (Yes(1)/No(0)): 1
    Density contrast fit (Yes(1)/No(0)): 0
    A priori max.densit.contrast=  -20.   20. kg/m3

         Layers model:   Shape=2.00  Bottom=  -7380.
         Weight.model:  q1n=520 q2n=-12  q1p=  0 q2p= 40
         Num. of layers=14
         Depths(m)   1270   1270   1040    600    370    -70   -520  -1050  -2010
         Density      399    399    399    399    399    399    399    399    399
         Flattening    11.0   11.0   11.0   11.0   11.0   11.0   11.0   11.0   11.0
         Neg.weig:    NaN    NaN    NaN    NaN   1.05   0.82   0.86   0.89   0.98
         Pos.weig:    NaN    NaN    NaN    NaN    NaN   0.91   0.80   0.73   0.62
         Num.cells      0      0      0      0      1    120    294    568    955
         Aver.weights:   Neg= 0.88  Pos= 0.39  Tot= 0.41
-------------- Date:04-Mar-20-----------------




-> file "FIL.DAT": ASCII file containing, for each data point (in planar coordinates), the following values (uGal) resulting from the inversion process:
              regional gravity anomaly
              observed local anomaly (= observed - regional anomaly)
              modelled local anomaly
              residual values
              estimated weights (relative to median=1.0)

     example:

 212876. 3079402.   12692.   -3598.   -3821.     223.     0.74        
 212904. 3079304.   12669.   -3628.   -3444.    -184.     0.61        
 213149. 3079854.   12466.   -7330.   -7126.    -205.     0.68        
 213128. 3080707.   12484.  -10425.  -10283.    -143.     0.47        
 212667. 3081850.   12865.  -11147.  -11392.     244.     0.81        
 211991. 3082700.   13424.  -15454.  -14296.   -1158.     3.84  *     
 212073. 3083784.   13356.  -13964.  -14031.      67.     0.22        
    . . . . . . . . . . . 


2.4. DIMENSIONS                                                         

    The memory requirements of the program execution depend on the following parameters:
                                                         
    ms:  max. number of gravity stations. 
    mc:  max. number of whole prismatic cells for the 3-D grid.   

The values of these size parameters can be changed in the code according to the applications and the memory abailability.



2.5. INCLUDED SUBROUTINES
                                    
  VAP: Vertical attraction (in microgal, positive upwards) of a parallelepiped cell of: 
              center xc,yc (metres, from the point), 
              horizontal sides dx,dy (metres), 
              depth of the bottom and top faces zb,zt 
                     (metres, positive upwards), and
              density d (kg/m3) 
      upon the coordinate origin. 
      The main calculus is carried out in the subroutine PIC.
  STEP: Obtaining a default value for correlation step corresponding to the geometrical distribution of benchmarks.
  DMEDIAN: Obtaining the median of n different numbers.
  DIM: Output massage dimension limits are exceeded.
  COV2: Autocorrelation of two-dimensional data for a given correlation step.
  DIAL1, DIAL2, DIAL3, DIAL4, ABIL1, ABIL2, ABIL3, ABIL4, ABIL5, ABIL6, ABIL7, ABIL8, FIN: Dialog interfaces.



2.6. COMPILING SPECIFICATIONS. 

Compiled with Intel Visual Fortran, by using a Quickwin Application Project. The complete compilation requires reveral additional files (that are available in the zip file of codes and):
    
   Resource files:  Growth.rc    (resource script)
                    Growth.ico   (icono)
   
   Fortran source files:  Growth.fd
                          Growth.for 

   C/C++Header:  Growth.h   (dialog window)    




3. PROGRAM "VIEW.FOR": Graphic presentation of the inversion model.


3.1. AIM:  to give a visual presentation of results coming from GROWTH gravity inversion. The program can offer several coloured pictures of the 3D inversion model by means of vertical profiles and horizontal cross sections. Maps of the gravity anomaly, altitudes, residuals, calculated anomaly, regional trend, etc are also availables. Optionally, output files can be obtained to export the figures to another more sophisticated graphical device.
                
           
3.2. INPUT.
                                                            
-> file "GRA.DAT" (default name): ASCII file of coordinates, altitudes, observed gravity values and relative errors for the gravity stations. See paragraph #3.2 about content and format.

-> file "MOD.DAT" (default name): resulting 3-D model for the anomalous density contrasts, obtained as output of GROWTH.FOR. See paragragh #3.4 about content and format.

-> file "FIL.DAT" (default name): resulting regional, local, calculated and residual anomaly values from the inversion process, obtained as output from GROWTH.FOR. See paragraph #3.4 about content and format.  
       
-> file "MAP.BLN" (default name): ASCII file containing a base map (coast, roads, etc.) to be included on the graphic presentation. The format of this file is:
 
       j1
     xx(1),yy(1)
     xx(2),yy(2)
     ..........
     xx(j1),yy(j1)
        j2
     xx(1),yy(1) 
     xx(2),yy(2)
     ...........
     xx(j2),yy(j2) 
       ...

 where j1,j2,... are the number of points for each line and xx,yy are plane coordinates for the sucessive points of the lines.
       
  example

     1528   * El Hierro
  211600 3084000   0.4
  211670 3084000   0.0
  211720 3084000   0.1
  211765 3083930   0.6
   . . . . . . . . . .


3.3. OUTPUT

3.3.1. GRAPHICS
Several kinds of pictures corresponding to program options can be obtained on the screen.

a. Plane maps (including the lines of MAP.BLN) for:
         station altitudes
         observed gravity anomaly
         relative errors of the observed anomaly
         regional trend values
         local anomaly (= observed anomaly - regional anomaly) 
         calculated values for the local anomaly
         inversion residuals

b. Combined view of one horizontal section (across a desired depth) and two vertical profiles (with nearly W-E and S-N directions) of the 3-D model for:         
         density contrasts
         sensitivity values   


3.4. DIMENSIONS                                                         

    The memory requirements of the program execution depend on the following parameters:
                                                         
    ms:  max. number of gravity stations. 
    mc:  max. number of prismatic cells for the 3-D grid.   
    mb:  max. number of points for the base map file.    
 
The values of these size parameters can be changed in the code according to the applications and the memory abailability.



3.5. INCLUDED SUBROUTINES
                                    
ACOL: determinig a colour k corresponding to a variable dn.
CMAP: displaying a coloured map of a plane data distribution x,y,g.
CONT: displaying a base map of polygonal lines.
PERTOP: determining a topographical line across a vertical profile
   of the 3-D model
AM: interpolating and smoothing from a 3-D grid of cells.
SST: mean sensitivity of the network for a variable point mass 
STRA: determining horizontal layers.
COV2 and BUSCA: Autocorrelation of two-dimensional data for a given correlation step.
TITULO: drawing a title.
DIM: Output massage dimension limits are exceeded.
EJES: Drawing axis and ticks.
DIAL1, DIAL2, DIAL3, DIAL4, FIN: Dialog interfaces.

function IRVA : combining basic colours (red, green, blue)
 


3.6. COMPILING SPECIFICATIONS. 

Compiled with Intel Visual Fortran, by using a Quickwin Application Project. The complete compilation requires reveral additional files (that are available in the zip file of codes and):
    
   Resource files:  View.rc    (resource script)
                    View.ico   (icono)
   
   Fortran source files:  View.fd
                          View.for 

   C/C++Header:  View.h   (dialog window)    




REFERENCES: 

[1] Camacho, A.G., Montesinos, F.G. and Vieira, R. (2001). A 3-D gravity inversion tool based on exploration of model possibilities. 
Computer & Geosciences 28, 191–204

[2] Camacho, A.G., Fernandez, J. and Gottsmann, J. (2011). The 3-D gravity inversion package GROWTH2.0 and its application to Tenerife
Island, Spain. Computer & Geosciences 37, 621-633.

[3] Camacho, A.G. and Fernandez, J. (2019).Modeling 3D free-geometry volumetric sources associated to geological and anthropogenic
hazards from space and terrestrial geodetic data. Remote Sens., 11(17), 2042; doi: 10.3390/rs11172042.
     

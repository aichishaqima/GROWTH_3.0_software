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
- 模型底部的最大深度(m) 低于海平面为负值.
- 结构单元的平均长度(m). 浅层结构单元的长度较小，深层结构单元的长度较大.

第三步 (底部): 重复. 

 如果生成的单元格数m的量太大 (>95,000 此版本) 则点击复选框 “Re-run” 能够重新进行运算.
 单击 “Run”, 生成的三维单元体模型默认保存在文件“GRI.DAT”中.

第四步 (中间和右侧窗格): 用于反演模型的参数(提供了默认值或示例值).

a) 区域趋势.
- 平均值 g0 (µGal). 可以视为布格异常总场值或平均偏移量值. 如果“Adj”按钮未按下, 反演基于给定的输入值, 该值被认为是一个固定值. 如果“Adj”按钮被按下(默认选项), 则该参数被认为是未知的, 在反演过程中不断完善该值. 偏移参数对模型的深部位置有一定的影响. 建议进行初始反演时选择自动调整的方式，然后通过手动改正该值来重新运行反演程序, 以便得到三维密度模型深层部分的最佳反演.
- gx, gy (µGal/km). 这两个量是趋势面的导数. 这些值可以从区域场中计算得到，然后作为已知量引入, 类似，他们也可以被当作未知值(按“Adj”按钮，默认选项)，在反演的过程中不断调整.

b) 反演参数.
- 随机搜索系数r. 整数值 (>=1) 通过输入该参数来进行随即搜索. 如果值为1, 则进行系统默认的搜索. 如果该值大于1, 能够实现更快但不太准确的计算方案.
- 平衡因子. 该值为正值, 对应于模型的 “fitness” 和 “smoothness” 之间的平衡参数. 该值取决于数据的质量和地下结构的复杂性. 如果该值为低值, 反演结果生成的是一个非常复杂的地下结构模型. 如果该值为高值, 繁衍的到一个相对简单的模型，而且与原始数据的拟合差较大. 操作工程中可以尝试默认值附近的不同值, 并通过第三个程序 (“VIEW”, 见下文) 和自相关分布的计算得出结论.

c) 地形密度匹配. 
该选项是用于估算重力异常计算中使用的地形密度改正值(kg/m3). 当测区地形起伏变化较大 (单位密度的地形系数较大且不同) 时, 应该采用该方法，以得到更加可靠的解. 在重力反演过程中，根据相同的最小化约束条件，来确定校正项. 在这种情况下, 需要一个新参数: 地形滤波半径 (meters). 这是用于对地形系数执行初始滤波的距离值.

d) 密度分布参数.
- 密度差异最大值 (kg/m3). 选填项, 如果“Adj”按钮被按下, 程序自动确定满足最小化条件的密度对比值(正和负). 
- 横向平滑系数 "a": 这正值定义了穿过异常体模型边界的密度过渡模式(几何平滑度). 对于较低的a值, 边界是突变的, 和近似均匀的几何体 (a=0) , 例如具有单个密度值的立方体或球体. 对于较高的a值, 边界看起来更光滑, 下降梯度更小, 密度值接近物体中心处取得极值, 并向其外围递减.
- 扁平率 "f": 当扁平率f接近零时, 反演得到一个杯型的三维模型, 对应均匀分布的背景介质. 当该系数f值较大时, 反演模型起伏较小, 垂直变化较小, 呈钟形, 与密度向下增加较大的背景介质相对应. 例如, 对于可能为垂直拉长侵入体的火山区, 可以尝试接近2或3的f值. 但对于以近水平层为主、压实度随深度增加的沉积区, f值大于6可能更为合适.
- 向上加权系数u: 基于拟合中的不平衡加权, 是计算浅层模型的可选参数. 顶部区域的重量比深部区域大. 这对于以浅层低密度构造为主的模型非常有用. 例如盆地构造和其他沉积构造. 如果u的值为零, 对应正常模型. 当取1, 2...或更高的值时, 对应较浅的模型. 
- 次水平不连续层: 此复选框用于说明由几个具有平均深度和固定密度的离散层组成的特定结构. 这种情况需要精确的介质结构先验信息才能得到合适的结果. 在勾选此选项的情况下, 将显示一个新的对话框窗口来输入先验值.



2.3. 运行

已经输入所有必需的参数, 就可以运行反演程序. 
单元格数、异常值数、密度对比度、标准差、区域值等的变化会显示在屏幕上.  图像 (平面视图和立体视图) 随着反演计算显示.

当无法在模型约束内聚合新单元时，程序结束运行. 然后，将会出现一条提示程序是否成功运行的信息. 此外，还会显示一个窗口，其中包括所生成模型的一些特征参数，及质量和深度分布的图形表示.

特别是，结果值(a)自相关, (b)质量反演, 和(c)质量/深度斜率的结果值有助于确定输入建模参数的修改。例如, 所得残值的高自相关值表明假设的平衡因子太低并且存在剩余信号. 以较低的平衡系数重新运行会使结果更接近预期. 当然, 最后的结论也可以通过检查质量和深度分布图得到.



2.4. 输出                                                              
                                                               
-> "MOD.DAT"文件: 将地下体积划分为多个单元体的密度异常三维反演. 这个ASCII文件的内容和一般格式为: 
 
    nc, side, step
    x(1),y(1),z(1),tx(1),ty(1),tz(1),d(1)   
    x(2),y(2),z(2),tx(2),ty(2),tz(2),d(2)          
    ..........................................
    x(nc),y(nc),z(nc),tx(nc),ty(nc),tz(nc),d(nc),ds(nc),at(j),sc(j)

    where:                                                         

    nc:  模型的单元体个数                                
    side: 单元体长宽高平均长度(m)                                 
    step: 进行相关性分析的单元体之间的距离(m) 
                                         
    x(j),y(j),z(j): 模型第j个单元中心的坐标(m), UTM及深度.
    tx(j),ty(j),tz(j): 模型第j个单元的密度密度异常.
    d(j): 模型第j个单元的密度异常(kg/m3)

    ds(j): 模型第j个单元体的密度异常,不包括介质效应(kg/m3).
    at(j): 模型第j个单元体ugal平均值平方除密度=10 Tm/m3 
    sc(j): 填充第j个单元体时的比例因子
    j=1,...,nc

在文件MOD.DAT的末尾给出了反演模型的几个特征参数: 模型的平均剩余密度, 模型负异常的平均剩余密度, 模型正异常的平均剩余密度, 平均剩余密度的极值差, 正负异常构造等等.

      例如:
 
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




-> 文件"FIL.DAT": ASCII文件, 对于每个数据点(在平面坐标中), 包含由反演过程产生的以下值(uGal):
              区域重力异常
              观测局部异常 (= 观测值 - 区域异常)
              模型局部异常
              剩余值(观测与模型差值)
              估计权重 (相对于中位数=1.0)

     例如:

 212876. 3079402.   12692.   -3598.   -3821.     223.     0.74        
 212904. 3079304.   12669.   -3628.   -3444.    -184.     0.61        
 213149. 3079854.   12466.   -7330.   -7126.    -205.     0.68        
 213128. 3080707.   12484.  -10425.  -10283.    -143.     0.47        
 212667. 3081850.   12865.  -11147.  -11392.     244.     0.81        
 211991. 3082700.   13424.  -15454.  -14296.   -1158.     3.84  *     
 212073. 3083784.   13356.  -13964.  -14031.      67.     0.22        
    . . . . . . . . . . . 


2.4. 要求                                                         

    程序执行的内存需求取决于以下参数:
                                                         
    ms:  最大重力观测点数. 
    mc:  三维模型最大单元体数.   

这些参数值的大小可以根据应用程序和可用内存用代码进行修改.



2.5. 包含的子程序
                                    
  VAP: 平行六面体单元体的垂直吸引 (以微伽为单位, 向上为正): 
              中心坐标 xc,yc (metres, from the point), 
              边长 dx,dy (metres), 
              底面和顶面深度 zb,zt 
                     (metres, 向上为正), and
              密度 d (kg/m3) 
      坐标原点向上. 
      主要计算在子程序PIC中进行.
  STEP: 获取与基准点几何分布相对应的相关步长的默认值.
  DMEDIAN: 求取n个不同数的中值.
  DIM: 提示维度超出限制.
  COV2: 二维数据的自相关.
  DIAL1, DIAL2, DIAL3, DIAL4, ABIL1, ABIL2, ABIL3, ABIL4, ABIL5, ABIL6, ABIL7, ABIL8, FIN: Dialog interfaces.



2.6. 编译规范. 

通过使用Quickwin程序完成Inter Visual Fortran程序的编译. 完整的编译需要额外的文件 (可以在压缩文件中的代码文件中找到):
    
   源文件:  Growth.rc    (resource script)
                    Growth.ico   (icono)
   
   Fortran源文件:  Growth.fd
                          Growth.for 

   C/C++Header:  Growth.h   (对话框窗口)    




3. 程序"VIEW.FOR": 反演模型图示.


3.1. 作用:  对重力反演的结果进行直观的描述. 该程序可以通过垂直剖面和水平剖面提供三维反演模型的多种彩色图像. 还提供了重力异常图、高程图、残差图、计算异常图、区域趋势图等. 可选的, 可以获得输出文件以将图形导出到另一个更复杂的图形设备.
                
           
3.2. 输入.
                                                            
-> 文件"GRA.DAT" (自定义名称): 重力观测点的坐标、高度、观测重力值和相对误差的ASCII文件. 关于内容和格式, 见3.2.

-> 文件"MOD.DAT" (自定义名称): 反演得到的密度异常三维模型, 程序GROWTH.FOR的输出文件. 见3.4.

-> 文件"FIL.DAT" (自定义名称): 反演过程产生的区域、局部、计算和剩余异常值, 程序GROWTH.FOR输出. 见3.4.  
       
-> 文件"MAP.BLN" (自定义名称): ASCII文件包含在图形表示中的基础组件 (海岸, 道路, 等.). 此文件的格式为:
 
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

 其中 j1,j2,... 是每条线的点数, xx, yy是线延续点的平面坐标.
       
  例如

     1528   * El Hierro
  211600 3084000   0.4
  211670 3084000   0.0
  211720 3084000   0.1
  211765 3083930   0.6
   . . . . . . . . . .


3.3. 输出

3.3.1. 图像文件
在屏幕上可以获得与程序选项相对应的多种图片.

a. 平面图 (包括MAP.BLN文件绘制的线):
         观测点的高度
         观测重力异常
         观测异常相对误差
         区域趋势值
         局部异常 (= 观测异常 - 区域异常) 
         局部异常的计算值
         反演残差

b. 三维模型的一个水平剖面(穿过所需深度), 以及两个垂直剖面(具有近W-E和S-N方向)的组合视图:         
         密度差异
         灵敏度值  


3.4. 要求                                                         

    程序执行的内存需求取决于以下参数:
                                                         
    ms:  最大观测点数. 
    mc:  三维网格最大单元体个数.   
    mb:  底图文件的最大点数.    
 
这些参数值的大小可以根据应用程序和内存可用性在代码中更改.



3.5. 包含的子程序
                                    
ACOL: 确定与变量dn相对应的颜色k.
CMAP: 显示平面数据分布 x,y,g 的彩色丢.
CONT: 显示多边形线的底图.
PERTOP: 确定穿过三维模型的垂直剖面的地形线
AM: 三维网格的插值与平滑.
SST: 不同质量点网络的平均灵敏度
STRA: 确定水平层.
COV2 and BUSCA: 给定相关步长的二维数据的自相关.
TITULO: 写标题.
DIM: 超出输出尺寸限制.
EJES: 作图显示坐标轴和标注.
DIAL1, DIAL2, DIAL3, DIAL4, FIN: Dialog interfaces.

function IRVA : 组合三原色 (红, 绿, 蓝)
 


3.6. 编译规范. 

通过使用Quickwin程序完成Inter Visual Fortran程序的编译. 完整的编译需要额外的文件 (可以在压缩文件中的代码文件中找到):
    
   源文件:  View.rc    (源脚本)
                    View.ico   (图标)
   
   Fortran 源文件:  View.fd
                          View.for 

   C/C++Header:  View.h   (对话框窗口)    




参考文献: 

[1] Camacho, A.G., Montesinos, F.G. and Vieira, R. (2001). A 3-D gravity inversion tool based on exploration of model possibilities. 
Computer & Geosciences 28, 191–204

[2] Camacho, A.G., Fernandez, J. and Gottsmann, J. (2011). The 3-D gravity inversion package GROWTH2.0 and its application to Tenerife
Island, Spain. Computer & Geosciences 37, 621-633.

[3] Camacho, A.G. and Fernandez, J. (2019).Modeling 3D free-geometry volumetric sources associated to geological and anthropogenic
hazards from space and terrestrial geodetic data. Remote Sens., 11(17), 2042; doi: 10.3390/rs11172042.
     

                          

                           G R O W T H 3.0

重力异常三维反演地下密度结构的软件


          作者  A.G. Camacho (1), J. Fern�ndez (1), 
          (1) Institute of Geosciences (CSIC-UCM), Madrid, Spain
          翻译  wu



GROWTH is a software for determination of subsurface anomalous density structures from discrete gravity anomaly values. It works in a context of free 3D geometry of the causative bodies. From only very general assumptions, the code determines, in a nearly automatic approach, general anomalous structures described as aggregation of thousands of small rectangular prisms.
GROWTH是一个离散重力异常值确定地下异常密度结构的软件。（未完成）


This package includes:


    (a) Fortran 源代码:   GROWTH.for  VIEW.for
        
         用户手册:   UserManual-GROWTH.txt


    (b) 可执行文件 (如果文件名为  *-exe 形式, 请重新命名为 *.exe 再运行): 

                  GROWTH.exe
 
                  VIEW.exe 


    (c) Files for an application example, corresponding to a gravity survey in the volcanic Island of El Hierro (Canary Islands).


                  Data files:   GRA.DAT , and MAP.BLN  (the second one,only for pictures)
 
                  Output files:    MOD.DAT, FIL.DAT


   (d) Additional files (*.vfproj, *.h, *.rc, *.ico, etc) for compilation.
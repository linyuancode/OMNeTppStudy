# OMNeT++向量设备位置布局
## @display特性
```NED
@display=("p=p[0],p[1],p[2],p[3],p[4,p[5]]")
 ```
 参数 |值
:----|:----
p[0] - x							|X position of the center of the icon/shape; defaults to automatic graph layouting
p[1] - y							|Y position of the center of the icon/shape; defaults to automatic graph layouting
p[2] - arrangement		|Arrangement of submodule vectors. Values: row （r）, column （c）, matrix ( m ), ring (ri), exact (x)
p[3] - arr. par1				|Depends on arrangement: matrix => ncols, ring => rx, exact => dx, row => dx, column => dy
p[4] - arr. par2				|Depends on arrangement: matrix => dx, ring => ry, exact => dy
p[5] - arr. par3				|Depends on arrangement: matrix => dy

通过设置6个参数可以实现设备横、纵、矩阵、环、exact（这个不懂）布局。
# INET移动设备
INET移动设备[设备数量]若定义@display仿真会报错，需要删掉第三个参数。
## mobility参数
该类设备以INET提供的WirelessHost为代表，定义其静态位置需要通过mobility参数。
```NED
*.host[.].mobility.typename = "模型名"
```
INET提供三种不运动的静止模型。
模型名|解释
:--|:--
StationaryMobility|提供确定性（设置初始位置）和随机定位（不设置初始位置）
StaticGridMobility|将多个移动模型放置在矩形网格中（类似@display特性中matrix布局）
StaticConcentricMobility|将几个模型放在一组同心圆中

### StationaryMobility
该模型通过设置各设备初始位置确定设备向量中各设备位置。
```
haic.gateway[*].mobility.typename="StationaryMobility"
haic.gateway[*].mobility.initFromDisplayString=false		#默认为true，需手动设置为false
haic.gateway[0].mobility.initialX=200m
haic.gateway[0].mobility.initialY=300m
haic.gateway[0].mobility.initialZ=0m
haic.gateway[1].mobility.initialX=600m
haic.gateway[1].mobility.initialY=300m
haic.gateway[1].mobility.initialZ=0m
```
INET用户手册中提供了完整的参数列表
变量名|数据类型|默认值|描述
----|----|----|----
subjectModule	|string|	"^"|module path which determines the subject module, the motion of which this mobility model describes, the default value is the parent module
coordinateSystemModule|	string|	""	|module path of the geographic coordinate system module
displayStringTextFormat|	string|"p: %p\nv: %v"|format string for the mobility module's display string text
updateDisplayString	|bool|	true	|enables continuous update of the subject module's position via modifying its display string
constraintAreaMinX	|double	|-inf m	|min x position of the constraint area, unconstrained by default (negative infinity)
constraintAreaMinY|	double|	-inf m	|min y position of the constraint area, unconstrained by default (negative infinity)
constraintAreaMinZ|	double	|-inf m	|min z position of the constraint area, unconstrained by default (negative infinity)
constraintAreaMaxX	|double	|inf m	|max x position of the constraint area, unconstrained by default (positive infinity)
constraintAreaMaxY	|double	|inf m	|max y position of the constraint area, unconstrained by default (positive infinity)
constraintAreaMaxZ	|double	|inf m	|max z position of the constraint area, unconstrained by default (positive infinity)
initialX	|double	|uniform(this.constraintAreaMinX, this.constraintAreaMaxX)	
initialY	|double	|uniform(this.constraintAreaMinY, this.constraintAreaMaxY)	
initialZ	|double	|nanToZero(uniform(this.constraintAreaMinZ, this.constraintAreaMaxZ))	
initialLatitude	|double	|nan deg	
initialLongitude	|double	|nan deg	
initialAltitude	|double	|0m	
initialHeading	|double	|0deg	
initialElevation	|double	|0deg	
initialBank	|double	|0deg	
initFromDisplayString	|bool|	true	|enables one time initialization from the subject module's display string
updateFromDisplayString	|bool|	true	|enables continuous update from the subject module's display string for dragging and rotating it
### StaticGridMobility
该模型通过设置设备数、设备行列数设置布局。
```NED
#设备以网格布局分布在以下区域
haic.host[*].mobility.constraintAreaMaxX = 800m
haic.host[*].mobility.constraintAreaMinX = 0m
haic.host[*].mobility.constraintAreaMaxY = 800m
haic.host[*].mobility.constraintAreaMinY = 400m
haic.host[*].mobility.constraintAreaMaxZ = 0m
haic.host[*].mobility.constraintAreaMinZ = 0m

haic.host[*].mobility.typename="StaticGridMobility"
haic.host[*].mobility.columns = 6
haic.host[*].mobility.numHosts = 12							#必须设置设备数量
```

INET用户手册中的全部参数
变量名|数据类型|默认值|描述
----|----|----|----
subjectModule	|string	|"^"	|module path which determines the subject module, the motion of which this mobility model describes, the default value is the parent module
coordinateSystemModule	|string	|""	|module path of the geographic coordinate system module
displayStringTextFormat	|string|	"p: %p\nv: %v"	|format string for the mobility module's display string text
updateDisplayString|	bool|	true	|enables continuous update of the subject module's position via modifying its display string
constraintAreaMinX|	double|	-inf m	|min x position of the constraint area, unconstrained by default (negative infinity)
constraintAreaMinY|	double	|-inf m	|min y position of the constraint area, unconstrained by default (negative infinity)
constraintAreaMinZ	|double	|-inf m	|min z position of the constraint area, unconstrained by default (negative infinity)
constraintAreaMaxX|	double|	inf m	|max x position of the constraint area, unconstrained by default (positive infinity)
constraintAreaMaxY	|double|	inf m	|max y position of the constraint area, unconstrained by default (positive infinity)
constraintAreaMaxZ	|double|	inf m	max z position of the constraint area, unconstrained by default (positive infinity)
marginX|	double|	0m	
marginY	|double	|0m	
numHosts	|int		|
columns|	int|	int(ceil(sqrt(numHosts * (this.constraintAreaMaxX - this.constraintAreaMinX - 2 * marginX) / (this.constraintAreaMaxY - this.constraintAreaMinY - 2 * marginY))))	
rows	|int	|int((numHosts + columns - 1) / columns)	
initialZ	|double	|0m	
separationX|	double|	(this.constraintAreaMaxX - this.constraintAreaMinX - 2 * marginX) / columns	
separationY	|double	|(this.constraintAreaMaxY - this.constraintAreaMinY - 2 * marginY) / rows
### StaticConcentricMobility
INET用户手册全部参数
 变量名|数据类型|默认值|描述
----|----|----|----
subjectModule|	string|	"^"	|module path which determines the subject module, the motion of which this mobility model describes, the default value is the parent module
coordinateSystemModule|	string	|""	|module path of the geographic coordinate system module
displayStringTextFormat|	string|	"p: %p\nv: %v"	|format string for the mobility module's display string text
updateDisplayString|	bool	|true	|enables continuous update of the subject module's position via modifying its display string
constraintAreaMinX	|double	|-inf m	|min x position of the constraint area, unconstrained by default (negative infinity)
constraintAreaMinY|	double|	-inf m	|min y position of the constraint area, unconstrained by default (negative infinity)
constraintAreaMinZ|	double	|-inf m	|min z position of the constraint area, unconstrained by default (negative infinity)
constraintAreaMaxX|	double	|inf m	|max x position of the constraint area, unconstrained by default (positive infinity)
constraintAreaMaxY	|double	|inf m	|max y position of the constraint area, unconstrained by default (positive infinity)
constraintAreaMaxZ	|double	|inf m	|max z position of the constraint area, unconstrained by default (positive infinity)
numHosts|	int		
initialZ	|double|	0m	
distance|	double	|100m	
# INET环境配置相关
## 在INET网络仿真中配置障碍物
ned文件中需要定义有visualizer和physicalEnvironment
```
import inet.environment.common.PhysicalEnvironment;
import inet.visualizer.contract.IIntegratedVisualizer;
    submodules:
        visualizer: <default(firstAvailableOrEmpty("IntegratedCanvasVisualizer"))> like IIntegratedVisualizer if typename != "" {
            @display("p=757,301");
        }
        physicalEnvironment: PhysicalEnvironment {
            @display("p=760,217");
        }
```
visualizer用于显示physicalEnvironment的障碍物，若不定义会导致仿真正常运行但是不显示障碍物。  
omnetpp.ini配置文件中应当写入  
```
*.physicalEnvironment.config = xmldoc("envconf.xml")
```
envconf.xml配置文件在INT官方文档示例为
```
<environment>
  <!-- shapes and materials -->
  <shape id="1" type="sphere" radius="10"/>
  <shape id="2" type="cuboid" size="20 30 40"/>
  <shape id="3" type="prism" height="10" points="0 0 10 0 10 10 0 10"/>
  <shape id="4" type="polyhedron" points="0 0 0 10 0 0 10 10 0 0 10 0 ..."/>
  <material id="1" resistivity="10" relativePermittivity="4.5"/>
  <!-- an object that uses a previously defined shape and material -->
  <object position="min 10 20 0" orientation="45 0 0" shape="1" material="1"/>
  <!-- an object defined with an in-line shape -->
  <object position="min 10 20 0" orientation="45 -30 0" shape="cuboid 20 30 40"
             material="concrete" line-color="0 0 0" fill-color="112 128 144"/>
</environment>
```
## INET官方代码相关
INT官方示例代码存在较大问题，可能是由于OMNeT++或INET版本相关兼容问题。  
+ 自定义material代码
```
<material id="1" resistivity="10" relativePermittivity="4.5"/>
```
该行代码会报错缺少relativePermeability参数，该参数为相对渗透率。  
+ 对象使用自定义material与shape

```
<object position="min 10 20 0" orientation="45 0 0" shape="1" material="1"/>
```
该代码不会报错，但在仿真中无法显示，经查，将shape由使用自定义shape改为直接定义可以解决，该问题与material无关。  
+ 可运行代码示例
```
<environment>
    <shape id="1" type="cuboid" size="5 550 4"/>
    <material id="1" name="iron" resistivity="10" relativePermittivity="4.5" relativePermeability="0.5"/>
    <object position="min 397.5 250 0" orientation="0 0 0" shape="cuboid 5 550 4" material="1" fill-color="203 65 84" opacity="0.8"/>
</environment>
```
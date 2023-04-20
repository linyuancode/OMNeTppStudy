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
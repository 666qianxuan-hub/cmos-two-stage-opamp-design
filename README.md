# CMOS 两级运算放大器课程设计

CMOS Two-Stage Operational Amplifier Course Design

本仓库为《模拟集成电路设计（二）》课程设计项目，基于 **LTspice** 完成 CMOS 两级运算放大器（Two-Stage Op-Amp）的电路设计、仿真与验证，并附有课程设计报告及相关文档。

## 目录结构

```
public/
├── Draft3.asc                    # 主仿真电路图（LTspice 原理图，PMOS 输入两级运放，3.3V 电源）
├── Draft3.log                    # 主电路仿真日志（LTspice 运行输出）
├── 工艺文件/                     # 仿真用工艺库（PDK）
│   ├── LTS_GFMCU/LTS_GFMCU/      # GlobalFoundries GFMCU 工艺库（LTspice 专用，3.3V 器件，BSIM4 模型）
│   └── tsmc018/                  # TSMC 0.18μm 工艺库（BSIM3 / Level 49 模型）
├── 模拟集成电路设计二课设.docx   # 课程设计报告（最终版）
├── 课设说明.docx                 # 课程设计任务说明
├── EECS-2016-223.pdf             # 参考资料（英文文献，运放设计相关）
└── README.md                     # 本说明文件
```

## 文件说明

### 主电路与仿真（根目录）

| 文件 | 说明 |
| --- | --- |
| `Draft3.asc` | 主运放仿真原理图：两级结构（PMOS 差分输入对 + 共源输出级），3.3V 供电，含 5pF 密勒补偿电容与 5kΩ 补偿电阻；仿真命令包含 `.op`（工作点）与 `.ac oct 100 1 40M`（AC 分析至 40MHz） |
| `Draft3.log` | LTspice 运行日志，可查看工作点求解结果与模型加载信息 |

### 工艺文件 / 工艺库

| 路径 | 说明 |
| --- | --- |
| `工艺文件/LTS_GFMCU/LTS_GFMCU/dmodels_analog.lib` | GFMCU 模拟器件模型库（BSIM4，3.3V MOS 器件） |
| `工艺文件/LTS_GFMCU/LTS_GFMCU/gfmcu_nmos3p3.asy` / `gfmcu_pmos3p3.asy` | 3.3V NMOS / PMOS 器件符号 |
| `工艺文件/LTS_GFMCU/LTS_GFMCU/gfmcu_*.asy` | 工艺配套符号：多晶硅电阻（npolyf/ppolyf）、N+/P+ 电容、PNP 管（vpnp）、理想电容/电感/电阻（gcap/gind/gres）等 |
| `工艺文件/LTS_GFMCU/LTS_GFMCU/axl_*.asc` | GFMCU 工艺自带示例电路：`axl_bias`（偏置）、`axl_first`（初测）、`axl_opamp`（运放）、`axl_rxblk`（接收模块）；对应的 `.log` 为仿真日志，`.raw` / `.op.raw` 为仿真结果数据 |
| `工艺文件/tsmc018/tsmc018.lib.txt` | TSMC 0.18μm 工艺 SPICE 模型（BSIM3 Level 49，含 CMOSN / CMOSP 模型参数） |
| `工艺文件/tsmc018/cmosn.asy` / `cmosp.asy` | TSMC 0.18μm NMOS / PMOS 器件符号 |

### 文档

| 文件 | 说明 |
| --- | --- |
| `模拟集成电路设计二课设.docx` | 课程设计报告（设计过程、仿真结果与分析） |
| `课设说明.docx` | 课程设计任务书 / 要求说明 |
| `EECS-2016-223.pdf` | 参考资料（英文文献） |

## 使用说明

1. 安装 [LTspice](https://www.analog.com/en/design-center/design-tools-and-calculators/ltspice-simulator.html)（建议使用较新版本，支持 BSIM4 模型）。
2. 将 `工艺文件/LTS_GFMCU/LTS_GFMCU/` 与 `工艺文件/tsmc018/` 中的 `.asy` 符号文件和 `.lib` 模型库拷贝到 LTspice 的符号/模型搜索目录（或用 `.include` 指令直接引用模型库）。
3. 用 LTspice 打开 `Draft3.asc`，运行仿真：
   - `.op`：查看直流工作点；
   - `.ac oct 100 1 40M`：AC 扫描，查看增益带宽积（GBW）、相位裕度等指标。
4. 如需使用 TSMC 0.18μm 模型，在原理图中将器件替换为 `cmosn` / `cmosp` 符号，并 `.include tsmc018.lib.txt`。

## 仿真环境

- 工具：LTspice（Windows）
- 工艺：GlobalFoundries GFMCU（3.3V）为主，另附 TSMC 0.18μm 模型备选
- 电源电压：3.3V

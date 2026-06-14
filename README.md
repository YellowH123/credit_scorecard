# 信用评分卡模型（银行信贷风控方向）

## 项目简介

本项目基于 Kaggle 的 **Give Me Some Credit** 信贷数据，构建了一套完整的信用评分卡模型，用于预测用户是否会在未来两年内发生违约（SeriousDlqin2yrs）。该模型可帮助银行或金融机构在贷前审批环节量化风险，辅助决策。

## 数据集

- 来源：[Give Me Some Credit](https://www.kaggle.com/c/GiveMeSomeCredit/data)（Kaggle竞赛）
- 样本量：150,000 条
- 特征数：12（包括年龄、负债率、收入、逾期次数等）
- 目标变量：`SeriousDlqin2yrs`（1=违约，0=正常）

## 项目流程

1. **数据清洗**
   - 删除无意义的行号列
   - 处理异常值（年龄 ≤0、逾期次数 ≥90）
   - 缺失值填补：月收入用中位数填充，家属数用众数填充

2. **特征工程**
   - 决策树分箱（`scorecardpy.woebin`）
   - WOE（证据权重）转换
   - IV（信息值）筛选，保留 IV > 0.02 的变量（共11个）

3. **建模与评估**
   - 逻辑回归（主模型）
   - 随机森林（对比模型）
   - 评估指标：AUC、KS、PSI

4. **评分卡转换**
   - 将逻辑回归模型转换为评分卡，输出每个变量分箱对应的分数
   - 示例：年龄 44 岁以下扣 14 分，负债率 >0.8 扣 30 分

5. **策略建议**
   - 高风险客群（年龄<44 且负债率>0.8）：违约率 23%（整体 3.5 倍）→ 建议降额或拒绝
   - 低风险客群（月收入>5万 且 无逾期）：违约率仅 3.4% → 建议提额至 20 万

## 主要结果

| 模型 | AUC (测试集) | KS (测试集) | PSI |
|------|--------------|-------------|-----|
| 逻辑回归 | **0.848** | **0.540** | **0.03** |
| 随机森林 | 0.860 | 0.570 | — |

- **PSI = 0.03**（<0.1），表明模型在训练集和测试集上分布稳定，具备上线潜力。

## 技术栈

- Python 3.13
- pandas, numpy, matplotlib
- scikit-learn
- scorecardpy

## 文件说明

- `credit_scorecard.ipynb`：完整的 Jupyter Notebook 代码（含数据清洗、分箱、WOE/IV、建模、评估、评分卡转换）
- `cs-training.csv`：原始数据集（未上传至 GitHub，请从 Kaggle 下载）
- `requirements.txt`：依赖库列表
- `README.md`：项目说明

## 如何运行

1. 克隆本仓库
2. 下载数据集并放置于同一目录下
3. 安装依赖：`pip install -r requirements.txt`
4. 按顺序运行 `credit_scorecard.ipynb` 中的单元格

## 项目链接

[GitHub 仓库地址](https://github.com/YellowH123/credit_scorecard.git)

## 作者

黄丝雨 - 西南交通大学 数学专业 硕士

## 致谢

- Kaggle 提供数据集
- scorecardpy 开发团队

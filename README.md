# Telco-Customer-Churn-Analysis
基于Python 的用户流失预测与归因分析项目
# 电信平台用户流失预警与精细化挽留策略 (Telco Customer Churn Prediction)

## 📌 项目背景
在电信行业中，获取新客户的成本远高于挽留老客户。本项目的目标是基于真实的电信用户数据，构建机器学习分类模型，**精准预测高流失风险用户**，并利用可解释性模型挖掘**导致流失的核心因素**，为运营团队提供数据驱动的挽留策略。

## 🛠️ 技术栈
*   **数据处理**: `Pandas`, `NumPy`
*   **特征工程**: `Scikit-learn` (KBinsDiscretizer 等频分箱, OneHotEncoder 独热编码, LabelEncoder)
*   **特征选择**: 方差过滤 (VarianceThreshold), 卡方检验 (Chi2 降维)
*   **机器学习**: `RandomForestClassifier`, `LogisticRegression` (L1 正则化)
*   **模型评估与归因**: 混淆矩阵, ROC-AUC, 特征重要性 (Feature Importance), 回归系数 (Coefficients)

## 📊 项目流程与核心结果

### 1. 数据清洗与特征工程
*   处理了 `TotalCharges` 列中隐藏的空格异常值（`pd.to_numeric` 强转）。
*   对连续变量（在网时长、月租费、总费用）进行**等频分箱**，有效缓解了长尾分布和异常值影响。
*   使用卡方检验（Chi2）将高维特征（36维）精准降维至 15 个核心特征，防止过拟合。

### 2. 模型构建与对比验证
*   **随机森林 (Random Forest)**：测试集 **AUC 达到 0.83**，召回率 79.2%。模型能有效从真实流失人群中抓取大部分高危用户。
*   **逻辑回归 (L1 正则化)**：为了交叉验证业务归因，构建了白盒模型。通过 L1 正则化自动剔除冗余的共线性特征（如各项 "No internet service"），输出清晰的系数解释。

### 3. 核心业务洞察与归因（双模型交叉验证）
结合随机森林的特征重要性与逻辑回归的系数正负，得出以下结论：
*   🚨 **促流失元凶**：
    *   `InternetService_Fiber optic` (光纤套餐用户流失率偏高，可能因价格敏感或网速体验问题)。
    *   `PaymentMethod_Electronic check` (使用电子支票手动付款用户易流失，存在付款摩擦)。
*   🛡️ **抗流失护盾**：
    *   `Contract_Two year` (两年长约用户极度稳定，是第一大留存护城河)。
    *   `OnlineSecurity_Yes` (办理网络安全服务的用户，沉没成本高，极难流失)。

## 💡 落地业务建议
1.  **推动长约锁定**：针对高风险的按月付费用户，推出“首月免单绑定一年”等促销，用长期合约锁定用户。
2.  **降低支付摩擦**：引导电子支票用户绑定信用卡自动扣款，每月给予 5 元减免，防止因忘交费导致的流失。
3.  **新客服务捆绑**：针对新注册的潜在高危用户，免费赠送 3 个月的“网络安全+技术支持”大礼包，培养使用习惯。

## 📁 文件说明
*   `Telco_Churn_Project.ipynb`: 完整的项目代码（包含数据清洗、建模、可视化与归因过程）。
*   `Project_Report.pdf / html`: 面向业务方的可视化分析报告。
*   `WA_Fn-UseC_-Telco-Customer-Churn.csv`

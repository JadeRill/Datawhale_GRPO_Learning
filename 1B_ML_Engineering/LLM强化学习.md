# 概念
- SFT
- RL
- PPO：Proximal Policy Optimization近端策略优化。
- GRPO：Group Relative Policy Optimization
	- 相对于PPO无需训练新的value model估计baseline，只需要训练policy model
- policy：正在训练的模型权重
- rollout：从prompt出发生成一次回答
- on-policy：训练结果来自于当前的权重采样
- reward：给回答的打分
- advantage：GRPO对于同一题目采样一定数目回答，advantage= 当前reward - 组内平均reward
- log_probs：每个token的对数比率
- LoRA
- OPD：on policy distillation
- Loss
	- Important Sampling
	- PPO
	- CISPO

## 方法
PPO： **Actor-Critic 架构**：Actor 决定动作，Critic 评估状态
1. **采样**：用当前策略与环境交互，收集状态、动作、奖励等轨迹；
    
2. **计算回报和优势**：评估动作好坏；
    
3. **更新策略网络**：用 PPO-Clip 目标优化 Actor；
    
4. **更新价值网络**：让 Critic 更准确估计状态价值；
    
5. **重复迭代**：不断采样和更新。
   
GRPO：
1. 对同一个问题，采样一组回答。
2. 给每个回答打 reward。
3. 不再额外训练 value model，而是用这一组回答的平均 reward 当 baseline。
4. 比组内平均分高的回答，advantage 为正；低的为负。
5. 用这个 advantage 去更新 policy。
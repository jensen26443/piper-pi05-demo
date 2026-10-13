# Piper × π0.5 Real-Robot Demos · 真机演示

**English TL;DR:** π0.5 (language backbone + action expert, LoRA fine-tuned, vision encoder frozen) controlling an AgileX Piper arm for multi-task block stacking on a real robot, with RTC-based asynchronous action execution. After the stack collapses mid-episode, the policy recovers **autonomously — there is no explicit retry logic or anomaly detector**; recovery emerges from the policy reacting to the changed observations via RTC replanning.

基于 π0.5 的 AgileX Piper 机械臂多任务方块堆叠（真机）：

- **底座模型**：π0.5（openpi），语言主干与动作专家做 LoRA 微调，冻结视觉编码器；多任务混训
- **数据**：自建双臂主从示教链路，抓放入盒与方块堆叠共 200 条轨迹；动作标签为实际下发控制目标（命令与反馈分离）
- **部署**：RTC 异步重规划，按实际完成动作数对齐动作块、补偿推理延迟

## Demo 1 · 倒塌后自主恢复（无显式重试逻辑）

堆叠过程中方块倒塌 → 策略基于倒塌后的观测自主重新抓取 → 对位微调 → 重新堆叠成功。全程无人工干预；系统中没有重试状态机或异常检测模块，恢复行为来自策略对观测变化的响应。

<video src="https://raw.githubusercontent.com/jensen26443/piper-pi05-demo/main/block_stacking_retry_after_collapse.mp4" controls preload="metadata" muted></video>

▶ 播放/下载：[block_stacking_retry_after_collapse.mp4](./block_stacking_retry_after_collapse.mp4)

## Demo 2 · 高精度对位与稳定保持

按颜色组合完成堆叠（绿叠橙），桌面其余方块为干扰物；放置后稳定保持到结尾。

<video src="https://raw.githubusercontent.com/jensen26443/piper-pi05-demo/main/block_stacking_stable_hold.mp4" controls preload="metadata" muted></video>

▶ 播放/下载：[block_stacking_stable_hold.mp4](./block_stacking_stable_hold.mp4)

## 真机评测

| 任务 | 成功率 |
| --- | --- |
| 抓放入盒 | 95%（19/20） |
| 方块堆叠 | 83%（25/30，其中未示教颜色方块 73%，8/11） |

评测说明：回合初始位姿随机化；成功率按终局状态判定（倒塌后自主恢复成功计入成功）。

---

*视频为真实运行录制。*

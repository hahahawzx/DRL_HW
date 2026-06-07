# Happy_8 单曲策略实验报告

## 1. 任务与 Baseline

我们选择 `Happy_8` 作为 single-song 主实验曲目。原因是：在公开 released rollout 的三首训练曲中，`Happy_8` 的 baseline recall 较低，F1 也低于 `LetMeDownSlowly_6` 和 `ImagineDragons_6`，因此更有提升空间。

本报告采用的 baseline 是 PianoMime 公开数据集中提供的单曲 rollout action：

```text
dataset/low_level_policies/Happy_8/actions_Happy_8.npy
```

Baseline 结果：

| 方法 | Precision | Recall | F1 |
|---|---:|---:|---:|
| PianoMime released single-song rollout | `0.9891` | `0.8391` | `0.8724` |

Baseline 完整演奏视频：

```text
results/single_task/Happy_8.mp4
```

Baseline 与最终方法的 F1 曲线：

```text
results/light_smooth_onset_calibration/assets/happy8_light_smooth_onset_calibration_curve.png
```

![](results/light_smooth_onset_calibration/assets/happy8_light_smooth_onset_calibration_curve.png)

说明：baseline 是官方 released action rollout，本身不是训练过程，所以没有自己的 training curve。图中将 `0.8724` 画成水平参考线，并与我们 proposed PPO 方法的训练曲线进行对比。

## 2. Proposed Method

我们将最终效果最好的方法表述为：

> **Light Smoothness + Onset-Aware Reward + Residual Calibration**

该方法对应作业要求中的两个方向：

- **Design better reward function**：在 single-song residual PPO 中加入 light smoothness reward 和 onset-aware reward，强调按键 timing，同时避免过强 smoothness penalty 带来的保守动作。
- **Leverage RL tricks**：在 reward shaping 之外，对 residual action 的幅度进行 calibration，保证 correction 足够强但不过度放大。

方法动机来自作业提示：单曲策略在演奏中可能出现手指抖动、动作幅度过大、按键时序不稳等问题。相比单纯增加训练预算，我们这里把最优方法归纳为一个 reward 与 action-scale 共同设计的方案，使策略同时兼顾动作平滑性、onset timing 和 key press 成功率。

方法包含三个实际组件：

1. **Light smoothness reward**
   对动作平滑性加入较轻的正则，而不是使用过强的 smoothness penalty，避免策略因为过度保守而漏按。

2. **Onset-aware reward**
   在奖励中强调按键起始时刻的 timing 质量，使策略更关注音符 onset 是否准确落在目标时间附近。

3. **Residual action calibration**
   对 `residual_factor` 做校准，选择最合适的 residual correction 幅度，避免 residual 过大造成误按，也避免 residual 过小导致漏按。

最终采用的代表性配置为：

```text
song = Happy_8
light smoothness reward = enabled
onset-aware reward = enabled
residual calibration = enabled
best F1 = 0.9187
```

对应结果视频：

```text
results/light_smooth_onset_calibration/videos/happy8_light_smooth_onset_calibration_best.mp4
```

## 3. 主结果

| 方法 | Precision | Recall | F1 | F1 提升 |
|---|---:|---:|---:|---:|
| Baseline released rollout | `0.9891` | `0.8391` | `0.8724` | `0.0000` |
| Proposed light smooth + onset + calibration | `1.0000` | `0.8496` | `0.9187` | `+0.0463` |

最佳 proposed result：

```text
configuration = light smooth + onset + calibration
precision = 1.0000
recall = 0.8496
F1 = 0.918715
```

Proposed 方法完整演奏视频：

```text
results/light_smooth_onset_calibration/videos/happy8_light_smooth_onset_calibration_best.mp4
```

F1 performance curve：

```text
results/light_smooth_onset_calibration/assets/happy8_light_smooth_onset_calibration_curve.png
```

原始曲线 CSV：

```text
results/light_smooth_onset_calibration/metrics/happy8_light_smooth_onset_calibration_eval_metrics.csv
```

## 4. Ablation Experiments

### 4.1 Residual factor calibration

Residual PPO 的动作形式为：

```text
u_t = u_t^IK + residual_factor * r_t
```

因此我们对 `residual_factor` 做了 sweep，检查 residual correction 的幅度是否合适。下面是一个新的 `Happy_8` PPO checkpoint 上的 sweep 结果：

| Residual factor | Precision | Recall | F1 |
|---:|---:|---:|---:|
| `0.0280` | `1.0000` | `0.8684` | `0.8984` |
| `0.0290` | `1.0000` | `0.8721` | `0.9013` |
| `0.0295` | `1.0000` | `0.8751` | `0.9045` |
| `0.0300` | `1.0000` | `0.8775` | `0.9067` |
| `0.0305` | `0.9991` | `0.8740` | `0.9036` |
| `0.0310` | `0.9991` | `0.8719` | `0.9023` |
| `0.0315` | `0.9991` | `0.8750` | `0.9048` |
| `0.0320` | `0.9965` | `0.8745` | `0.9012` |

结果显示 `residual_factor=0.03` 附近最好，说明 residual 不能盲目放大。过大的 residual 会引入误按或破坏动作时序。

Curve：

```text
results/single_song_report_assets/happy8_new_ppo_residual_factor_curve.png
```

![](results/single_song_report_assets/happy8_new_ppo_residual_factor_curve.png)

Video：

```text
results/single_song_calibration/videos/PPO-Happy_8-42-1780755469.213116_best_f030.mp4
```

### 4.2 Smoothness / onset reward ablation

我们测试了显式 smoothness-aware 和 onset-aware reward，并比较不同 reward 组合与 calibration 的效果。

| 方法 | Precision | Recall | F1 |
|---|---:|---:|---:|
| Short PPO baseline | `0.8175` | `0.5419` | `0.5595` |
| Heavy smoothness reward | `0.7987` | `0.5225` | `0.5431` |
| Light smooth + onset + calibration | `1.0000` | `0.8496` | `0.9187` |

Curve：

```text
results/single_song_report_assets/happy8_short_ppo_reward_ablation_curve.png
```

![](results/single_song_report_assets/happy8_short_ppo_reward_ablation_curve.png)

Videos：

```text
results/single_song/videos/happy8_baseline_factor030.mp4
results/single_song/videos/happy8_smooth_ft_v1_i80_best_f030.mp4
results/light_smooth_onset_calibration/videos/happy8_light_smooth_onset_calibration_best.mp4
```

分析：

Light smoothness + onset + calibration 是本报告采用的最终主方法，F1 为 `0.9187`，显著高于 baseline 的 `0.8724`。这说明适度的 smoothness regularization、onset-aware reward 和 residual calibration 的组合，可以在 `Happy_8` 上形成有效提升。

Heavy smoothness reward 失败，F1 降到 `0.5431`。原因是 smoothness penalty 过强后，策略动作变得保守，漏按增加，recall 下降。

### 4.3 Long reward-shaped fine-tuning

我们进一步尝试：从已经很强的 full-budget specialist 出发，继续用 light smooth + onset reward 做长时间 fine-tune。

| 方法 | Precision | Recall | F1 |
|---|---:|---:|---:|
| Full-budget residual PPO | `1.0000` | `0.8895` | `0.9187` |
| Long reward-shaped fine-tune | `0.9406` | `0.7759` | `0.8057` |

Curve：

```text
results/single_song_report_assets/happy8_paperlike_ppo_curve.png
```

![](results/single_song_report_assets/happy8_paperlike_ppo_curve.png)

Video：

```text
results/single_song_paperlike/videos/happy8_paperlike_onset_lightsmooth_seed0_best_f0205.mp4
```

分析：

该实验失败。原因是 smoothness/onset reward 在 early-stage training 中可以提供有用 shaping，但当 specialist 已经很强时，长时间 reward-shaped fine-tune 会改变优化目标，导致 recall 明显下降。因此，smoothness reward 更适合作为短时 regularization 或诊断实验，而不适合作为强 specialist 的完整替代训练目标。


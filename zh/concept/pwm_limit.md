# PWM_limit 状态机

[PWM_limit 状态机] 将预解锁(pre-armed)或解锁(armed)状态的输入经过一个函数f()后映射到最终的PWM输出。除此之外，该状态机在接收到解锁指令(armed signal)和完全进入解锁(armed)状态之间提供了一个油门的斜坡过度(ramp-up)过程。

## 总览

**输入**

- 解锁（armed）信号：激活后允许执行所有动作指令，包括危险的动作，如转动螺旋桨。
- 预解锁（pre-armed）信号：激活后只允许执行温和的动作指令，如移动控制舵面。 （螺旋桨/电机会被上锁）
    - 该输入会直接覆盖当前状态。
    - 激活 锁定（pre-armed）模式后无论飞控当前处于什么状态，飞控都会立刻终止状态 ON 的运转。
    - 取消 锁定（pre-armed）模式会使飞控返回到当前状态。

**状态**

- INIT 和 OFF 
    - pwm 输出设置为锁定状态(disarmed)的值。
- RAMP 
    - pwm 输出从锁定状态的值斜坡过渡到解锁状态(armed)的最小值。
- ON 
    - 根据实际控制量设定 pwm 的输出值。

## 状态转移图

![](../../assets/diagrams/pwm_limit_state_diagram.png)
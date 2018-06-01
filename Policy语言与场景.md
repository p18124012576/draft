## Policy语言规范扩充

### 问题描述

当前policy语言对状态的处理非常简单，只分为授权和非授权态。也不能对子合同有任何限制，无法满足新功能的需求。当前规划中的需求需要授权语言能够区分不同的授权场景，比如对已存在的授权点或节点进行访问授权并不同于授权依赖节点进行再签约。

### 初步设计

当前的计划是对状态权限进行细分，并且状态描述中可以带有限制条款。比如

```
for ALL:
  state_1:
    proceed to state_2 on event_1
  state_2:
    proceed to state_3 on event_2
    consumable
    recontractable with conditions:
      target:
        SELF
      agreement:
        0x1234
  state_3:
    terminate
```

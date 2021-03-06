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

在状态2描述中出现的consumable代表来自已签约乙方的访问可以通过，recontractable表示在当前状态下允许乙方节点生成合约。如果附加了conditions部分则表示有条件允许，比如这个例子中要求乙方只能自引用，不得开放给第三方，同时要求一个同意特定协议的步骤（这两个业务上有些矛盾，仅做例子）。对于签约对象的限制可以对乙方给自身指定的policy做合规检查，看其for语句对象是不是限制范围的子集，而对于协议，则必须从初始态遍历乙方策略描述的状态机，看看是否存在不经过所要求的协议就可以到达任意有授权的状态。

#### 状态描述
consumable, presentable, recontractable

#### 限制描述
target, agreement, exclude event

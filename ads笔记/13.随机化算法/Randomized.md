# Randomized Algorithm
## #案例
### #Hiring Problem
朴素算法：
记录当前最好的Q，如果新来的人比Q好，就雇佣。
最坏的情况：Q递增，要雇佣n次。
假设候选人顺序随机，那么第i个人被雇佣的概率是i/1，雇佣次数的期待是ln(n).
### #Online Hiring - hire only once
对于前k人只记录不雇佣，后面的人只要超过记录最佳的Q就雇佣。


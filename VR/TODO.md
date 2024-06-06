因时间紧迫未能得到优化的TODO

PlayerBehavior中Avatar骨骼同步，数据传输使用的byte[]会带来GC压力,应为NativeArray实现序列化方式以避免GC;
[Mirror 自定义类型序列化实现](https://mirror-networking.gitbook.io/docs/manual/guides/serialization)



GlobalEventBinding相关部分待重构 因为整个游戏也需要EventSystem
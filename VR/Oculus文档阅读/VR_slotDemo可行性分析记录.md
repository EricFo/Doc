
Avatar手部位置可由设置`OvrAvatarEntity`内的`_criticalJointTypes`获取，被设置的关节将变成Object显示在Hierarchy中



可以将碰撞体绑定在骨骼上，在SampleAvatarEntity中设置CriticalJointType可以让模型显示需要被绑定的骨骼点。之后将碰撞体绑定在上边，之后的穿戴系统也可以参照此做法。   
暂时碰撞体只使用正方形。

相关设置和脚本在D盘VR-SlotDemo的主场景里
`LiuHongSen分支`下是Avatar和其他东西的测试用例，比如添加Avatar手部碰撞体。是项目内的 `HandColliderHelper`。
# Slot端
Slot端使用以往的Demo架构整合RGS的后端，使用HTTP向服务器发送请求接收Json数据先解析成一个大的数据结构然后再依据其中数据实现前端功能，其中BaseGame和FreeGame每一次Spin都会发送一次请求，而其他玩法的数据包含再触发特殊玩法的那次Base Spin返回的数据中，与RGS一样.

## 网络数据
由于首次解析完整的数据结构较为庞大，所以在代码中会有将代码细分成对应不同模块对应的数据类并向不同模块发送的类`NetworkDataProcessor`。
当需要发送网络请求时应尽量在GameManager中通过`NetworkDataProcessor`发送，返回的数据将会由此类做处理之后发送到各个模块

此类依赖两种接口

``` Java
  public interface INetworkData
    {
        NetworkFilterFlag FilterFlag { get; }
    }

    public interface ISpinDataReceiver
    {
        void OnDataReceive(INetworkData result);

        public NetworkFilterFlag FilterFlag { get; }
    }
```

`NetworkDataProcessor`处理后的数据应继承`INetworkData`接口，而接收端通过实现`ISpinDataReceiver`的`OnDataReceive(INetworkData result)`回调接收数据，这两个接口中的`NetworkFilterFlag`将在发送时进行`与`判断当两个filter拥有相同的flag时接收端才能够接收到对应数据。所以实现时务必指定对对应的Flag。

`ISpinDataReceiver`暂时只会被`ISlotMachine`实现。

举例说明：      
`BaseReelManager`->`NetworkFilterFlag FilterFlag => NetworkFilterFlag.Base`     
`FreeReelManager`->`NetworkFilterFlag FilterFlag => NetworkFilterFlag.Free`      

当一个新的数据包的`FilterFlag`指明为`NetworkFilterFlag.Base`时，只有BaseReelManager会接收到带有该数据的回调。      
当一个新的数据包的`FilterFlag`指明为`NetworkFilterFlag.Base | NetworkFilterFlag.Free`时，`BaseReelManage`和`FreeReelManager`都会接收到该数据的回调。      

最终这些经过二次处理的数据会变成原Demo框架使用的类。


### GameResultParser
在上述的`NetworkDataProcessor`中需要将基础的表格结果解析出来然后通过前端画面显示赔付信息，而赔付信息分类两类：线赔与全陪，全赔对应结果类型`PayTableResult`。线赔对应结果类型`PayLineResult`.以上两种类型都是继承实现`ResultBase`。如果无法判断出具体赔付类型或是根本没有产生赔付将会返回Null;      


此处`GameResultParser`会直接根据后端的数据获取赔付类型信息然后产生对应类型的赔付结果，而前端只需要根据产生的数据播放线赔与全陪的表现实现即可。


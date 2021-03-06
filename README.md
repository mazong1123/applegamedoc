
## 通用参数
每个api都需要加上参数gameId=d58adea36cb54aa2a2ddeec276590463

## 获取当前用户流量币

GET http://xxx/GetCurrentUserMoney.do?tocken=099394834eiirejire009090920932067890098

### 参数
- token 用于识别用户身份。该值由流量银行跳转到游戏页面带过来，前台页面获取后每次调用api都需要传给后台

### 返回

```cpp
{
  code: '0000' // 0000表示成功。非0000表示失败, 失败信息在message字段里面
  message: ''
  data: {
    money: 123 // 当前用户的流量币
  }
}
```

## 开始游戏

POST http://xxx/StartGame.do

### 参数
- token 用于识别用户身份
- data 格式为 '赌注类型1=押注数量;赌注类型2=押注数量;...' 目前游戏中有9种类型的赌注，每种赌注的回报不同，详情如下:
<table>
<th>赌注类型</th>
<th>投入回报比</th>
<tr>
<td>铃铛</td>
<td>20</td>
</tr>
<tr>
<td>西瓜</td>
<td>15</td>
</tr>
<tr>
<td>橙子</td>
<td>10</td>
</tr>
<tr>
<td>葡萄</td>
<td>2</td>
</tr>
<tr>
<td>西红柿</td>
<td>5</td>
</tr>
<tr>
<td>西瓜</td>
<td>20</td>
</tr>
<tr>
<td>星星</td>
<td>30</td>
</tr>
<tr>
<td>数字77</td>
<td>40</td>
</tr>
<tr>
<td>BAR</td>
<td>100</td>
</tr>
</table>

由于前台画面已固定，所以后台需要按照上述顺序固定好赌注类型的Id, 例如前台如果传`1=5;3=10;7=2`, 代表用户押注铃铛5个币, 橙子10个币，星星2个币。

### 返回

```cpp
{
  code: '0000' // 0000表示成功。非0000表示失败, 失败信息在message字段里面
  message: ''
  data: {
    prizeId: 3 // 这次中奖的赌注Id
    isAward: true // true表示当前用户中奖，false表示当前用户未中奖
    money: 123 // 当前用户此时的流量币
    positionId: 0 // 从左上角开始顺时针计数，从0开始，表示这次应该落在哪个赌注上
  }
}
```

需要注意的是，现在的游戏如果用户中奖了，不会立刻把流量币发给用户，而是会弹出一个再次赌博的对话框。那个对话框之后决定是否要加流量币。详见下面的接口。

### 中奖后杠杆
游戏在用户中奖后，还会出现一个杠杆赌博游戏，即猜左右。如果猜对了，会将奖励翻倍，如果猜错了，奖励失效。
> 注: 这个杠杆游戏用户可以放弃，放弃的接口见后面。放弃后用户获得常规奖励。

GET http://xxx/StartLever.do?token=099394834eiirejire009090920932067890098&action=1

### 参数
- token 用于识别用户身份
- action 0 表示猜左, 1表示猜右

### 返回

```cpp
{
  code: '0000' // 0000表示成功。非0000表示失败, 失败信息在message字段里面
  message: ''
  data: {
    isAward: true // true表示当前用户杠杆中奖，false表示当前用户未中奖
    money: 123 // 当前用户此时的流量币
  }
}
```

### 放弃中奖后杠杆
用户可以放弃中奖后杠杆。如果放弃了中奖后杠杆，用户会获得常规奖励。

GET http://xxx/GiveupLever.do?token=099394834eiirejire009090920932067890098

### 参数
- token 用于识别用户身份

### 返回

```cpp
{
  code: '0000' // 0000表示成功。非0000表示失败, 失败信息在message字段里面
  message: ''
  data: {
    money: 123 // 当前用户此时的流量币
  }
}
```

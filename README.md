# 汉堡工具包

## Array

`Array` 是一个泛型数组类，支持常用数组操作方法，如 `push`、`pop`、`map`、`filter`、`for_each`、`splice` 等。  
支持自定义索引、只读长度属性、深拷贝、排序等高级功能。

示例用法：

```lua
local arr = Array()
arr:push(1)
arr:push(2)
arr:push(3)
local filtered = arr:filter(function(x) return x > 1 end)
filtered:for_each(function(i, v) print(i, v) end)
```

| 签名                                                                  | 用途                                                             |
| --------------------------------------------------------------------- | ---------------------------------------------------------------- |
| Array:push(value: T)                                                  | 向数组尾部追加元素                                               |
| Array:unshift(value: T)                                               | 向数组头部追加元素                                               |
| Array:pop() -> T                                                      | 去除头部元素并返回                                               |
| Array:shift() -> T                                                    | 去除尾部元素并返回                                               |
| Array:every(predicate: fun(item: T) -> boolean) -> boolean            | 检查每个元素是否满足指定条件                                     |
| Array:some(predicate: fun(item: T) -> boolean) -> boolean             | 检查是否有元素满足指定条件                                       |
| Array:find(predicate: fun(item: T) -> boolean) -> T?                  | 找到抵押给满足条件的元素，否则返回nil                            |
| Array:map(predicate: fun(item: T) -> G) -> Array<G>                   | 映射数组的所有元素得到一个新的数组                               |
| Array:slice(start: integer, over: integer, step: integer) -> Array<T> | 将数组切片                                                       |
| Array:splice(index: integer, count: integer, ...: T...) -> Array<T>   | 将数组拼合，删除index之后的count个元素并插入T...得到一个新的数组 |
| Array:filter(predicate: fun(item: T) -> boolean) -> Array<T>          | 过滤掉满足条件的元素并得到新的数组                               |
| Array:index_of(item: T) -> integer                                    | 得到数组中指定元素的索引，否则返回-1                             |
| Array:clear()                                                         | 清空数组所有元素                                                 |
| Array:for_each(func: fun(index: integer, item: T))                    | 遍历数组所有元素并执行回调函数                                   |
| Array:sort(comparator: fun(a: T, b: T) -> boolean)                    | 在数组内排序数组                                                 |
| Array(from?: T[] \| Array<T>) -> Array<T>                             | 构造数组，可通过已有的lua数组或Array构造                         |


## Class

`Class` 提供简单的类继承和实例化机制，支持多继承、自定义 getter/setter、销毁方法等。  
可用于构建复杂的面向对象结构。

示例用法：

```lua
local MyClass = Class("MyClass")
function MyClass:init()
    self.value = 123
end
local obj = MyClass()
print(obj.value)
obj:destroy()
```

### 创建一个类
```lua
---@class ClsName : ClassUtil, Parent...
local ClsName = Class("ClsName", Parent...) ---Parent为继承的类
```

### getter & setter

| 签名                                             | 用途                                                                                              |
| ------------------------------------------------ | ------------------------------------------------------------------------------------------------- |
| function ClsName:\_\_get\_\[attribute\]\(\)      | attribute是属性名称，通过它可以设置getter方法，可通过Instance.attribute获取属性而无需调用getter   |
| function ClsName:\_\_set\_\[attribute\]\(value\) | attribute是属性名称，通过它可以设置setter方法，可通过Instance.attribute = xxx使用setter而无需调用 |

> ### 示例
> ```lua
> function Dog:__get_name()
>    return self._name
> end
> function Dog:__set_name(value)
>    self._name = value
> end
> local xiao_huang = Dog()
> xiao_huang.name = "小黄"
> print(xiao_huang.name) --- 输出："小黄"

### 析构
可为类配置destroy方法，会在Instance:destroy()调用时，先调用类的destroy而后销毁对应的实例（所有的属性将解引用，当整个表不再被任意地方引用时会从内存中删除）

### 自定义索引和新索引
通过ClsName:__custom_index和ClsName:__custom_new_index可实现lua元表中的__index和__new_index方法，这两个方法都要求有返回值且不为nil，否则找不到值的时候会报错

---

## Frameout

`Frameout` 提供帧计时器工具，可按指定帧间隔重复执行回调，支持暂停、恢复和销毁。


示例用法：

```lua
local timer = SetFrameOut(30, function(f)
    print("当前帧:", f.frame)
end, 10, true)
---立刻输出一次当前帧，并在未来的9个30帧后分别输出一次当前帧
timer.pause() ---暂停计时器
timer.resume() ---恢复计时器
timer.destroy() ---销毁计时器
```

详细接口请参考 [Frameout.lua](Frameout.lua)。

## 工具程序文件名称
- Array.lua
- Class.lua
- Frameout.lua

# 贡献

豆油汉堡
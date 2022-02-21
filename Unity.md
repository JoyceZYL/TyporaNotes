### 全局变量

#### 赋值过程

```c#
public class Test : MonoBehaviour {
    
    public int a = 10;
    void Awake(){
        a = 20;
    }
    void Atart{
        a = 30;
    }
}
```

- a 是 public 类型，会在 unity 面板中显示，假设Unity 面板中 a 的值为 15



**赋值顺序**

1. 定义时 a = 10
2. 被 unity 面板中 15 替换
3. 调用 Awake，替换为20
4. 调用 Start，替换为30



- 初始化公共变量最好放在 Start 中
- 可以使用 [HideInInspector] 让变量不在 Unity 面板中显示
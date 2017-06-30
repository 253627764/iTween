# Gridmovement案例讲解

* Gridmovement案例介绍（可以通过官网查看）

* 根据需求搭建此案例场景，创建相关素材

* 分析案例功能，编写代码完成功能

## Gridmovement案例介绍

### 案例参考

[案例参考网址](http://www.itween.pixelplacement.com/examples.php)

### 场景如图所示

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/iTween%E7%BB%BC%E5%90%88%E6%A1%88%E4%BE%8B/images/Image2.png)

### Gridmovement场景搭建

#### 搭建场景

* 创建场景添加素材

* 需找规律

* 利用代码创景场景

* 根据需求调整场景

### 完成效果如下图所示

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/iTween%E7%BB%BC%E5%90%88%E6%A1%88%E4%BE%8B/images/Image3.png)

## Gridmovement功能详解

* 编写程序代码

首先创建场景，搭建小块地板，每相邻的两块地板颜色不同，创建小球，将此脚本挂载到摄像机上

```C#
using UnityEngine;
using System.Collections;

public class GridmovementDemo : MonoBehaviour {

    public GameObject floor;
    public GameObject ball;
    public int row = 9;  //地板的行数
    public int lie = 9;  //地板的列数

    void Awake()
    {
        floor = Resources.Load("Cube") as GameObject;
        
    }

    void Start()
    {
        //遍历生成9*9的地板
        for (int i = 0; i < row; i++)
        {
            for (int j = 0; j < lie; j++)
            {
                GameObject floors = GameObject.Instantiate(floor, new Vector3(j, 0, i), Quaternion.identity) as GameObject;
                floors.transform.SetParent(Camera.main.transform);  //将生成的cube作为Main Camera的子对象
                if ((i + j) % 2 == 0) //每相邻的两块地板颜色不同
                {
                    floors.GetComponent<MeshRenderer>().material.color = Color.black;  //j将符合条件的小块设置为黑色
                }
            }
        }
        //在指定的位置生成一个小球，Resources.Load("Sphere")是在Project面板中的Resources文件夹中的取出名为“Sphere”的预制体生成
        ball = GameObject.Instantiate(Resources.Load("Sphere"), new Vector3(row / 2, 0.6f, lie / 2), Quaternion.identity) as GameObject;
    }
    GameObject currentTarget;   //持有点击地板对象
    public static bool isMoving = false;
    void Move(GameObject target)
    {
        //每次点击的地板不是null，并且本次点击的和上一次点击的不是同一块地板
        if (currentTarget != null && target != currentTarget)
        {
            currentTarget.SendMessage("Fun");   //发送消息，调用“Fun”方法
        }
        currentTarget = target;  //将本次点击的地板赋给currentTarget， 用于判断下次点击是否与这次点击相同
        isMoving = true;  //点击后小球是移动状态

        //小球移动                       //先移动x轴方向，在移动z轴方向，距离是点击位置与球原本位置的差  //时间为1秒，移动类型，z轴移动与x轴间隔时间为1秒，z轴移动完调用Reset方法，执行对象为this.gameObject
        iTween.MoveBy(ball, iTween.Hash("x", (target.transform.position.x - ball.transform.position.x), "time", 1f, "easetyp", iTween.EaseType.easeInOutCirc));
        iTween.MoveBy(ball, iTween.Hash("z", (target.transform.position.z - ball.transform.position.z), "time", 1f, "delay", 1f, "easetyp", iTween.EaseType.easeInOutCirc, "oncomplete", "Reset", "oncompletetarget", this.gameObject));
    }
    void Reset()
    {
        isMoving = false;  //小球的状态为未运动
    }
}
```

地板的变化,将此脚本挂载到小方块预制体上，这样能保证每个方块都有此脚本

```C#
using UnityEngine;
using System.Collections;

public class FloorDemo : MonoBehaviour {

    Color StartColor; //持有初始时的颜色
    bool isActive;  //设置地板是否被点击
    void Start()
    {
        //获取小方块的出时颜色
        StartColor = this.gameObject.GetComponent<MeshRenderer>().material.color;
    }
    //当鼠标放在小方块上时触发事件
    void OnMouseEnter()
    {
        if(!isActive){  //如果小方块未被点击
            if (!GridmovementDemo.isMoving) //小球没有移动
            {
                //小方块改变颜色为绿色，并在y轴正方向上移动
                iTween.ColorTo(this.gameObject, Color.green, 0.1f);
                iTween.MoveTo(this.gameObject, iTween.Hash("y", 0.2f, "time", 0.1f));
            }
            else  //如果小球正在移动
            {
                //小方块改变颜色为黄色，并在y轴正方向上移动
                iTween.ColorTo(this.gameObject, Color.yellow, 0.1f);
                iTween.MoveTo(this.gameObject, iTween.Hash("y", 0.2f, "time", 0.1f));
            }
        }
    }

    //当鼠标离开小方块的时候触发事件
    void OnMouseExit()
    {
        if (!isActive)  //小方块没被点击
        {
            //改变颜色为本来颜色，并移动到原来的位置
           iTween.ColorTo(this.gameObject, StartColor, 0.1f);
           iTween.MoveTo(this.gameObject, iTween.Hash("y", 0, "time", 0.1f));
        }
    }
    //当鼠标点击时触发事件
    void OnMouseDown()
    {
        if (!GridmovementDemo.isMoving)  //小球没有移动
        {
            isActive = true;  //小方块被点击
            //改变颜色为红色，并移动到原来的位置
            iTween.MoveTo(this.gameObject, iTween.Hash("y", 0, "time", 0.1f));
            iTween.ColorTo(this.gameObject, Color.red, 0.1f); 
            //向父对象发送消息调用"Move"方法，参数为此对象
            this.gameObject.SendMessageUpwards("Move", this.gameObject);
        }
    }

    void Fun()
    {
        isActive = false;  //设置小方块的状态时未被点击
        iTween.ColorTo(this.gameObject, StartColor, 0.1f);  //改变回原来的颜色
    }
}

```


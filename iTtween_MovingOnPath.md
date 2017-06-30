# MovingOnPath案例讲解

* 了解MovingOnPath案例（可以通过官网查看）

* 根据需求搭建此案例场景

* 分析案例功能模块

* 完成功能演示项目

## 了解MovingOnPath案例

### 案例参考：

[官方案例网址：](http://www.itween.pixelplacement.com/examples.php)

## 根据需求搭建此案例场景

### 基本步骤：

首先导入素材 —— 制作场景所需对象 —— 完成搭建场景

## 分析案例功能模块

### 主要功能模块

* 运行游戏时，Replay按钮隐藏，物体按照固定的轨迹移动

* 完成运动后，Replay按钮激活，点击RePlay物体重复运动

## 完成功能演示项目

为物体设置一些运动轨迹,本示例中是创建了几个空物体，放在不同位置作为物体运动的轨迹节点，在脚本中，创建Transfome数组，将轨迹节点放在数组中，物体就会按照轨迹移动

### 核心功能实现

* 利用API编写相关代码,代码如下所示：

```C#
using UnityEngine;
using System.Collections;

public class MovingOnPath : MonoBehaviour {

    //路径寻路中的所有点
    public Transform[] paths;
    GameObject button;
    void Start()
    {
        button = GameObject.Find("Canvas/Button");
        button.SetActive(false);
        Hashtable args = new Hashtable();
        //设置路径的点
        args.Add("path", paths);
        //设置类型为线性，线性效果会好一些。
        args.Add("easeType", iTween.EaseType.linear);
        //设置寻路的速度
        args.Add("speed", 10f);
        //是否先从原始位置走到路径中第一个点的位置
        args.Add("movetopath", true);
        //是否让模型始终面朝当面目标的方向，拐弯的地方会自动旋转模型
        //如果你发现你的模型在寻路的时候始终都是一个方向那么一定要打开这个
        args.Add("orienttopath", true);
        args.Add("oncomplete", "Show");

        //让模型开始寻路	
        iTween.MoveTo(gameObject, args);
        
    }

    void Update()
    {
        iTween.MoveUpdate(Camera.main.gameObject, iTween.Hash("x", this.transform.position.x, "y", this.transform.position.y + 10, "speed", 10));
    }

    void Show()
    {
        button.SetActive(true);
    }

    public void OnButtonClick()
    {
        Start();
    }
   
}

```

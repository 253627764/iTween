# WorkingWithPhysics案例讲解

* 了解WorkingWithPhysics.案例（可以通过官网查看）

* 根据需求搭建此案例场景

* 分析案例功能模块

* 完成功能演示项目

## 了解WorkingWithPhysics.案例

### 案例参考：

[官方案例网址](http://www.itween.pixelplacement.com/examples.php)

## 根据需求搭建此案例场景

### 基本步骤：

首先导入素材 —— 制作场景所需对象 —— 完成搭建场景

## 分析案例功能模块

### 主要功能模块

* 游戏开始时生成一堵墙，由若干小方块组成

* 点击鼠标左键，大棒想做旋转，到一定角度后，向右挥棒，打在墙上

* 小方块被击飞，运动的方块颜色变红，静止后恢复颜色

## 完成功能演示项目

* 要想大棒不围绕自身重心旋转，可以将大棒放在空物体中，作为子对象，并将大棒的一端与空物体位置相同；也可以设置大棒的知心（没试过）

### 核心功能实现

* 利用API编写相关代码,代码如下所示：

将脚本放在空物体上（但是放在大棒上好像也行）

```C#
using UnityEngine;
using System.Collections;

public class WorkingWithPhysics : MonoBehaviour {

    private GameObject cube;
    public int row = 9;
    public int lie = 9;

    GameObject obj;
    void Awake()
    {
        cube = Resources.Load("CubeW") as GameObject;   //在Resources文件夹中取出Cube预制体给wall
        obj = GameObject.Find("GameObject");
    }
	// Use this for initialization
	void Start () {
        for (int i = 0; i < row; i++)
        {
            for (int j = 0; j < lie; j++)
            {
                GameObject wall = GameObject.Instantiate(cube, new Vector3(-0.5f, j + 0.5f, i - 2.5f), Quaternion.identity) as GameObject;
                //wall.AddComponent<Rigidbody>();
                wall.transform.SetParent(Camera.main.transform);
            }
        }
	}
	
	// Update is called once per frame
	void Update () {
        Move();
	}

    void Move()
    {
        if (Input.GetKeyDown(KeyCode.Mouse0))
        {
            iTween.RotateTo(obj, iTween.Hash("y", -70, "time", 3f, "easetype", iTween.EaseType.easeInOutCubic));
            iTween.RotateTo(obj, iTween.Hash("y", 50, "time", 1f,"delay", 3, "easetype", iTween.EaseType.easeInCirc));
        }
    }
}

```

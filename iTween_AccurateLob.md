## Accurate Lob案例讲解

* 了解Accurate Lob案例（可以通过官网查看）

* 根据需求搭建此案例场景

* 分析案例功能模块

* 完成功能演示项目

## 了解AccurateLob案例

### 案例参考：

[官方案例网址](http://www.pixelplacement.com/itween/examples.php)

## 根据需求搭建此案例场景

### 基本步骤：

* 1. 首先导入素材； 

* 1. 制作场景所需对象:
  * 1. 创建一个plane，改名为BlastArea，贴一张皮肤，BlastArea；
  * 1. 创建一个Quad， 改名为crosshair，贴一张皮肤，crosshair，设置Shader为GUI/Text Shader或者其他（主要是把背景白板隐化）
  * 1. 创建一个球的预制体

* 1. 完成搭建场景


## 分析案例功能模块

### 主要功能模块

* 图片跟随鼠标坐标移动

* 诞生物体在坐标原点

* 物体诞生后以抛物线形式运动，并消失物体

## 完成功能演示项目

### 核心功能实现

* 利用API编写相关代码,代码如下所示：

```C#
using UnityEngine;
using System.Collections;

public class MouseDemo : MonoBehaviour {

    public GameObject obj;  //持有一个预制体,为球
    GameObject crosshair;   //持有一个游戏对象，为准星
    Hashtable hashtable;
    
	void Start () {
        crosshair = GameObject.Find("crosshair").gameObject;
        hashtable = new Hashtable();
        

	}
	
	// Update is called once per frame
	void Update () {
        Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);   //由摄像机向鼠标在屏幕中的位置生成射线
        RaycastHit hit;  //用于接收射线的碰撞信息
        if (Physics.Raycast(ray, out hit))
        {
            if (hit.transform.name == "BlastArea")
            {
                //crosshair.transform.position = hit.point;
                //crosshair.transform.position.y = 0.1f;
                crosshair.transform.position = new Vector3(hit.point.x, 0.1f, hit.point.z);
                //iTween.MoveUpdate(crosshair, new Vector3(hit.point.x, 0.1f, hit.point.z), 0.1f);
                if (Input.GetMouseButtonDown(0))
                {
                    GameObject ball = GameObject.Instantiate(obj, this.transform.position, Quaternion.identity) as GameObject;
                    ball.GetComponent<SimpleSphere>().Move(hit.point);
                    Destroy(ball, 3f);
                }
            }
        }
	}
}

```

### 小球的移动效果
```C#
using UnityEngine;
using System.Collections;

public class SimpleSphere : MonoBehaviour {


    Vector3[] v3s = new Vector3[3];

    public void Move(Vector3 trs)
    {
        iTween.MoveTo(this.gameObject, iTween.Hash("path", Point(trs), "time", 3f));
    }

    Vector3[] Point(Vector3 trs)
    {
        v3s[0] = GameObject.Find("BlastArea").transform.position;
        v3s[1] = new Vector3(trs.x / 2, 3f, trs.z / 2);
        v3s[2] = trs;
        return v3s;
    }
}

```

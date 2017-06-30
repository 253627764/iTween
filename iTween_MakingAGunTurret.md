# Making a gun turret案例讲解

* 了解Making a gun turret案例（可以通过官网查看）

* 根据需求搭建此案例场景

* 分析案例功能模块

* 完成功能演示项目

# 了解Making a gun turret案例

## 案例参考：

[官方案例网址：](http://www.itween.pixelplacement.com/examples.php)

## 根据需求搭建此案例场景

### 基本步骤：

首先导入素材 —— 制作场景所需对象 —— 完成搭建场景

### 分析案例功能模块

#### 主要功能模块

* 图片跟随鼠标坐标移动

* 物体对象随着鼠标移动而旋转，始终看向鼠标的坐标

* 点击鼠标左键时，诞生物体在对象坐标原点

* 物体诞生后向鼠标坐标位置移动，一段时间后物体消失

### 完成功能演示项目

#### 核心功能实现

* 利用API编写相关代码,代码如下所示：

改脚本挂载到物体对象上

物体对象随着鼠标的移动而转动，始终看向鼠标坐标，点击鼠标左键时生成子弹，子弹向鼠标坐标移动，5秒后消失
```C#
using UnityEngine;
using System.Collections;

public class MakingGun : MonoBehaviour {

    Ray ray;
    RaycastHit hit;
    GameObject obj;  //准星
    GameObject gun;  //子弹
	// Use this for initialization
	void Start () {
        obj = GameObject.Find("Quad");
        gun = Resources.Load("GunSphere") as GameObject;
	}
	
	// Update is called once per frame
	void Update () {
        ray = Camera.main.ScreenPointToRay(Input.mousePosition);
        if (Physics.Raycast(ray, out hit))
        {
            if (hit.transform.name == "Plane")
            {
                obj.transform.position = hit.point;
                iTween.LookTo(this.gameObject, hit.point, .1f);
                
            }
        }
        if (Input.GetMouseButtonDown(0))
        {
            GameObject Gun = GameObject.Instantiate(gun, this.transform.position, Quaternion.identity) as GameObject;
            Gun.GetComponent<GunDemo>().Move(hit.point);
            //Gun.GetComponent<GunDemo>().Move(this.transform.position);

            Destroy(Gun, 5f);
        }
	}
}

```

改脚本挂载到子弹预制体上

```C#
using UnityEngine;
using System.Collections;

public class GunDemo : MonoBehaviour {

    Rigidbody rb;
    Vector3[] v3s = new Vector3[2];
    void Start()
    {
        rb = this.GetComponent<Rigidbody>();
    }
    public void Move(Vector3 v)
    {
        iTween.MoveTo(this.gameObject, iTween.Hash("path", Point(v), "speed", 10f));
        //rb.AddForce((v)* 100);
    }

    Vector3[] Point(Vector3 trs)
    {
        v3s[0] = transform.position;
        v3s[1] = new Vector3((trs.x - transform.position.x) * 100, trs.y + 1, (trs.z - transform.position.z) * 100);
        return v3s;
    }
}

```

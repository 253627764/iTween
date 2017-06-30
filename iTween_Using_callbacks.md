# Using callbacks案例讲解

* Using callbacks案例介绍（可以通过官网查看）

* 搭建案例场景，创建相关素材

* 分析案例功能,编写代码完成功能

## Using callbacks案例介绍

### 案例参考：

[官方案例网址](http://www.itween.pixelplacement.com/examples.php)

### 如图所示：

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/iTween%E7%BB%BC%E5%90%88%E6%A1%88%E4%BE%8B/images/Image.png)

## Using callbacks场景搭建

### 搭建场景:

* 创建场景添加素材
  * 1. 创建Plane，Cube，搭建GUI按钮Button

* 根据需求调整场景

## 完成效果如下图所示：

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/iTween%E7%BB%BC%E5%90%88%E6%A1%88%E4%BE%8B/images/Image1.png)

## Using callbacks功能详解

## 如何实现：

* 编写程序代码

将代码挂载到Cube上

```C#
using UnityEngine;
using System.Collections;
using UnityEngine.UI;

public class Callbacks : MonoBehaviour {


    private Text text;
    GameObject camera;
    GameObject button;

    void Awake()
    {
        //获取Text组件
        text = GameObject.Find("Canvas/Text").GetComponent<Text>();
        camera = GameObject.Find("Main Camera");
        button = GameObject.Find("Canvas/Button");
        button.SetActive(false);
        button.GetComponent<Button>().onClick.AddListener(OnButtonClick);
    }
	// Use this for initialization
	void Start () {
        //物体移动 MoveBy移动距离是amount,而MoveTo是移动到amount位置
        //"y", 3f 是在y轴上移动3米，"time", 1.5f 是在1.5秒内完成，"easetype",..是缓动运算
        //"onstart", "Show" 是在开始时调用Show方法，"onstarttarget",this.gameObject,目标对象  "onstartparams",  "上升开始..",Show方法里要传的参数
        //"delay",2f  是间隔2秒在运行iTween.RotateBy
        //"oncomplete", "ButtonShow" 是结束时调用ButtonShow方法，"oncompletetarget",this.gameObject, 是指执行对象，"oncompleteparams","全部结束" 是给方法传的参数。
        iTween.MoveBy(this.gameObject, iTween.Hash("y", 3f, "time", 1.5f, "easetype", iTween.EaseType.easeInOutCubic,
            "onstart","Show", "onstarttarget", this.gameObject, "onstartparams",  "上升开始..",
            "oncomplete", "Show", "oncompletetarget", this.gameObject, "oncompleteparams", "上升结束.."));
        iTween.RotateBy(this.gameObject, iTween.Hash("z", 0.25, "time", 1.5, "delay", 2f,
            "onstart", "Show", "onstarttarget", this.gameObject, "onstartparams", "旋转开始..",
            "oncomplete", "Show", "oncompletetarget", this.gameObject, "oncompleteparams", "旋转结束.."));
        iTween.MoveBy(this.gameObject, iTween.Hash("x", -2.5f, "time", 1.5f, "delay", 4f,"easetype", iTween.EaseType.easeInCubic,
            "onstart", "Show", "onstarttarget", this.gameObject, "onstartparams", "下降开始..",
            "oncomplete", "Show", "oncompletetarget", this.gameObject, "oncompleteparams", "下降结束.."));
        iTween.ShakeRotation(camera, iTween.Hash("x", 2f, "time", 2f, "delay", 5.6f,
             "onstart", "Show", "onstarttarget", this.gameObject, "onstartparams", "振动开始..",
            "oncomplete", "ButtonShow", "oncompletetarget", this.gameObject, "oncompleteparams", "全部结束.."));

	}

    void ButtonShow(string text)
    {
        this.text.text += text + "\n";
        button.SetActive(true);
    }

    public void OnButtonClick()
    {
        this.transform.rotation = Quaternion.identity;
        this.transform.position = new Vector3(0, 0.5f, 0);
        text.text = "";  // 等价于 text.text = string.Empty
        button.SetActive(false);
        Start();
    }

    void Show(string text)
    {
        this.text.text += text+"\n";  //this用来区分变量与参数的（因为变量和参数同名会报错）
    }
}

```

实现相应功能

完成案例效果

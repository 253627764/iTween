## iTween基本介绍

iTween是一个动画库,作者创建它的目的就是最小的投入实现最大的产出.让你做开发更轻松,用它可以轻松实现各种动画,晃动,旋转,移动,褪色,上色,控制音频等等

## iTween基本原理

iTween的核心是数值插值，简单说就是给iTween两个数值(开始值，结束值)，它会自动生成一些中间值，大概像这样子, 开始值->中间值 -> 中间值 …. -> 结束值。
这里的数值可以理解为: 数字，坐标点，角度，物体大小，物体颜色，音量大小 等

## iTween的下载地址

[官网](http://itween.pixelplacement.com)

## iTween 类介绍

iTween类的公共操作接口均以静态方法的形式提供。可分为三大类：

* 1.静态注册方法：提供注册动画效果的静态方法接口。如：MoveTo、CameraFadeTo等。

* 2.Update静态方法：提供每帧改变属性值的环境，在Update或循环环境中调用。如：MoveUpdate、AudioUpdate等。

* 3.外部工具方法：包括动画控制、路径绘制等。

iTween类内部定义了三种枚举类型：

* 1.EaseType：缓动类型枚举

[缓动类型枚举](http://robertpenner.com/easing/easing_demo.html)

* 2.LoopType：动画的循环类型枚举

* 3.NamedValueColor:已命名颜色枚举

## 主要功能介绍

### 八种实现动画方法：

* 1. Fade：淡入淡出

```C#
//FadeTo淡入淡出（只能改变GUIText和GUITexture的alpha值，不能改变其他物体的alpha值）
image  = GameObject.Find("Image");
iTween.FadeTo(image, 0.1f, 1f);  //alpha的值由初始值，在1秒内逐渐变到0.1
//iTween.FadeFrom(image, 0.1f, 1f);  //alpha的值由0.1，在1秒内逐渐变到初始值
//iTween.FadeUpdate(image, 0.1f, 1f);  //因为是每一帧都调用，所以要放在Update每调用， 效果和FadeTo差不多
iTween.LookUpdate(obj, this.transform.position, 5f); 放在Update里调用

```

* 2. Look：旋转对象使其面朝指定位置

```C#
        //LookTo  看向、朝向，物体obj看向目标物体this.gameobject
        //LookFrom  物体obj视线（z轴正方向）远离目标物体，回到obj本身的z轴位置
        //LookUpdate obj的z轴正方向朝向目标方向，因为是每一帧都调用，所以要放在Update每调用
        obj = GameObject.Find("Capsule");
        //iTween.LookTo(obj, this.transform.position, 5f);  
        //iTween.LookFrom(obj, this.transform.position, 5f);
        //iTween.LookUpdate(obj, this.transform.position, 5f);
        //iTween.LoopType  是什么？？
        //iTween.MoveUpdate(obj, this.transform.position, 5f);  放在Update里调用
```

* 3. Move：移动位置，

```C#
        //1.固定传参        要移动的物体       目标位置           移动所用时间
        //iTween.MoveTo(this.gameObject, new Vector3(3, 3, 3), 2f);

        //2.传入哈希表的值（哈希表是纯键值对）
        //首先创建哈希表
        Hashtable hashtable = new Hashtable();
        //其次添加键和值，查找iTween的MoveTo内容，查看它里面对应的哈希表的键
        hashtable.Add("point", new Vector3(5, 5, 5));
        //hashtable.Add("time", 5f);
        //hashtable.Add("speed", 0.1f);
        //iTween.MoveTo(this.gameObject, hashtable);
        //Move  移动位置
        //MoveTo(GameObject target, Vector3 position float time),物体target在time秒内移向目标位置position,快慢的效果
        //iTween.MoveTo(obj, this.transform.position, 5f);
        //MoveFrom(GameObject target, Vector3 position float time),物体target在time秒内从目标位置position,移回初始位置，快慢的效果
        //iTween.MoveFrom(obj, this.transform.position, 5f);
        //MoveBy(GameObject target, Vector3 position float time),物体target在time秒内移向目标位置position,快慢的效果
        //iTween.MoveBy(obj, this.transform.position, 5f);
        //MoveAdd(GameObject target, Vector3 amount, float time),物体target在time秒内移向目标位置position,快慢的效果
        //iTween.MoveAdd(obj, this.transform.position, 5f);  
        //MoveUpdate(GameObject target, Vector3 amount, float time)  因为是每一帧都调用，所以要放在Update每调用 ,效果和MoveTo差不多
        //iTween.MoveUpdate(obj, this.transform.position, 5f); 放在Update中调用
        
```

* 4. Rotate：旋转指定欧拉角度

```C#
//Rotate:旋转指定欧拉角度
        //RotateAdd(GameObject target, Vector3 amount, float time)  随着time过去，向物体target提供一个欧拉角度旋转向目标位置
        //iTween.RotateAdd(obj, this.transform.position, 5f);
        //RotateBy(GameObject target, Vector3 amount, float time)  把提供的值乘以360，然后随着时间旋转游戏物体的角度按计算得的值。
        //iTween.RotateBy(obj, new Vector3(0,1,0), 5f);  //例如（0，1，0）,就是绕Y轴旋转360度。 顺时针旋转
        //RotateFrom(GameObject target, Vector3 amount, float time) 立即改变游戏物体角度的欧拉角，然后随着时间旋转回原来的角度，属性提供的欧拉角为变化初始值
        //iTween.RotateFrom(obj, new Vector3(0, 1, 0), 5f);
        //RotateTo(GameObject target, Vector3 amount, float time) 旋转游戏物体角度到我们所提供的欧拉角角度amount。
        //iTween.RotateTo(obj, new Vector3(0, 10, 0), 5f);
        //RotateUpdate(GameObject target, Vector3 amount, float time)旋转游戏物体角度到我们所提供的欧拉角角度amount。
        //iTween.RotateUpdate(obj, new Vector3(0, 180, 0), 5f);
```

* 5. Scale：缩放大小

```C#
 //Scale:缩放大小
        //ScaleAdd(GameObject target, Vector3 amount, float time)随着时间根据提供的amount(Vector3)增加游戏物体的大小
        //iTween.ScaleAdd(obj, new Vector3(3, 3, 3), 5f);
        //ScaleBy(GameObject target, Vector3 amount, float time)
        //随着时间变形游戏物体，游戏物体最终变形大小由我们提供的amount(Vector3)值决定 算法：最终变形大小=游戏物体初始的sacle * 我们提供的 amount值
        //iTween.ScaleBy(obj, new Vector3(3, 3, 3), 5f);
        //ScaleFrom(GameObject target, Vector3 amount, float time)
        //立即改变游戏物体的比例大小amount，然后随时间返回游戏物体原本的大小。
        //iTween.ScaleFrom(obj, new Vector3(3, 3, 3), 5f);
        //ScaleTo(GameObject target, Vector3 amount, float time)随着时间改变物体的比例大小到我们提供的Scale大小(amount值)
        //iTween.ScaleTo(obj, new Vector3(3, 3, 3), 5f);
        //ScaleUpdate(GameObject target, Vector3 amount, float time)随着时间改变物体的比例大小到我们提供的Scale大小(amount值)
        //iTween.ScaleUpdate(obj, new Vector3(3, 3, 3), 5f);
```

* 6. Punch：添加摇晃力

```C#
 //Punch：添加摇晃力
        //iTween.PunchPosition(GameObject target, Vector3 amount, float time) 对物体的位置添加摇晃动画,使其摇晃最终归于原来的位置.
        //iTween.PunchPosition(obj, new Vector3(3, 3, 3), 5f);
        //PunchRotatetion(GameObject target, Vector3 amount, float time)对物体的角度添加摇晃动画,使其摇晃最终归于原来的角度
        //iTween.PunchRotation(obj, new Vector3(3, 3, 3), 5f);
        //PunchScale(GameObject target, Vector3 amount, float time)对物体的大小添加摇晃动画,使其摇晃最终归于原来的大小.
        //iTween.PunchScale(obj, new Vector3(3, 3, 3), 5f);
        //PutOnPath(GameObject target, Vector3[] path, float percent)根据提供的百分比将游戏物体置于所提供路径上。（1为百分之百）
        Vector3[] v3s = {new Vector3(0,0,0), new Vector3(0.8f,0.8f,0.8f), new Vector3(0.4f,0.4f,0.4f)};
        //iTween.PutOnPath(obj, v3s, 5f);
        //iTween.PointOnPath(Vector3[] path, float percent);根据提供的百分比返回一条路径上的Vector3的位置
```

* 7. Shake：摆动对象

```C#
//Shake:摆动对象
        //ShakePosition(GameObject target, Vector3 amount, float time)根据提供的amount衰减其值随机摇动游戏物体的位置，其晃动大小和方向由提供的
        //iTween.ShakePosition(obj, new Vector3(3, 0, 0), 5f);
        //ShakeRotation(GameObject target, Vector3 amount, float time)根据提供的amount衰减其值随机摆动旋转游戏物体的角度 。
        //iTween.ShakeRotation(obj, new Vector3(3, 0, 0), 5f);
        //ShakeScale(GameObject target, Vector3 amount, float time)根据提供的amount衰减其值随机摆动改变游戏物体的大小。
        //iTween.ShakeScale(obj, new Vector3(3, 0, 0), 5f);
```

* 8. CameraFade：摄像机的淡入淡出

```C#
//.CameraFade：摄像机的淡入淡出
        //CameraFadeAdd(Texture2D texture, int depth)创建一个对象可以模拟摄相机的淡入淡出
        Texture2D texture = iTween.CameraTexture(new Color(0, 0, 0));
        iTween.CameraFadeAdd(texture, 99);
```

### 两种音频方法：

* 1. Audio：音量和音调的变化

* 2. Stab ：播放AudioClip一次，不用手动加载AudioSource组件

### 一种颜色变化方法：

* 1. Color：变换颜色

### 一种值变化方法：

* 1. ValueTo：返回一个“from”和“to”之间的插值，用以改变属性

每个方法一般有两种重载方式：最小定制选项、完全定制项。

* Update类方法：提供每帧改变属性值的环境。在Update或FixedUpdate方法或类似于循环的环境中调用。

* 动画控制：Pause（暂停），Resume（恢复），Stop（停止并销毁iTween）

* 绘制方法：DrawLine(绘制线条)，DrawLineGizmos（绘制线条），DrawPath（绘制弯曲的路径）DrawPathGizmos（与DrawPath相同）

* 其他方法：Count（返回iTween的数量），PathLength（返回路径长度），PutOnPath（根据提供的百分比将物体放置于所提供路径上），PointOnPath（返回路径上指定百分比处的Vector3）

* iTweenPath类：用于在Scene试图中编辑路径。

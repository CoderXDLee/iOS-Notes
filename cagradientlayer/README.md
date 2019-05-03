# CAGradientLayer

## 故事背景

对于程序主视图 `view` 或者其它任意的 `UIView`，我们通常需要设置其背景颜色。

* 设置**纯色**背景
  * 通过 `backgroundColor` 属性来设置
* 设置**渐变色**背景:
  * 通过 `Core Graphics` 绘图框架来直接绘制填充
  * 通过 `CAGradientLayer` 来实现

## 基本介绍

* **`CAGradientLayer`** 是 `CALayer` 的子类，隶属于苹果的 **QuartzCore** 框架。可以理解为是一个渐变图层。如果使用的是透明的颜色，还可以做到透明渐变。
* `CAGradientLayer` 目前**只能实现线性渐变**，还不能实现放射性渐变。
* `CAGradientLayer` 的4个主要属性介绍:

  * **colors:**  颜色数组，定义了渐变层的各个颜色。
  * **locations\(可选\):**  决定每个渐变颜色的终止位置，这些值必须是递增的，数组的长度和 colors 的长度最好一致。
  * **startPoint\(可选\):**  渲染的起始位置，默认值是: \[0.5, 0\]（具体坐标参考下图）。
  * **endPoint\(可选\):**   渲染的终止位置，默认值是: \[0.5, 1\] （具体坐标参考下图）。

![&#x5355;&#x4F4D;&#x5750;&#x6807;&#x7CFB;](../.gitbook/assets/image%20%281%29.png)

* 官方代码:

```swift
/* The array of CGColorRef objects defining the color of each gradient
     * stop. Defaults to nil. Animatable. */
open var colors: [Any]?

/* An optional array of NSNumber objects defining the location of each
     * gradient stop as a value in the range [0,1]. The values must be
     * monotonically increasing. If a nil array is given, the stops are
     * assumed to spread uniformly across the [0,1] range. When rendered,
     * the colors are mapped to the output colorspace before being
     * interpolated. Defaults to nil. Animatable. */
open var locations: [NSNumber]?

/* The start and end points of the gradient when drawn into the layer's
 * coordinate space. The start point corresponds to the first gradient
 * stop, the end point to the last gradient stop. Both points are
 * defined in a unit coordinate space that is then mapped to the
 * layer's bounds rectangle when drawn. (I.e. [0,0] is the bottom-left
 * corner of the layer, [1,1] is the top-right corner.) The default values
 * are [.5,0] and [.5,1] respectively. Both are animatable. */
open var startPoint: CGPoint
open var endPoint: CGPoint
```


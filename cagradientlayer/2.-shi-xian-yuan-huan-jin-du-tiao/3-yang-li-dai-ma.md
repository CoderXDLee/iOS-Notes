# 3、样例代码

## （1）TemperatureMeter.swift \(进度条组件\)

```swift
import UIKit

class TemperatureMeter: UIView {

    // 进度条所在圆的直径
    let progressDiameter: CGFloat = 80
    
    // 进度条线条宽度
    let progressWidth: CGFloat = 4
    
    // 进度条轨道颜色
    let trackColor = UIColor.lightGray
    
    // 渐变进度条
    var progressLayer: CAShapeLayer!
    
    // 进度文本标签
    var label: UILabel!
    
    // 路径
    var path: UIBezierPath!
    
    override init(frame: CGRect) {
        super.init(frame: frame)
        makeLabel()
        makeProgressBarTrackLayer()
        makeProgressBar()
        makeGrandientLayers()
    }
    
    required init?(coder aDecoder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
}

extension TemperatureMeter {
    
    /// 创建文本标签
    func makeLabel() {
        // 创建进度文本标签
        label = UILabel(frame: CGRect(x: 0, y: 0, width: bounds.width, height: progressDiameter))
        label.textAlignment = .center
        label.text = "0℃"

        addSubview(label)
    }
    
    /// 绘制进度条(圆弧)轨道
    func makeProgressBarTrackLayer() {
        
        // 轨道以及上面进度条的路径（在组件内部水平居中）
        path = UIBezierPath(arcCenter: CGPoint(x: bounds.midX, y: 40),
                                radius: (progressDiameter - progressWidth) / 2,
                                startAngle: toRadians(degrees: -210),
                                endAngle: toRadians(degrees: 30),
                                clockwise: true)
        
        // 绘制进度条背景轨道
        let trackLayer = CAShapeLayer()
        trackLayer.frame = self.bounds
        trackLayer.fillColor = UIColor.clear.cgColor
        // 轨道颜色
        trackLayer.strokeColor = trackColor.cgColor
        // 轨道透明度
        trackLayer.opacity = 0.25
        // 轨道使用圆角线条
        trackLayer.lineCap = .round
        // 轨道线条宽度
        trackLayer.lineWidth = progressWidth
        // 设置轨道路径
        trackLayer.path = path.cgPath
        
        // 将轨道添加到视图层中
        self.layer.addSublayer(trackLayer)
    }
    
    /// 绘制进度条
    func makeProgressBar() {
        
        progressLayer = CAShapeLayer()
        progressLayer.frame = self.bounds
        progressLayer.fillColor = UIColor.clear.cgColor
        progressLayer.strokeColor = UIColor.black.cgColor
        progressLayer.lineCap = .round
        progressLayer.lineWidth = progressWidth
        progressLayer.path = path.cgPath
        
        // 进度条默认结束位置是0
        progressLayer.strokeEnd = 0
        // 将进度条添加到视图层中
        self.layer.addSublayer(progressLayer)
    }
    
    /// 绘制渐变背景
    func makeGrandientLayers() {
        
        // 1.绘制左侧的渐变层（从上往下是：由黄变蓝）
        let gradientLayer1 = CAGradientLayer()
        gradientLayer1.frame = CGRect(x:0, y:0, width:self.frame.width/2,
                                      height:progressDiameter/4 * 3 + progressWidth)
        gradientLayer1.colors = [UIColor.yellow.cgColor,
                                 UIColor.green.cgColor,
                                 UIColor.cyan.cgColor,
                                 UIColor.blue.cgColor]
        gradientLayer1.locations = [0.1, 0.4, 0.6, 1]
        
        // 2.绘制右侧的渐变层（从上往下是：由黄变红）
        let gradientLayer2 = CAGradientLayer()
        gradientLayer2.frame = CGRect(x:self.frame.width/2, y:0, width:self.frame.width/2,
                                      height:progressDiameter/4 * 3 + progressWidth)
        gradientLayer2.colors = [UIColor.yellow.cgColor,
                                 UIColor.red.cgColor]
        gradientLayer2.locations = [0.1, 0.8]
        
        // 用于存放左右两侧的渐变层，并统一添加到视图层中
        let gradientLayer = CALayer()
        gradientLayer.addSublayer(gradientLayer1)
        gradientLayer.addSublayer(gradientLayer2)
        self.layer.addSublayer(gradientLayer)
        
        // 将渐变层的遮罩设置为进度条
        gradientLayer.mask = progressLayer
    }
    
    // 把角度转换成弧度
    func toRadians(degrees: CGFloat) -> CGFloat {
        return .pi * (degrees) / 180.0
    }
}

extension TemperatureMeter {
    
    /// 设置进度（可以设置是否播放动画，以及动画时间）
    func setPercent(percent: CGFloat, animated: Bool = true) {
        // 改变进度条终点，并带有动画效果（如果需要的话）
        CATransaction.begin()
        CATransaction.setDisableActions(!animated)
        CATransaction.setAnimationTimingFunction(CAMediaTimingFunction(name: .easeInEaseOut))
        progressLayer.strokeEnd = percent / 100
        CATransaction.commit()
        
        // 设置文本标签
        label.text = "\(Int(percent))℃"
    }
}
```

### （2）使用样例

```swift
import UIKit

class ViewController: UIViewController {
    
    @IBOutlet weak var progressSlider: UISlider!
    // 温度计组件
    var meter: TemperatureMeter!

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        // 创建并添加温度计组件
        meter = TemperatureMeter(frame: CGRect(x: 100, y: 200, width: 200, height: 200))
        self.view.addSubview(meter)
    }
    
    // 滑块值改变响应
    @IBAction func sliderDidChange(_ sender: Any) {
        // 设置进度
        meter.setPercent(percent: CGFloat(progressSlider.value))
    }
}
```




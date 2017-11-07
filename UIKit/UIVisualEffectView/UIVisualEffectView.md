## UIVisualEffectView

### Reference
[](https://www.omnigroup.com/developer/how-to-make-text-in-a-uivisualeffectview-readable-on-any-background)
[](https://github.com/ide/UIVisualEffects)

### Overview
1. EffectView 实现了一些视觉效果（作用于，它被加入的视图层级的父视图，或是加在它的contentView的子视图）

### Attention
1. 避免EffectView的alpha小于1
2. EffectView不直接添加子视图，而是添加到它的contentView里
3. masks可以直接加在EffectView 或它的contentView上，不能加在EffectView的父view，会抛出异常
4.任何mask添加到EffectView上，不是实际执行mask，UIKit使用一份copy来实现mask效果到每一个子view上，如果需要改变大小，需要重新更改mask，然后设置在effectView上面
5.截屏，许多Effect实现，需要Window的支持，所以普通截屏可能不会包含effect，使用`UIWindow`或者`UIScreen`来实现截图。

##UIBlurEffect

### OverView
1. 实现高斯模糊
2. 加在EffectView的contentView的子视图不受这种效果影响


## UIVibrancyEffect

### OverView
1. 作为有`UIBlurEffect`效果的`UIVisualEffectView`的子View或者视图层级顶部，使得加在`UIVibractEffect`的`UIVisualEffectView`的contentView上的内容更加生动（一般为label或者image）
2. 有可能使得文本显示不清楚，可以参考Reference

### Sample

```
//1.被加入父view的视图层级里
    let lightBlurEffect = UIBlurEffect(style: .extraLight)
    let lightBlurEffectView = UIVisualEffectView(effect: lightBlurEffect)
    lightBlurEffectView.frame = CGRect(x: 0, y: 0, width: view.bounds.size.width, height: view.bounds.size.height /  2.0)
    view.addSubview(lightBlurEffectView)
//2.添加在EffectView的contentView里，不受`UIBlurEffect` 影响
    let redRectView = UIView(frame: CGRect(x: 20, y: 20, width: 20, height: 20))
    redRectView.backgroundColor = .red
    lightBlurEffectView.contentView.addSubview(redRectView)

    let vibrancyEffect = UIVibrancyEffect(blurEffect: lightBlurEffect)
    let vibrancyEffectView = UIVisualEffectView(effect: vibrancyEffect)
    vibrancyEffectView.frame = CGRect(x: view.bounds.size.width/2.0, y: 0, width: view.bounds.size.width/2.0, height: view.bounds.size.height/2.0)
    let label = UILabel(frame:CGRect(x: 0, y: 0, width: 100, height: 100))
    label.text = "Hello World"
    label.tintColor = .yellow   
vibrancyEffectView.contentView.addSubview(label)
    lightBlurEffectView.contentView.addSubview(vibrancyEffectView)

```

## 


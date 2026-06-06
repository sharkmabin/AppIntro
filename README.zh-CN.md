# AppIntro

Android 引导页库，帮助开发者为应用制作类似 Google 应用的炫酷引导页，支持多种布局样式、页面动画、权限请求和滑动策略。

## 功能概述

- **ViewPager 滑动引导**：基于 Fragment 的多页滑动引导，自动生成圆点指示器和操作按钮
- **两种布局风格**：
  - `AppIntro`：经典布局，底部栏含 Skip/Next/Done 按钮和圆点指示器
  - `AppIntro2`：替代布局，信息更紧凑
- **内置幻灯片**：`AppIntroFragment` / `AppIntro2Fragment` 快速构建标题+描述+图片的引导页
- **自定义幻灯片**：支持传入自定义布局和 Fragment
- **页面切换动画**：内置 Fade、Zoom、Flow、SlideOver、Depth 五种动画，支持自定义 PageTransformer
- **背景颜色过渡**：滑动时页面背景颜色平滑渐变
- **运行时权限请求**（Android 6.0+）：一行代码申请指定页面的运行时权限
- **滑动策略**：`ISlidePolicy` 接口控制是否允许用户滑动到下一页（如要求勾选协议）
- **外观定制**：自定义 Bar 颜色、指示器、Skip/Done 按钮文字与颜色、分隔线等
- **震动反馈**：滑动时提供触觉反馈

## 项目结构

| 目录/模块 | 说明 |
|------|------|
| `library/` | 引导页核心库 |
| `library/src/main/java/com/github/paolorotolo/appintro/` | 核心类（AppIntro、AppIntro2、AppIntroBase、AppIntroFragment 等） |
| `library/src/main/java/com/github/paolorotolo/appintro/util/` | 工具类（LogHelper、CustomFontCache） |
| `example/` | 示例应用，展示多种引导页用法 |
| `example/src/main/java/com/amqtech/opensource/appintroexample/ui/mainTabs/` | 示例：默认布局、自定义布局、自定义背景 |
| `example/src/main/java/com/amqtech/opensource/appintroexample/ui/permsTabs/` | 示例：权限请求引导 |
| `example/src/main/java/com/amqtech/opensource/appintroexample/util/` | 示例工具（SampleSlide 自定义幻灯片模板） |

### 核心类说明

| 类/接口 | 说明 |
|------|------|
| `AppIntroBase` | 抽象基类，封装 ViewPager、指示器、按钮等通用逻辑 |
| `AppIntro` | 经典布局引导页（继承 AppIntroBase） |
| `AppIntro2` | 替代布局引导页（继承 AppIntroBase） |
| `AppIntroFragment` | 经典布局的内置幻灯片 Fragment |
| `AppIntro2Fragment` | 替代布局的内置幻灯片 Fragment |
| `AppIntroBaseFragment` | 幻灯片 Fragment 基类 |
| `AppIntroViewPager` | 自定义 ViewPager，支持滑动锁定和动画时长控制 |
| `PagerAdapter` | ViewPager 适配器 |
| `DefaultIndicatorController` / `ProgressIndicatorController` | 圆点指示器 |
| `IndicatorController` | 指示器接口 |
| `ScrollerCustomDuration` | 控制 ViewPager 滑动速度 |
| `ISlideSelectionListener` | 页面切换监听 |
| `ISlidePolicy` | 滑动策略接口（控制是否允许翻页） |
| `ISlideBackgroundColorHolder` | 背景色过渡接口 |
| `PermissionObject` | 权限请求数据对象 |
| `FadePageTransformer` / `ViewPageTransformer` | 页面切换动画 |
| `CustomFontCache` | 自定义字体缓存 |

## 技术栈

- Java
- Android Support Library v4（ViewPager、Fragment）
- 发布至 Maven Central（`com.github.paolorotolo:appintro:4.1.0`）

## 使用 / 运行

### 添加依赖

```groovy
repositories {
    mavenCentral()
}

dependencies {
    compile 'com.github.paolorotolo:appintro:4.1.0'
}
```

### 基础用法

```java
public class IntroActivity extends AppIntro {
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        // 注意：不需要调用 setContentView()

        // 添加幻灯片 Fragment
        addSlide(firstFragment);
        addSlide(secondFragment);

        // 或使用内置幻灯片
        addSlide(AppIntroFragment.newInstance(title, description, image, backgroundColor));

        // 可选配置
        setBarColor(Color.parseColor("#3F51B5"));
        setSeparatorColor(Color.parseColor("#2196F3"));
        showSkipButton(false);
    }
}
```

### 使用替代布局

```java
public class IntroActivity extends AppIntro2 {
    // 与 AppIntro 用法一致，仅布局不同
}
```

### 请求运行时权限

```java
// 在第二个引导页请求相机权限
askForPermissions(new String[]{Manifest.permission.CAMERA}, 2);
```

### 滑动策略控制

```java
public class MySlide extends Fragment implements ISlidePolicy {
    @Override
    public boolean isPolicyRespected() {
        return checkBox.isChecked(); // 用户勾选后才允许翻页
    }

    @Override
    public void onUserIllegallyRequestedNextPage() {
        Toast.makeText(getContext(), "请先同意条款", Toast.LENGTH_SHORT).show();
    }
}
```

在 AndroidManifest.xml 中声明：

```xml
<activity android:name="com.example.example.intro"
    android:label="@string/app_intro" />
```

## 说明

AppIntro 是一个成熟的 Android 开源库，发布在 Maven Central，被数以百计的应用使用。库提供了两种布局样式、丰富的动画效果，支持页面切换时的背景色平滑过渡。配合 `ISlidePolicy` 可实现"同意条款才能继续"等交互，内置的权限请求功能简化了 Android 6.0+ 的运行时权限处理。

完整示例代码见 `example/` 模块，示例应用可在 [Google Play](https://play.google.com/store/apps/details?id=com.amqtech.opensource.appintroexample) 下载体验。

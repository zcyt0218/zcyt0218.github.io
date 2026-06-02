#### 漏洞描述

APK 中嵌入的 Samsung Mobile Ads SDK 暴露了内部开发设置页面：

`com.samsung.android.mas.internal.ui.DevSettingsPage`

该 Activity 设置为 `android:exported="true"`，且未配置权限。代码显示该页面包含测试模式开关、Mock Settings、测试服务器 URL 设置、清除广告配置和 Consent 值等功能。

#### 证据

- `store_audit/decoded/AndroidManifest.xml:1032`
- `store_audit/jadx/sources/com/samsung/android/mas/internal/ui/DevSettingsPage.java:26`
- `store_audit/jadx/sources/com/samsung/android/mas/internal/ui/DevSettingsPage.java:57`
- `store_audit/jadx/sources/com/samsung/android/mas/internal/ui/DevSettingsPage.java:587`
- `store_audit/jadx/sources/com/samsung/android/mas/utils/m.java:11`
- `store_audit/jadx/sources/com/samsung/android/mas/utils/m.java:27`
- `store_audit/jadx/sources/com/samsung/android/mas/utils/l.java:96`
- `store_audit/jadx/sources/com/samsung/android/mas/utils/l.java:147`

#### PoC

```bash
adb shell am start -n "com.sec.android.app.samsungapps/com.samsung.android.mas.internal.ui.DevSettingsPage"
```

#### 影响

本地攻击者可诱导用户打开内部广告 SDK 设置页面。根据运行时口令门禁与 build type，不同设备上可能造成测试模式启用、测试服务器切换、广告配置清除等影响。

#### 修复建议

生产包中应移除该 Activity，或设置：

```xml
<activity
    android:name="com.samsung.android.mas.internal.ui.DevSettingsPage"
    android:exported="false" />
```

如确需保留，应增加签名级权限并加入 build type / debug flag 校验。

---

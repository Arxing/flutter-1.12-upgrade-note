# Flutter 1.12 and Dart 2.7.0 升級筆記

### 遷移至AndroidX
1. `android/gradle.properties`加入`android.useAndroidX=true`
2. `android-gradle-tools`升級至`3.3.0`以上，`kotlin`升級至`1.3.0`以上
##### `android/build.gradle`
```gradle
buildScript {
  ext.kotlin_version = '1.3.0'
  dependencies {
    classpath 'com.android.tools.build:gradle:<3.3.0+>'
  } 
}
```
3. 相關依賴庫遷移至androidX，可以到[官方遷移對照表](https://developer.android.com/jetpack/androidx/migrate/artifact-mappings)查看並遷移依賴路徑
4. 過程中若遇到版本衝突問題，參考以下設定去覆寫版本
##### `android/app/build.gradle`
```gradle
project.configurations.all {
  resolutionStrategy.eachDependency { details ->
    def group = details.requested.group
    def id = details.requested.name
    if(group == 'androidx.core' && id == 'core') details.useVersion '1.1.0'
  }
}
```

### Flutter升級
1. 在[官方Releases](https://flutter.dev/docs/development/tools/sdk/releases?tab=windows)中找到要升遷的版本，例如`v1.12.13+hotfix.5`
2. 下指令`flutter version <升遷版本>`並升級flutter版本

### `intl`衝突問題
1. 由於`flutter v1.9.1+hotfix.6`依賴`intl 0.15.8`，而`flutter v1.12.13+hotfix.5`依賴`intl 0.16.0`，將所有`intl`的依賴改為`0.16.0`或是覆寫
```yaml
dependency_overrides:
  intl: 0.16.0
```



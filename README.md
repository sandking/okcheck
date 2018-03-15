# Okcheck

差量扫描，自动集成Lint、KtLint、Checkstyle、Findbugs、Pmd 5大互补静态扫描工具，灵活配置

## 如何引入

在根项目的`build.gradle`中配置:

```groovy
buildscript {
    dependencies {
        classpath 'com.liulishuo.okcheck:gradle:0.0.7'
    }
}

allprojects {
    apply plugin: 'okcheck'
}
```

至此，就已经完全整合，并且采用我们定制的统一规则生效5大静态扫描工具，以及可以通过`./gradlew okcheck`差分静态扫描，并且默认所有报告会整合到根项目的`build/reports`目录下，方便统一导出。

## 如何配置

如下配置不引入KtLint与Checkstyle，并且配置让报告导出到每个module自己的reports目录下:

```groovy
allprojects {
    okcheck {
        // AndroidLint 会强制开启，所以目前没有提供开关
        enableCheckstyle = false
        // enableFindbugs = false
        // enablePmd = false
        enableKtlint = false

	// 为了方便统一导出，因此默认的所有报告会在根项目的`build/reports/<modules>/`下面
	// 如果想要定制到每个独立module自己的reports下面，使用如下配置
	destination = buildDir
    }
}
```

配置让编译继续，如果findbugs扫描出存在warnnings的错误(其他的同理):
```
subprojects {
    findbugs {
        ignoreFailures = true
    }
}
```

## 其他

- 在`apply`了`okcheck`以后，如若没有对源码与资源扫描的任务(`lint`,`ktlint`,`checkstyle`,`pmd`)进行关闭，默认也会将这几个任务绑定到`check`任务中。

#### 已经忽略包

在`checkstyle`,`findbugs`,`pmd`中忽略了以下路径的扫描:

```
**/gen/**
**/test/**
**/proto/*.java
**/protobuf/*.java
**/com/google/**/*.java
```

## LICENSE

```
Copyright (c) 2018 LingoChamp Inc.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
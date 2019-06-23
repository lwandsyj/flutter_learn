#1.flutter doctor 检测

#2.flutter create appName
 
 创建app 

 appName 必须全部是小写

 #3.flutter run  
   
   运行项目

#4.热更新

在vscode 中，debug 模式可以保证我们创建了项目，保存直接更新

在命令行下点击r 键可以重新加载

#5.启动遇到的问题

Error runing Gradle 错误解决（1.x版本已经修复）
在Debug项目的时候，应该最常见的错误就是类似下面这样的错误了。

    Launching lib/main.dart on Android SDK built for x86 in debug mode...
    Initializing gradle...
    Resolving dependencies...
    * Error running Gradle:
    ProcessException: Process "/Users/rabbit/develop/android/flutter_app/android/gradlew" exited abnormally:
    Project evaluation failed including an error in afterEvaluate {}. Run with --stacktrace for details of the afterEvaluate {} error.

    FAILURE: Build failed with an exception.

    * Where:
    Build file '/Users/rabbit/develop/android/flutter_app/android/app/build.gradle' line: 25

    * What went wrong:
    A problem occurred evaluating project ':app'.
    > Could not resolve all files for configuration 'classpath'.
      > Could not find lint-gradle-api.jar (com.android.tools.lint:lint-gradle-api:26.1.2).
        Searched in the following locations:
            https://jcenter.bintray.com/com/android/tools/lint/lint-gradle-api/26.1.2/lint-gradle-api-26.1.2.jar

    * Try:
    Run with --stacktrace option to get the stack trace. Run with --info or --debug option to get more log output. Run with --scan to get full insights.

    * Get more help at https://help.gradle.org

    BUILD FAILED in 0s
      Command: /Users/rabbit/develop/android/flutter_app/android/gradlew app:properties

    Finished with error: Please review your Gradle project setup in the android/ folder.
这个问题的产生的原因，还是中国特有的问题，解决方案是改位阿里的链接(1.0已经修复了这个问题，不用再重新设置了)。

第一步：修改掉项目下的android目录下的build.gradle文件，把google() 和 jcenter()这两行去掉。改为阿里的链接。

    maven { url 'https://maven.aliyun.com/repository/google' }
    maven { url 'https://maven.aliyun.com/repository/jcenter' }
    maven { url 'http://maven.aliyun.com/nexus/content/groups/public' }
全部代码：

    buildscript {
        repositories {
            //  google()
            //  jcenter()
            maven { url 'https://maven.aliyun.com/repository/google' }
            maven { url 'https://maven.aliyun.com/repository/jcenter' }
            maven { url 'http://maven.aliyun.com/nexus/content/groups/public'}
            }
            dependencies {
            classpath 'com.android.tools.build:gradle:3.1.2'
        }
    }

    allprojects {
        repositories {
            // google()
            // jcenter()
            maven { url 'https://maven.aliyun.com/repository/google' }
            maven { url 'https://maven.aliyun.com/repository/jcenter' }
            maven { url 'http://maven.aliyun.com/nexus/content/groups/public' }
        }
    }

    rootProject.buildDir = '../build'
    subprojects {
        project.buildDir = "${rootProject.buildDir}/${project.name}"
    }
    subprojects {
        project.evaluationDependsOn(':app')
    }

    task clean(type: Delete) {
        delete rootProject.buildDir
    }

注意是有两个部分进行了修改，不要只修改一处。

第二步：修改Flutter SDK包下的flutter.gradle文件,这个目录要根据你的SDK存放的位置有所变化。比如我放在了D盘Flutter目录下，那路径就是这个。

    D:\Flutter\flutter\packages\flutter_tools\gradle
打开文件进行修改，修改代码如下（其实也是换成阿里的路径就可以了）。

    buildscript {
        repositories {
            //jcenter()
            // maven {
            //     url 'https://dl.google.com/dl/android/maven2'
            // }
            maven{
                url 'https://maven.aliyun.com/repository/jcenter'
            }
            maven{
                url 'http://maven.aliyun.com/nexus/content/groups/public'
            }
        }
        dependencies {
            classpath 'com.android.tools.build:gradle:3.1.2'
        }
    }

#6.代码路径中不要有中文
 
 比如：存放代码的路径为

   E:\\学习\\code\learn_code
   编译时会报错
    
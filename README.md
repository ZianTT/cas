Apereo CAS WAR Overlay 模板
=====================================

WAR Overlay 类型: `cas-overlay`

# 版本
   

- CAS Server `6.6.0-SNAPSHOT`
- JDK `11`
                     
# 构建

要构建项目，请使用:

```bash
# Use --refresh-dependencies to force-update SNAPSHOT versions
./gradlew[.bat] clean build
```

要查看哪些命令/任务可用于构建脚本，请运行:

```bash
./gradlew[.bat] tasks
```

如果你需要，在Linux/Unix系统上，你可以删除Gradle已经下载的所有工件(artifacts和metadata):

```bash
# Only do this when absolutely necessary
rm -rf $HOME/.gradle/caches/
```

同样的策略也适用于Windows，只要您在上面的命令中将`$HOME`切换到它的等效值。

# Keystore

要使服务器成功运行，您可能需要创建一个keystore文件。这可以通过使用JDK的`keytool`工具或通过以下命令来完成:

```bash
./gradlew[.bat] createKeystore
```

使用密钥存储库和密钥/证书条目的密码`changeit`。确保密钥存储库装载了服务器的密钥和证书。

## 扩展模块

扩展模块可以在[Gradle构建脚本](build.gradle)的`dependencies`块中指定

```gradle
dependencies {
    implementation "org.apereo.cas:cas-server-some-module"
    ...
}
```

收集覆盖中所有项目模块和依赖项的列表:

```bash
./gradlew[.bat] dependencies
```                                                                       

查看可配置和使用的所有项目依赖项的完整列表:

```bash
curl https://localhost:8080/dependencies
```     

或者:

```bash
curl https://localhost:8080/actuator/info
```

# 部署

通过以下方法成功部署后，服务器将在运行:


* `https://localhost:8443/cas`



  
## 可执行 WAR

将服务器web应用程序作为可执行的WAR运行。请注意，运行可执行的WAR需要CAS使用嵌入式容器，如Apache Tomcat、Jetty等。

当前servlet容器被指定为 `-tomcat`.

```bash
java -jar build/libs/cas.war
```

或通过:

```bash
./gradlew[.bat] run
```

将CAS web应用程序调试为可执行的WAR:

```bash
./gradlew[.bat] debug
```
       
或通过:

```bash
java -Xdebug -Xrunjdwp:transport=dt_socket,address=5000,server=y,suspend=y -jar build/libs/cas.war
```

运行CAS web应用程序作为一个*独立的*可执行的WAR:

```bash
./gradlew[.bat] clean executable
```

## External

成功构建servlet容器后，将二进制web应用程序文件部署到`build/libs`中。

# Docker

以下策略概述了如何构建和部署CAS Docker映像。

## Jib

覆盖包含了[Jib Gradle Plugin](https://github.com/GoogleContainerTools/jib)，以提供易于使用的开箱即用的工具，用于构建CAS docker图像。Jib是一个来自谷歌的开源Java容器器，它允许Java开发人员使用他们知道的工具构建容器。它是一个容器映像构建器，处理将应用程序打包为容器映像的所有步骤。它不需要你编写Dockerfile或安装Docker，它直接集成到覆盖中。

```bash
# Running this task requires that you have Docker installed and running.
./gradlew build jibDockerBuild
```

## Dockerfile

你也可以使用本地的Docker工具和提供的`Dockerfile`来构建和运行。

```bash
chmod +x *.sh
./docker-build.sh
./docker-run.sh
```

为方便起见，额外的`docker-compose.yml`也可被提供来协调构建:

```bash  
docker-compose build
```


# CAS 命令行

要启动到CAS命令行:

```bash
./gradlew[.bat] downloadShell runShell
```

# Retrieve Overlay Resources

To fetch and overlay a CAS resource or view, use:

```bash
./gradlew[.bat] getResource -PresourceName=[resource-name]
```

# Create User Interface Themes Structure

You can use the overlay to construct the correct directory structure for custom user interface themes:

```bash
./gradlew[.bat] createTheme -Ptheme=redbeard
```

The generated directory structure should match the following:

```
├── redbeard.properties
├── static
│ └── themes
│     └── redbeard
│         ├── css
│         │ └── cas.css
│         └── js
│             └── cas.js
└── templates
    └── redbeard
        └── fragments
```

HTML templates and fragments can be moved into the above directory structure, 
and the theme may be assigned to applications for use.

# List Overlay Resources
 
To list all available CAS views and templates:

```bash
./gradlew[.bat] listTemplateViews
```

To unzip and explode the CAS web application file and the internal resources jar:

```bash
./gradlew[.bat] explodeWar
```

# Configuration

- The `etc` directory contains the configuration files and directories that need to be copied to `/etc/cas/config`.

```bash
./gradlew[.bat] copyCasConfiguration
```

- 构建的细节是使用 `gradle.properties` 文件控制的.

## 配置metadata

配置metadata允许您将CAS属性集合作为报告导出到一个文件中，以便稍后进行检查。你会发现一个完整的CAS设置列表，以及注释、类型、默认值和接受值:

```bash
./gradlew exportConfigMetadata
```                           

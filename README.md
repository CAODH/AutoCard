<h1 style="text-align: center">浙江大学自动健康打卡AutoCard</h1>
<div style="text-align: center">

![AUR](https://img.shields.io/badge/license-Apache%20License%202.0-blue.svg)
![star](https://gitee.com/GCSZHN/AutoCard/badge/star.svg?theme=white)
![GitHub stars](https://img.shields.io/github/stars/GCS-ZHN/AutoCard.svg?style=social&label=Stars)
![GitHub forks](https://img.shields.io/github/forks/GCS-ZHN/AutoCard.svg?style=social&label=Fork)

</div>
本项目为解决浙江大学每日重复的健康打卡而开发，在完成首次手动打卡后，可以自动进行定时打卡。并通过邮件形式提醒通知打卡结果。

## 依赖
本项目使用spring-boot、quartz和httpclient开发，使用maven进行项目管理，编译版本为jdk-14.0.2。若用户需要使用低版本如jdk 1.8，需要在对应版本（安装对应版本jdk并修改pom.xml中版本信息）下重新编译打包maven项目。

        bash build.sh         ## linux
        powershell build.ps1  ## windows

## 基本运行
- 执行`java -jar autocard-1.0.jar`。在工作目录下要有`config/`。注意工作目录是输入命令时的shell终端目录。

`config/application.properties`

该文件用于配置用户账户等

        zjupassport.username=学工号
        zjupassport.password=密码
        mail.auth.username=邮箱
        mail.auth.password=邮箱密码

邮箱用于打卡的通知，默认需要浙大邮箱，否则需要`mail.smtp.host`和`mail.smtp.port`参数配置为指定第三方邮箱如QQ邮箱的配置。若不配置邮箱信息，将不会邮件提醒。

`config/log4j2.xml`

该文件是日志配置文件，可以不用修改。

- 此外，可以在上述命令中使用`--zjupassport.name=学工号`与`--zjupassport.password=密码`启动。该参数会覆盖配置文件。

        java -jar --zjupassport.name=XXXXX --zjupassport.password=XXXXX

- 鉴于个人PC不会长期保持运行，建议将程序放在VPS等服务器上运行。

## shell脚本
在shell环境（linux终端），可以用`startup.sh`和`shutdown.sh`启动或关闭程序，采用nohup形式后台运行。注意shell脚本要与jar包和config放在一个目录下。

        bash startup.sh --zjupassport.name=XXXX --zjupassport.password=XXXX "'--app.autoCard.cronExpression=0 0 6 * * ? *'"
        bash shutdown.sh

值得注意的是，cron表达式参数由于是空格分开，需要两重引号，因此建议通过配置文件配置参数。

## 额外参数
- --app.autoCard.cronExpresssion

cron表达式是用于定时任务的经典表达式，该参数允许用户自定义打卡定时方式。网上有很多现成的表达式模板以及表达式在线生成工具。

- --app.zjuClient.cookieCached

该参数默认为false，设置为true则会启动cookie缓存。

- --mail.smtp.host

该参数修改了SMTP服务器主机域名，默认采用浙大服务器smtp.zju.edu.cn。在需要使用QQ邮箱等其他邮箱是需要配套修改设定

- --mail.smtp.port
  
该参数为SMTP服务端口，默认为994，具体看SMTP邮件服务提供商。

## 注意
若打卡题目被更新，请先手动打卡一次。本项目仅供学习参考。使用时请确保信息的正确性。滥用造成的后果请自行承担。

## 反馈
任何使用问题，欢迎通过邮箱**zhang.h.n@foxmail.com**交流。

### Git
1. 代码冲突怎么处理
   1. 多个分支向主分支合并时
   2. 同一个分支下pull或push操作时
发生了冲突需要手动合并代码，选择最终的版本，可以通过图形界面

2. 在哪个分支进行开发：
   1.一般是在开发分支开发


### Maven
1. 常用命令
   1. mvn  clean 清除target目录中生成的结果
   2. mvn compile 编译源代码
   3. mvn test 执行测试单元
   4. mvn package 打包
   5. mvn install 打包并把打好的包保存到本地仓库
   6. mvn deploy 打包并把打好的包上传到远程仓库
2. maven依赖版本冲突
   1. 使用exclusion排除冲突
   2. 使用dependencyManagement锁定版本号
   通常在父工程对依赖的版本统一管理



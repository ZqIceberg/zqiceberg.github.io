# macOS环境下的操作

### macOS下使用vscode

macOS下不能使用DEV C++，如果用vim，学习成本比较高（虽然学习vim是非常好的）

我们可以选择vscode，安装C++插件，就可以编译运行.cpp文件



非常建议，禁止使用自动补全符号，自动补全代码等IDE自带功能



### CodeBlocks汉化方法

1. 找到CodeBlocks.app，右键，选择“显示包内容”
2. 进入Contents > Resources > share > codeblocks > 
3. 在这个文件夹下，新创建一个文件夹，“locale”
4. 在新创建的locale文件下，再新创建一个“zh_CN”文件夹
5. 将codeblocks.mo文件拷贝到下面
6. 回到CodeBlocks界面，进入“Setting”- "Enviroment";点击左边的“View”, 将右边的  "internationalization"选项的复选框打上勾，再选择“Chinese simplify”, 保存，重启codeblock，再次启动就是汉化的了

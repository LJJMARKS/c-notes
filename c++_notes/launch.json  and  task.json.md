#### launch.json		and		task.json

##### **launch.json参数介绍**—对应调试相关的参数

```c++
{
    "configurations": [     //配置域
        {
            "name": "(gdb) 启动",       //配置文件的名字
            "type": "cppdbg",       //调试的类型，正在使用的调试器,使用Visual Studio Windows时必须为cppvsdbg,使用GDB或LLDB时必须为cppdbg.
            "request": "launch",     //配置文件的请求类型，有launch和attach两种，表示此配置是用于启动程序还是附加到已运行的实例上
            "targetArchitecture": "x64",   //硬件内核架构，为64bit
            // "program": "输入程序名称，例如 ${workspaceFolder}/a.out", //这个是默认生成的样式
            // "program": "${workspaceRoot}/${fileBasenameNoExtension}", //${workspaceRoot}指的是.vscode所在的目录(绝对路径)，${fileBasenameNoExtension}指的是所运行文件不带扩展名的名称，即main.cpp是main
            "program": "${fileDirname}/${fileBasenameNoExtension}", //调试可执行文件/程序，此处为可执行文件的路径，包括文件名字，即要执行的可执行文件的完整路径，${fileDirname}指的是所运行文件所在的目录(绝对路径)，这里和.vscode在同一目录(绝对路径)下
            "args": [],     //主函数调用时传入的参数，一般为空
            "stopAtEntry": false,      //设为true时程序将暂停在程序入口处，一般设为false，是否在 main 函数处暂停执行
            "cwd": "${fileDirname}",     //调试时的工作目录，设置调试器启动的应用程序的工作目录，不是可执行程序的路径，而是所运行程序的路径，或者.vscode所在的目录(绝对路径)，且此处的cwd路径与task.json的cwd路径不一样
            // "cwd": "${workspaceFolder}",  //.vscode所在的目录(绝对路径)，与${fileDirname}都可以，且此处的cwd路径与task.json的cwd路径不一样
            "environment": [],   
            "externalConsole": false, //控制是否显示在新的终端，即打开外部终端执行程序,而不是在 VSCode 的控制台中运行，true显示外置的控制台窗口，false显示内置终端
            // "externalConsole": true,
            "MIMode": "gdb",      //指定连接的调试器，可以省略不写
            //"miDebuggerPath": "C:\\MinGW\\bin\\gdb.exe",//调试器路径,在Linux环境下需要注释掉这一行
            "setupCommands": [
                {
                    "description": "为 gdb 启用整齐打印",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                },
                {
                    "description": "将反汇编风格设置为 Intel",
                    "text": "-gdb-set disassembly-flavor intel",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "g++",       //调试会话开始前执行的任务，一般为编译程序。与tasks.json的label相对应,一般为编译程序，c++为g++, c为gcc,采用cmake的多文件编译则为build
        }
    ],
    "version": "2.0.0"       //配置文件的版本，以前使用是0.2.0，新版本已经弃用，改用为2.0.0
}
```

##### **task.json参数介绍**—编译参数设置

```c++
	{
	    "tasks": [
	        {
	            "type": "cppbuild",
	            // "label": "C/C++: g++ 生成活动文件",
	            "label": "g++",     // 当前编译任务的名称，与launch.json的preLaunchTask相对应
	            "command": "/usr/bin/g++",     // cpp使用g++命令，c使用gcc，指定对应的路径，编译时执行的程序，调试ros时使用catkin_make DCMAKE_BUILD_TYPE=Debug，此时对应的命令：g++ -g ${file} -o ${fileDirname}/${fileBasenameNoExtension}
	            "args": [    //编译时候的参数，传递给 command 的参数,也就是编译参数
	                "-fdiagnostics-color=always",
	                "-g",   //添加gdb调试选项
	                "${file}", //当前运行的文件
	                "-o",     //指定生成可执行文件的名称
	                "${fileDirname}/${fileBasenameNoExtension}", //生成可执行文件的路径(包括文件名字，此处没有带后缀.out)
	                // "-std=c++11",   //使用c++11
	                // "-lpthread"       //链接thread库
	            ],       
	            "options": {
	                "cwd": "${fileDirname}"   //需要进入到执行tasks任务的文件夹中，即可执行文件所在的目录(绝对路径)，不包括文件名
	            },
	            //${fileDirname} 当前打开的文件所在的绝对路径，不包括文件名

	            "problemMatcher": [
	                "$gcc"
	            ],
	            "group": {
	                "kind": "build",
	                "isDefault": true     //为 True 时，用户可以通过 Ctrl+Shift+B 直接运行编译任务
	            },
	            "detail": "调试器生成的任务。"
	        }
	    ],
	    "version": "2.0.0"
}
```

##### 常用路径参数含义

```c++
	${workspaceRoot} 当前打开的文件夹的绝对路径+文件夹的名字

	&{workspaceFolder}当前程序的路径，.vscode路径

	${file}当前打开正在编辑的文件名，包括绝对路径，文件名，文件后缀名
	
	${fileBasename}  当前打开的文件名+后缀名，不包括路径
	
	${fileBasenameNoExtension} 当前打开的文件的文件名，不包括路径和后缀名
	
	${fileDirname} 当前打开的文件所在的绝对路径，不包括文件名
	
	${fileExtname} 当前打开的文件的后缀名
```


# nginx-1.0.15-reading

# vscode调试nginx

- WSL2 安装
- 编译环境
```bash
sudo apt update
sudo apt install build-essential
```
- VS Code 安装 `Remote-SSH` 插件、`C/C++` 拓展
- VS Code 调试配置，编辑 `.vscode/launch.json`
```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(gdb) Launch",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/objs/nginx",
            "stopAtEntry": false,
            // 调试 nginx 程序，我们一般要关闭守护进程和 Master 进程，所以需要修改 args 参数，或者配置 nginx.conf
            "args": [
                "-g",
                "master_process off;daemon off;"
            ],
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                },
                {
                    "description": "Set Disassembly Flavor to Intel",
                    "text": "-gdb-set disassembly-flavor intel",
                    "ignoreFailures": true
                }
            ],
            "miDebuggerPath": "/usr/bin/gdb"
        }
    ]
}

```
- nginx 编译参数，`auto/cc/gcc` 增加一行
```bash
CFLAGS="$CFLAGS -g -O0 -gstabs"
```

- 自动编译(可选)，打开 `.vscode/tasks.json`，添加
```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "build",
            "type": "shell",
            "command": "make",
            "args": [
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            },
            "problemMatcher": "$gcc"
        }
    ]
}
```
`.vscode/launch.json` 增加一行 
```json
{
    "preLaunchTask": "build"
}
```
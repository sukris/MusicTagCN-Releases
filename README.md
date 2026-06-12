# MusicTagCN

MusicTagCN 是一个音乐标签补全和音乐文件整理 CLI 工具。

本仓库只发布编译后的可执行文件和使用说明。源码维护在私有仓库中，不在这里公开。

## 下载

从 Releases 页面下载对应系统版本：

```text
https://github.com/sukris/MusicTagCN-Releases/releases
```

当前发布产物包括：

```text
musictagcn-linux-x64
musictagcn-macos-arm64
musictagcn-macos-x64
musictagcn-windows-x64.exe
```

每个文件都有对应的 `.sha256` 校验文件。

## macOS 使用

下载后先加执行权限：

```bash
chmod +x ./musictagcn-macos-arm64
```

或 Intel Mac 使用：

```bash
chmod +x ./musictagcn-macos-x64
```

如果 macOS 提示来自未知开发者，可以先移除隔离属性：

```bash
xattr -d com.apple.quarantine ./musictagcn-macos-arm64
```

然后查看帮助：

```bash
./musictagcn-macos-arm64 --help
```

## Linux 使用

```bash
chmod +x ./musictagcn-linux-x64
./musictagcn-linux-x64 --help
```

## Windows 使用

在 PowerShell 中运行：

```powershell
.\musictagcn-windows-x64.exe --help
```

## 基本命令

所有命令都可以通过 `--dir` 指定音乐目录；不指定时默认使用当前目录。

```bash
./musictagcn --dir "/path/to/music" scan
```

### 扫描音乐文件

统计目录中的音频文件：

```bash
./musictagcn --dir "/path/to/music" scan
```

输出文件列表：

```bash
./musictagcn --dir "/path/to/music" scan --tree
```

检查标签是否可读：

```bash
./musictagcn --dir "/path/to/music" scan --check
```

建立处理清单：

```bash
./musictagcn --dir "/path/to/music" scan --manifest
```

### 查看状态

查看清单处理状态：

```bash
./musictagcn --dir "/path/to/music" status
```

### 检查标签内容

输出文件标签状态 JSON：

```bash
./musictagcn --dir "/path/to/music" inspect
```

只检查前 20 个文件：

```bash
./musictagcn --dir "/path/to/music" inspect --max 20
```

### 补全标签

默认只处理需要补全的文件：

```bash
./musictagcn --dir "/path/to/music" scrape
```

指定数据源顺序：

```bash
./musictagcn --dir "/path/to/music" scrape --sources qmusic,netease,kugou,kuwo,migu
```

限制最多处理 50 个文件：

```bash
./musictagcn --dir "/path/to/music" scrape --max 50
```

处理失败项：

```bash
./musictagcn --dir "/path/to/music" scrape --scope failed
```

按艺人筛选处理：

```bash
./musictagcn --dir "/path/to/music" scrape --artist "周杰伦"
```

重新处理已成功文件：

```bash
./musictagcn --dir "/path/to/music" scrape --force
```

输出报告到指定位置：

```bash
./musictagcn --dir "/path/to/music" scrape -o scrape-report.json
```

### 整理文件

默认是 dry-run，只显示将要移动的结果，不真正移动文件：

```bash
./musictagcn --dir "/path/to/music" organize --target "/path/to/organized"
```

确认执行移动：

```bash
./musictagcn --dir "/path/to/music" organize --target "/path/to/organized" --confirm
```

按艺人筛选整理：

```bash
./musictagcn --dir "/path/to/music" organize --target "/path/to/organized" --artist "周杰伦"
```

以 JSON 输出整理报告：

```bash
./musictagcn --dir "/path/to/music" organize --target "/path/to/organized" --json
```

## 推荐流程

```bash
./musictagcn --dir "/path/to/music" scan --manifest
./musictagcn --dir "/path/to/music" inspect --max 20
./musictagcn --dir "/path/to/music" scrape --max 50
./musictagcn --dir "/path/to/music" status
./musictagcn --dir "/path/to/music" organize --target "/path/to/organized"
```

确认整理结果没有问题后，再执行：

```bash
./musictagcn --dir "/path/to/music" organize --target "/path/to/organized" --confirm
```

## 校验下载文件

Linux/macOS：

```bash
shasum -a 256 musictagcn-linux-x64
cat musictagcn-linux-x64.sha256
```

Windows PowerShell：

```powershell
Get-FileHash .\musictagcn-windows-x64.exe -Algorithm SHA256
Get-Content .\musictagcn-windows-x64.exe.sha256
```

## 注意事项

- 建议先使用 `organize` 的默认 dry-run 模式确认整理结果，再加 `--confirm` 真正移动文件。
- `scrape` 会访问在线音乐数据源，网络环境会影响成功率和速度。
- 转码相关功能依赖 `ffmpeg`，需要系统中已安装 `ffmpeg` 并能在 `PATH` 中找到。
- 使用前建议先备份重要音乐文件。

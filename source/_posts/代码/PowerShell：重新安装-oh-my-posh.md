---
title: PowerShell：重新安装 oh-my-posh
tags:
  - 代码
  - Powershell
date: 2024-12-18 00:00:00
---

## 起因

最近打开 PowerShell 时，看到这样一条讯息：

[![pAL4IVx.png](https://s21.ax1x.com/2024/12/18/pAL4IVx.png)](https://imgse.com/i/pAL4IVx)

主要是说， oh-my-posh 的 PowerShell module 版本已经停止支持，并已移除了相关功能。

现在在 PowerShell 上需要使用init启动oh-my-posh。

_详情参见[PowerShell module | Oh My Posh](https://ohmyposh.dev/docs/migrating)_

## 卸载

### 删除模块的缓存文件

```
Remove-Item $env:POSH_PATH -Force -Recurse
```

### 卸载 PowerShell 模块

```
Uninstall-Module oh-my-posh -AllVersions
```

### 删除 PowerShell 模块的 `$PROFILE`

打开 `$PROFILE` 文件

```
open $Profile
```

在该文件中删去如下命令

```
Import-Module oh-my-posh
```

如果继续使用该命令，启动时将会得到开篇的提示，且启动失败。

## 安装

推荐 winget 和 Windows 应用商店，安装后会自动添加环境变量。

这里使用 winget 。

### 安装 oh-my-posh.exe

打开 PowerShell 并运行以下命令：

```
winget install JanDeDobbeleer.OhMyPosh -s winget
```

这个命令会安装内容如下：

*   `oh-my-posh.exe`Windows 可执行文件
*   `themes`最新的 Oh My Posh [主题](https://ohmyposh.dev/docs/themes)

建议运行如下命令：

```
$env:Path += ";C:\Users\user\AppData\Local\Programs\oh-my-posh\bin"
```

这会手动将其添加到环境变量中

## 安装 Nerd 字体

Nerd 字体提供了大量的图标，如果不使用 Nerd 字体的话，主题可能无法正常显示。

这里介绍两种安装方式：

### 用 oh-my-posh 安装

_注：非管理员模式将安装在用户目录下，管理员模式将会安装在系统目录下。_

你可以直接使用 oh-my-posh 中如下命令安装字体

```
oh-my-posh font install
```

这将显示可安装的字体列表，比如我想要安装字体`meslo`

```
oh-my-posh font install meslo
```

### 直接下载字体

访问 [Nerd官网](https://www.nerdfonts.com/font-downloads) ，安装你喜欢的字体，大部分都可预览。

## 修改配置文件

### 只是启用 oh-my-posh

打开 `$PROFILE` 文件

```
open $Profile
```

在该文件中加入

```
# 只是启用 oh-my-poshoh-my-posh init pwsh | Invoke-Expression
```

### 选择主题

将刚才的配置稍作修改

```
# 设置 PowerShell 主题oh-my-posh init pwsh --config "$env:POSH_THEMES_PATH\atomic.omp.json" | Invoke-Expression
```

其中 “$env:POSH\_THEMES\_PATH\\atomic.omp.json“ 是对官方主题 atomic.omp.json 的调用

你可以使用如下命令来查看所有的官方主题

```
Get-PoshThemes
```

将你喜欢的主题名称，替换掉这里的 atomic 即可。

使用如下命令使配置文件立即生效

```
. $PROFILE
```

## 白名单

官方提到：_“由于 Oh My Posh 的频繁更新，防病毒软件偶尔会对其进行标记（误报）。 为确保 Oh My Posh 不被阻止，您可以将其报告给您最喜欢的防病毒软件作为误报或为其创建排除项。”_

​因此，我们最好把 oh-my-posh 添加到杀毒软件的白名单。

​由于我只使用Windows10的Windows Defender，故只介绍这一个。

### 确定位置

首先要获取 oh-my-posh.exe 的详细位置

执行如下命令：

```
(Get-Command oh-my-posh).Source
```

### 加入白名单

打开windows 安全中心 → 病毒和威胁防护 → “病毒和威胁防护”设置(管理设置) → 排除项

然后添加该文件即可
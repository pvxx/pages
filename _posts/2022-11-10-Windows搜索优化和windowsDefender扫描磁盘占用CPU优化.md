---
title: windows搜索优化 ，Windows Defender占用大cpu内存优化
---

### 1、Windows 搜索优化

#### 1.1 禁用Windows搜索服务

```
services.msc
进入Windows Search，启动类型：禁用
```

#### 1.2 删除Windows.emb文件

```
借助PCmaster删除
```

### 2、Windows Defender 占用过大优化

```
病毒威胁防护---->管理设置---->排除项---->添加排除项---->选择不想扫描的文件夹即可
```


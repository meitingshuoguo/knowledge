## tree 参数详解

```
# 彩色显示 1 层目录
$ tree -C -L 1
```

### 常用参数

- `-a` 显示所有文件和目录 (默认显示所有的文件和目录)
- `-L n` 只显示 n 层目录 (n 为数字)
- `-d` 只显示目录名称
- `-f` 显示完整的相对路径以及名称
- `-s` 显示文件或目录大小。
- `-r` 以相反次序排列
- `-t` 用文件和目录的更改时间排序
- `-C` 文件和目录加上色彩，便于区分各种类型。
- `-dirsfirst` 目录显示在前,文件显示在后

### 更多参数

- `-A` 使用 `ASNI` 绘图字符显示树状图而非以 `ASCII` 字符组合
- `-D` 列出文件或目录的更改时间。
- `-g` 列出文件或目录的所属群组名称，没有对应的名称时，则显示群组识别码。
- `-N` 直接列出文件和目录名称，包括控制字符。
- `-l` 如遇到性质为符号连接的目录，直接列出该连接所指向的原始目录。
- `-u` 列出文件或目录的拥有者名称，没有对应的名称时，则显示用户识别码。
- `-F` 在执行文件，目录，Socket，符号连接，管道名称名称，各自加上 “\*“, “/“, “=”, “@”, “|”号。
- `-i` 不以阶梯状列出文件或目录名称。
- `-I` 不显示符合范本样式的文件或目录名称。
- `-n` 不在文件和目录清单加上色彩。
- `-p` 列出权限标示。
- `-P` 只显示符合范本样式的文件或目录名称。
- `-q` 用”?”号取代控制字符，列出文件和目录名称。
- `-x` 将范围局限在现行的文件系统中，若指定目录下的某些子目录，其存放于另一个文件系统上，则将该子目录予以排除在寻找范围外。

[来源](https://www.dazhuanlan.com/2020/02/02/5e36e2593ec7c/?__cf_chl_jschl_tk__=9efb0953b4022c3e2072eb7852da1b1a447f04ea-1602576350-0-AXhuWEPgDV_jBUs9EVEIJxQBcvcvN9QUeiIA7_OFLkc9cQwdPKfPne8rwoQ3Hv22wWune97GXgisVFGBvpoge2emaD_PKW46Uu3b_o7oH2eYHUFjl47ffHk-iWENDV2_vXr7xfEwz-bX9K_Evw3PE2L8_aIb9LP1yh4V6CQcOh6_Fx2msiGDtrxAmMTPABBDWJZBjw58tAwqdltn9ojzU2d63gmlAUFSUXG_PCWrCUzTe-gzxVaAKcMLiF3WHAgMvlSAlIuwXBfiIP_svkkfEG9YjOvFusv4wpMYoOqPFt4iItkFDTBvUKbY4WirecWozw)

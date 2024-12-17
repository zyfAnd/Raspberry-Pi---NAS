# 树莓派4 家庭 NAS 搭建指南

## 硬件需求
- 树莓派4
- 512GB 外接固态硬盘(SSD)
- 电源适配器（建议使用官方推荐的5V/3A电源）
- 网线（建议使用千兆网线）
- Micro SD卡（建议16GB以上）

## 软件准备
1. 下载最新版本的 Raspberry Pi OS（推荐使用64位版本）
2. 下载 Raspberry Pi Imager 烧录工具

## 基础系统安装步骤

### 1. 系统安装
1. 使用 Raspberry Pi Imager 将系统烧录到 Micro SD 卡
2. 插入 SD 卡并启动树莓派
3. 完成初始化设置（设置用户名、密码、时区等）

### 2. 系统更新

```bash
sudo apt update
sudo apt upgrade -y
```

### 3. 安装 OpenMediaVault (OMV)

1. 下载并运行 OMV 安装脚本
```bash
wget -O - https://github.com/OpenMediaVault-Plugin-Developers/installScript/raw/master/install | sudo bash
```

2. 安装完成后，通过浏览器访问 OMV
- 打开浏览器，输入：`http://树莓派IP地址`
- 默认登录凭据：
  - 用户名：admin
  - 密码：openmediavault

### 4. OMV 基本配置

1. 系统设置
- 修改默认密码
- 设置时区
- 配置网络

2. 存储设置
- 在 "存储器" -> "磁盘" 中查看并管理硬盘
- 在 "存储器" -> "文件系统" 中格式化并挂载硬盘
- 创建共享文件夹

3. 开启服务
- SMB/CIFS（Windows文件共享）
- NFS（Linux/Unix文件共享）
- FTP（可选）

### 5. 媒体服务器配置

1. 安装 Plex Media Server 插件
```bash
# 在 OMV 网页界面中
系统 -> 插件 -> 搜索 "Plex" -> 安装
```

2. Plex 配置
- 访问 `http://树莓派IP地址:32400/web`
- 登录 Plex 账户
- 添加媒体库：
  - 电影
  - 电视节目
  - 音乐
  - 照片

### 6. 其他实用插件推荐
- Docker (容器管理)
- Portainer (Docker GUI管理)
- Transmission (BT下载)
- NextCloud (私有云存储)

## 使用说明

### 文件共享访问
1. Windows
- 文件资源管理器地址栏输入：`\\树莓派IP地址`

2. macOS
- 访达菜单 -> 前往 -> 连接服务器
- 输入：`smb://树莓派IP地址`

3. 移动设备访问方式
- iOS 设备：
  1. 打开"文件"应用
  2. 点击"浏览" -> 点击"..."或"更多"
  3. 选择"连接服务器"
  4. 输入：`smb://树莓派IP地址`
  5. 输入在 OMV 中设置的用户名和密码

- Android 设备：
  1. 使用支持 SMB 的文件管理器（如 Solid Explorer、ES文件浏览器等）
  2. 添加新的存储位置或网络位置
  3. 选择 SMB/CIFS 服务器
  4. 输入树莓派IP地址
  5. 输入用户名和密码
  6. 连接后即可上传下载文件

### 文件夹结构建议
建议在 OMV 中创建以下共享文件夹：
```
/shared
  ├── Photos        # 照片存储
  ├── Videos        # 视频文件
  ├── Documents     # 文档文件
  ├── Music         # 音乐文件
  └── Backup        # 备份文件
```

### 权限设置
在 OMV 界面中：
1. 进入"访问权限管理" -> "共享文件夹"
2. 选择需要设置的文件夹
3. 点击"权限"
4. 确保对应用户具有读写权限

### 自动备份建议
- iOS：使用系统照片库的 iCloud 备份，或第三方应用如 PhotoSync
- Android：可以使用 FolderSync、Syncthing 等应用实现自动同步
- 建议开启自动备份功能，将手机照片/视频自动同步到 NAS

### 性能优化建议
- 使用 5GHz WiFi 或千兆网络连接以获得更快传输速度
- 大量文件传输时建议使用有线网络
- 可以通过 OMV 界面监控网络传输状态

### Plex 媒体访问
1. 本地网络
- 浏览器访问：`http://树莓派IP地址:32400/web`
- Plex App 自动发现服务器

2. 远程访问
- 在 Plex 设置中启用远程访问
- 通过 Plex App 或网页 `app.plex.tv` 访问

## 维护建议
1. 定期备份重要数据
2. ��过 OMV 界面监控系统状态：
   - CPU 使用率
   - 内存使用情况
   - 硬盘健康状态
3. 定期更新系统和插件

## 故障排除
1. OMV 网页无法访问
   - 检查网络连接
   - 确认 OMV 服务是否运行：`systemctl status openmediavault`

2. Plex 无法扫描媒体
   - 检查文件权限
   - 确认媒体文件格式是否支持
   - 查看 Plex 日志

3. 硬盘无法识别
   - 通过 OMV 界面检查硬盘状态
   - 查看系统日志
   - 检查电源供应

## 注意事项
- OMV 安装需要较长时间，请耐心等待
- 建议使用静态IP地址
- 媒体文件请按照 Plex 推荐的命名规范组织
- 定期备份 OMV 配置
- 确保供电稳定，建议使用 UPS

## 初始化设置

### 1. 首次启动配置
1. 使用 Raspberry Pi Imager 烧录系统时，可以预先配置：
   - 点击齿轮图标⚙️
   - 设置主机名（如：raspberrypi）
   - 启用 SSH
   - 设置用户名和密码
   - 配置 WiFi（如果使用无线连接）

2. 找到树莓派的 IP 地址方法：
   - 方法1：查看路由器管理页面的设备列表
   - 方法2：使用 IP 扫描工具（如 Advanced IP Scanner）
   - 方法3：将显示器接入树莓派，使用 `ip addr` 命令查看

### 2. 远程连接树莓派

1. Windows 用户：
   ```bash
   # 使用 SSH 连接（推荐使用 Windows Terminal 或 PuTTY）
   ssh 用户名@树莓派IP地址
   ```
   
   或使用 Windows 远程桌面：
   - 在树莓派上安装 xrdp：
     ```bash
     sudo apt install xrdp
     sudo systemctl enable xrdp
     ```
   - 在 Windows 中打开"远程桌面连接"
   - 输入树莓派的 IP 地址

2. Mac 用户：
   ```bash
   # 终端中使用 SSH 连接
   ssh 用户名@树莓派IP地址
   ```

### 3. 安装 OMV 前的准备
1. 确保系统已更新：
   ```bash
   sudo apt update
   sudo apt upgrade -y
   sudo reboot
   ```

2. 检查网络连接：
   ```bash
   # 测试网络连接
   ping -c 4 google.com
   ```

3. 检查硬盘连接：
   ```bash
   # 查看已连接的硬盘
   lsblk
   ```

### 4. OMV 安装后的访问
1. 通过网页浏览器访问 OMV：
   - 打开浏览器
   - 输入：`http://树莓派IP地址`
   - 如果无法访问，等待 5-10 分钟后再试（安装后系统需要一些时间初始化）

2. 登录 OMV：
   - 默认用户名：admin
   - 默认密码：openmediavault
   - 首次登录后立即修改密码

### 5. 基础网络设置
1. 建议设置静态 IP：
   - 在 OMV 界面中：系统 -> 网络
   - 选择网络接口
   - 配置静态 IP 地址
   - 设置正确的网关和 DNS

2. 如果使用 WiFi：
   ```bash
   # 编辑 WiFi 配置文件
   sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
   ```
   添加：
   ```
   network={
       ssid="你的WiFi名称"
       psk="WiFi密码"
   }
   ```

### 6. 故障排除提示
- 如果无法通过 IP 访问 OMV：
  ```bash
  # 检查 OMV 服务状态
  sudo systemctl status openmediavault
  
  # 如果服务未运行，重启服务
  sudo systemctl restart openmediavault
  ```

- 如果忘记 IP 地址：
  ```bash
  # 在树莓派终端中查看 IP
  ip addr show
  ```

- 如果网页界面加载缓慢：
  - 清除浏览器缓存
  - 使用其他浏览器尝试
  - 检查网络连接速度

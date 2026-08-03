
- 局域网控制、穿透控�?
- 想到可以远程控制工作站。很好实现，这样就可以呆在空调房，甚至离家工作了。太对了，那么使用场景拓宽了，很有必要购入了
- 虽然远程可能不方便、有延迟。但能省下一大笔空调钱和制冷电费，而且能不限制场景。太对了



### 方案一：Tailscale + 自带 RDP（最推荐�?
**优点**：安全（WireGuard 加密）、免费（个人使用够用）、性能最佳、无需端口映射
- 组网设备可相互控制。都设置一遍组�?远程控制+保障�?
1. 两台电脑都装 [Tailscale](https://tailscale.com/download)，注册同一账号
2. 自动组成虚拟局域网，Win10 上直接用 Win+R → `mstsc`，输�?Win11 �?Tailscale IP 即可

- 保持在线�?    - 关闭休眠
    - BIOS来电自启�?    - 开机自动启�?Tailscale
    - 网络不掉�?

## 平板端配�?
### iPad / iPhone

1. App Store 搜索 **"Windows App"**（微软官方应用，原名 Microsoft Remote Desktop�?    
2. 安装后打开，点击右上角 **+** �?**添加电脑**
    
3. **PC 名称** 填入 Windows �?Tailscale IP（如 `100.80.12.34`�?    
4. **用户账户** 选择"每次询问"或添加保�?    
5. 点击保存，然后连�?    

### Android 平板

1. Google Play 搜索 **"Windows App"** �?**"Microsoft Remote Desktop"**
    
2. 安装后打开，点�?**+** �?**Desktop**
    
3. **PC name** 填入 Tailscale IP（如 `100.80.12.34`�?    
4. **User name** 填入 Windows 用户�?    
5. 保存后点击连�
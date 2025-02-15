---
title: "管理子域名"
description: dns的独立子域名
weight: 6
draft: false
---



独立子域名是由复杂的主域名拆分而成，可用于不同用户帐号管理不同的业务域名。

QingCloud DNS 支持添加子域名，并独立管理子域名。

## 添加子域名

1. 登录 QingCloud 控制台。
2. 选择**产品与服务** > **网络服务** > **云解析 DNS**，进入域名列表页。
3. 点击列表上方的**添加**，弹出域名添加窗口。
4. 输入需要进行独立管理的子域名。
   
   ![添加域名](../_images/create_domain_2.png)

5. （可选）验证子域名所有权。
   
   如果主域名已托管在 QingCloud 且使用相同的用户帐号来添加子域名，则无需验证即可直接添加。

6. 在域名添加窗口，单击**添加**，返回域名列表，子域名添加成功。

### 验证所有权

为了防止恶意测试，若**主域名尚未被 QingCloud 当前用户帐号接管**，需验证添加的子域名的所有权。

1. 获取授权验证信息。
   1. 添加域名窗口输入子域名后，点击**添加**，窗口提示如下信息。
   
   ![子域验证提示](../_images/subzone_1.png)

   2. 点击**TXT 授权校验**，弹出域名信息验证提示窗口，即可获取当前子域名待授权验证信息。
   
   ![子域验证方法](../_images/subzone_2.png)

2. 配置上级域名验证
   
   根据弹窗提示，登录**该子域的上级域名**的权威解析处，并配置验证解析记录。
   
   - 下图所示上级域名托管在 QingCloud DNS 的不同用户帐号下，需要登录到该用户帐号，并设置验证。
   - 若上级域名托管在其他解析商，则需要登录到相应解析商管理控制台，并设置验证。
   
   ![子域验证示例](../_images/subzone_3.png)

3. 验证配置信息
   
   返回验证提示窗口，点击**验证**。
   
   ![子域验证方法](../_images/subzone_2.png)

4. 确认添加子域名
   
   提示**验证通过**，点击**确认添加**，即可成功添加子域。
   
   > **说明**
   >
   > 由于验证域名时受限于不同解析商的生效速度，在操作子域验证时，建议保持验证域名的浏览器窗口于等待验证状态，直至验证通过。
   
   ![确认添加子域](../_images/subzone_4.png)

## 自动转移子域名解析记录

当 QingCloud 主域名解析记录中，存在该子域名解析记录时。在独立管理的子域名后，系统将自动将子域名解析记录（NS 类型除外）转移到子域名解析记录列表下，且不再允许在主域管理中添加该子域名解析记录。

> **说明**
>
> NS 类型的解析记录将不会被转移，且在主域名解析记录中继续添加。

如图，域名 `ephen.icu` 下有两条子域名解析记录。

![主域的子域解析](../_images/subzone_5.png)

独立管理子域名 `demo.ephen.icu` 后，在解析记录列表中，提示将自动转移上述两条解析记录。

![子域解析转移](../_images/subzone_6.png)

返回域名 `ephen.icu` 解析记录列表，已无上述两条解析记录，且禁止再添加该子域名解析记录。

![禁止添加子域解析](../_images/subzone_7.png)

## 配置子域名服务器

QingCloud 支持托管拆分的子域名。为进一步加强对子域名的管理，可将子域名托管到 QingCloud 。

> **注意**
>
> 为了保证子域名在变换 DNS 服务器过程中的解析完整，修改 NS 前需要确保独立子域名解析配置正确有效。

1. 登录 QingCloud 管理控制台。

2. 查看并复制 QingCloud DNS 服务器。

    > **说明**：
    >
    > QingCloud DNS 会采取一定的规则将域名划分到不同的 NameServer 平台，具体服务器地址请以对应域名解析页面的提示为准。

    ![获取DNS服务器信息](../_images/subzone_8.png)

    如图，当前域名需要将 DNS 服务器修改为 `ns3.routewize.com` 和 `ns4.routewize.com` ，QingCloud DNS 才会接管。

3. 登录主域名的解析商管理控制台，为该子域名添加 NS 类型解析记录，记录值填写已复制的服务器地址。

    ![添加NS解析记录](../_images/subzone_9.png)

4. 请耐心等待 QingCloud 接管子域名，最长不超过72小时。

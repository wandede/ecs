# CreateLaunchTemplate {#doc_api_Ecs_CreateLaunchTemplate .reference}

创建一个实例启动模板，简称模板。实例启动模板能免除您每次创建实例时都需要填入大量配置参数。

## 接口说明 {#description .section}

实例启动模板中包含用于创建实例的相关配置，例如实例所属地域、镜像ID、实例规格、安全组ID和公网带宽等。如果模板中没有指定某一实例配置，您需要在创建实例时为实例指定该配置。创建模板（`CreateLaunchTemplate`）后，模板的初始版本为1，您可以后续基于该版本创建多个模板版本（`CreateLaunchTemplateVersion`），版本号从1开始顺序递增。如果您在创建实例（[RunInstances](~~63440~~)）时不指定模板版本号，会采用默认版本。

调用该接口时，您需要注意：

-   您最多能在一个地域内创建30个实例启动模板，且每个模板最多能有30 个版本。
-   实例启动模板的参数大多数为可选参数。创建模板时，我们不会验证模板中参数取值的存在性和有效性。只会在真正创建实例时校验参数取值的有效性。
-   如果实例启动模板中设置了某一配置，创建实例（[RunInstances](~~63440~~)）时就无法过滤掉该配置。例如，如果模板设置了`HostName=LocalHost`，`RunInstances` 中 `HostName`取值为空时，实例的主机名依然是`LocalHost`。如果您想覆盖`HostName=LocalHost`这一配置，可以在 `RunInstances` 中取`HostName=MyHost`或其他参数值。

## 调试 {#apiExplorer .section}

前往【[API Explorer](https://api.aliyun.com/#product=Ecs&api=CreateLaunchTemplate)】在线调试，API Explorer 提供在线调用 API、动态生成 SDK Example 代码和快速检索接口等能力，能显著降低使用云 API 的难度，强烈推荐使用。

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|LaunchTemplateName|String|是|JoshuaWinPrePaid|实例启动模板名称。长度为 2~128 个英文或中文字符。必须以大小字母或中文开头，不能以 http:// 和 https:// 开头。可以包含数字、半角冒号（:）、下划线（\_）或者连字符（-）。

 |
|RegionId|String|是|cn-hangzhou|地域ID。您可以调用 [DescribeRegions](~~25609~~) 查看最新的阿里云地域列表。

 |
|Action|String|否|CreateLaunchTemplate|系统规定参数。取值：CreateLaunchTemplate

 |
|AutoReleaseTime|String|否|2018-01-01T12:05:00Z|自动释放时间。按照 [ISO8601](~~25696~~) 标准表示，并需要使用 UTC 时间。格式为：yyyy-MM-ddTHH:mm:ssZ。

 -   如果秒（`ss`）取值不是 `00`，则自动取为当前分钟（`mm`）开始时。
-   最短释放时间为当前时间半小时之后。
-   最长释放时间不能超过当前时间三年。

 |
|DataDisk.N.Category|String|否|cloud\_ssd|数据盘n的磁盘种类。取值范围：

 -   cloud：普通云盘
-   cloud\_efficiency：高效云盘
-   cloud\_ssd：SSD 云盘
-   ephemeral\_ssd：本地 SSD 盘
-   cloud\_essd：ESSD 云盘。目前 ESSD 云盘正在火热公测中，仅部分地域下的可用区可以选购。更多详情，请参阅 [ESSD 云盘 FAQ](~~64950#AvailableRegion~~)。

 |
|DataDisk.N.DeleteWithInstance|Boolean|否|true|表示数据盘是否随实例释放。

 |
|DataDisk.N.Description|String|否|FinanceDept|实例描述。长度为 2~256 个英文或中文字符，不能以 http:// 和 https:// 开头。

 |
|DataDisk.N.DiskName|String|否|cloud\_ssdData|数据盘名称。长度为 2~128 个英文或中文字符。必须以大小字母或中文开头，不能以 http:// 和 https:// 开头。可以包含数字、半角冒号（:）、下划线（\_）或者连字符（-）。

 |
|DataDisk.N.Encrypted|String|否|false|数据盘n是否加密。

 |
|DataDisk.N.Size|Integer|否|2000|第n个数据盘的容量大小，n的取值范围为1~16，内存单位为 GiB。取值范围：

 -   cloud：5~2000
-   cloud\_efficiency：20~32768
-   cloud\_ssd：20~32768
-   cloud\_essd：20~32768
-   ephemeral\_ssd：5~800

 该参数的取值必须大于等于参数 `SnapshotId` 指定的快照的大小。

 |
|DataDisk.N.SnapshotId|String|否|s-bp17441ohwka0yuhx3h0|创建数据盘n使用的快照。n的取值范围为1~16。指定参数 `DataDisk.N.SnapshotId` 后，参数`DataDisk.N.Size`会被忽略，实际创建的磁盘大小为指定的快照的大小。不能使用早于2013年7月15日（含）创建的快照，请求会报错被拒绝。

 |
|Description|String|否|FinaceDept|实例描述。长度为 2~256 个英文或中文字符，不能以 http:// 和 https:// 开头。

 |
|EnableVmOsConfig|Boolean|否|false|是否启用实例操作系统配置。

 **说明：** 该参数即将被弃用，为提高兼容性，请尽量使用其他参数。

 |
|HostName|String|否|JoshuaHost|云服务器的主机名。

 -   点号（.）和短横线（-）不能作为首尾字符，更不能连续使用。
-   Windows 实例：字符长度为 2~15，不支持点号（.），不能全是数字。允许大小写英文字母、数字和短横线（-）。
-   其他类型实例（Linux 等）：字符长度为 2~64，支持多个点号（.），点之间为一段，每段允许大小写英文字母、数字和短横线（-）。

 |
|ImageId|String|否|win2008r2\_64\_ent\_sp1\_en-us\_40G\_alibase\_20170915.vhd|镜像 ID，启动实例时选择的镜像资源。您可以通过 [DescribeImages](~~25534~~) 查询您可以使用的镜像资源。

 |
|ImageOwnerAlias|String|否|system|镜像来源。取值范围：

 -   system：阿里云提供的公共镜像。
-   self：您创建的自定义镜像。
-   others：其他阿里云用户共享给您的镜像。
-   marketplace：[云市场](https://marketplace.alibabacloud.com/) 提供的镜像。您查询到的云市场镜像可以直接使用，无需提前订阅。您需要自行留意云市场镜像的收费详情。

 默认值：空，空表示返回取值为system、self以及others的结果。

 |
|InstanceChargeType|String|否|PrePaid|实例的付费方式。取值范围：

 -   PrePaid：预付费，包年包月。选择该类付费方式时，您必须确认自己的账号支持信用支付，否则将返回 `InvalidPayMethod` 的错误提示。
-   PostPaid：按量付费。

 |
|InstanceName|String|否|JoshuaHost|实例名称。长度为 2~128 个英文或中文字符。必须以大小字母或中文开头，不能以 http:// 和 https:// 开头。可以包含数字、半角冒号（:）、下划线（\_）或者连字符（-）。

 |
|InstanceType|String|否|ecs.g5.large|实例的资源规格。更多详情，请参阅 [实例规格族](~~25378~~)，也可以调用 [DescribeInstanceTypes](~~25620~~) 接口获得最新的规格表。

 |
|InternetChargeType|String|否|PayByTraffic|网络计费类型。取值范围：

 -   PayByTraffic：按使用流量计费

 |
|InternetMaxBandwidthIn|Integer|否|200|公网入带宽最大值，单位为 Mbit/s。取值范围：1~200

 |
|InternetMaxBandwidthOut|Integer|否|5|公网出带宽最大值，单位为 Mbit/s。取值范围：0~100

 |
|IoOptimized|String|否|optimized|是否为I/O优化实例。取值范围：

 -   none：非I/O优化
-   optimized：I/O优化

 |
|KeyPairName|String|否|Instancetest|密钥对名称。

 -   Windows实例，忽略该参数。即使填写了该参数，仍旧只执行 `Password` 的内容。
-   Linux实例的密码登录方式会被初始化成禁止。

 |
|NetworkInterface.N.Description|String|否|FinanceDept|弹性网卡描述信息。长度为 2~256 个英文或中文字符，不能以 http:// 和 https:// 开头。

 **说明：** 弹性网卡相关请求参数`NetworkInterface.N`的N取值不能大于1。

 |
|NetworkInterface.N.NetworkInterfaceName|String|否|FinanceJoshua|弹性网卡名称。

 **说明：** 弹性网卡相关请求参数`NetworkInterface.N`的N取值不能大于1。

 |
|NetworkInterface.N.PrimaryIpAddress|String|否|192.168.2.XXX|弹性网卡的主私有IP地址。

 **说明：** 弹性网卡相关请求参数`NetworkInterface.N`的N取值不能大于1。

 |
|NetworkInterface.N.SecurityGroupId|String|否|sg-bp15ed6xe1yxeycg7ov3|弹性网卡所属安全组的ID。弹性网卡的安全组和实例的安全组必须在同一个VPC下。

 **说明：** 弹性网卡相关请求参数`NetworkInterface.N`的N取值不能大于1。

 |
|NetworkInterface.N.VSwitchId|String|否|vsw-bp1s5fnvk4gn2tws03ziX|弹性网卡所属的虚拟交换机ID。实例与弹性网卡必须在同一VPC的同一可用区中，可以分属于不同交换机。

 **说明：** 弹性网卡相关请求参数`NetworkInterface.N`的N取值不能大于1。

 |
|NetworkType|String|否|vpc|实例网络类型。取值范围：

 -   classic
-   vpc

 |
|Period|Integer|否|1|购买资源的时长，单位为：月。当参数 `InstanceChargeType` 取值为 `PrePaid` 时才生效且为必选值。一旦指定了 DedicatedHostId，则取值范围不能超过专有宿主机的订阅时长。取值范围：

 -   `PeriodUnit=Week`时，Period取值：\{“1”, “2”, “3”, “4”\}
-   `PeriodUnit=Month`时，Period取值：\{ “1”, “2”, “3”, “4”, “5”, “6”, “7”, “8”, “9”, “12”, “24”, “36”,”48”,”60”\}

 |
|RamRoleName|String|否|FinanceDept|实例RAM角色名称。您可以使用 RAM API [ListRoles](~~28713~~) 查询您已创建的实例 RAM 角色。

 |
|ResourceGroupId|String|否|rg-resourcegroupid1|实例、磁盘和弹性网卡所在的企业资源组 ID。

 |
|SecurityEnhancementStrategy|String|否|Deactive|是否为操作系统开启安全加固。取值范围：

 -   Active：启用安全加固，只对公共镜像生效。
-   Deactive：不启用安全加固，对所有镜像类型生效。

 |
|SecurityGroupId|String|否|sg-bp15ed6xe1yxeycg7ov3|指定新创建实例所属于的安全组 ID。同一个安全组内的实例之间可以互相访问，一个安全组最多能管理 1000 台实例。

 |
|SpotDuration|Integer|否|1|实例保护周期。

 **说明：** 该参数即将被弃用，为提高兼容性，请尽量使用其他参数。

 |
|SpotPriceLimit|Float|否|0.97|设置实例的每小时最高价格。支持最大 3 位小数，参数 `SpotStrategy`取值为 `SpotWithPriceLimit`时生效。

 |
|SpotStrategy|String|否|NoSpot|按量实例的抢占策略。当参数 `InstanceChargeType` 取值为 `PostPaid` 时生效。取值范围：

 -   NoSpot：正常按量付费实例。
-   SpotWithPriceLimit：设置上限价格的抢占式实例。
-   SpotAsPriceGo：系统自动出价，跟随当前市场实际价格。

 |
|SystemDisk.Category|String|否|cloud\_ssd|系统盘的磁盘种类。取值范围：

 -   cloud：普通云盘
-   cloud\_efficiency：高效云盘
-   cloud\_ssd：SSD 云盘
-   ephemeral\_ssd：本地 SSD 盘
-   cloud\_essd：ESSD 云盘。目前 ESSD 云盘正在火热公测中，仅部分地域下的可用区可以选购。更多详情，请参阅 [ESSD 云盘 FAQ](~~64950#AvailableRegion~~)。

 |
|SystemDisk.Description|String|否|FinanceDept|系统盘描述。长度为 2~256 个英文或中文字符，不能以 http:// 和 https:// 开头。

 |
|SystemDisk.DiskName|String|否|cloud\_ssdSystem|系统盘名称。长度为 2~128 个英文或中文字符。必须以大小字母或中文开头，不能以 http:// 和 https:// 开头。可以包含数字、半角冒号（:）、下划线（\_）或者连字符（-）。

 |
|SystemDisk.Iops|Integer|否|30000|系统盘每秒I/O次数。

 **说明：** 该参数即将被弃用，为提高兼容性，请尽量使用其他参数。

 |
|SystemDisk.Size|Integer|否|40|系统盘大小，单位为GiB。取值范围：20~500

 该参数的取值必须大于或者等于max\{20, ImageSize\}。

 |
|Tag.N.Key|String|否|FinanceDept|实例、磁盘和主网卡的标签键。N 的取值范围：1~5。一旦传入该值，则不允许为空字符串。最多支持 64 个字符，不能以 aliyun 和 acs: 开头，不能包含 http:// 或者 https:// 。

 |
|Tag.N.Value|String|否|FinanceDept.Joshua|实例、磁盘和主网卡的标签值。N 的取值范围：1~5。一旦传入该值，可以为空字符串。最多支持 128 个字符，不能以 aliyun 和 acs: 开头，不能包含 http:// 或者 https:// 。

 |
|TemplateResourceGroupId|String|否|rg-resourcegroupid2|启动模板所在的企业资源组 ID。

 |
|TemplateTag.N.Key|String|否|LTFinance|启动模板的标签键。N 的取值范围：1~20。一旦传入该值，则不允许为空字符串。最多支持 64 个字符，不能以 aliyun 和 acs: 开头，不能包含 http:// 或者 https:// 。

 |
|TemplateTag.N.Value|String|否|LTFinanceJoshua|启动模板的标签值。N 的取值范围：1~20。一旦传入该值，可以为空字符串。最多支持 128 个字符，不能以 aliyun 和 acs: 开头，不能包含 http:// 或者 https:// 。

 |
|UserData|String|否|ZWNobyBoZWxsbyBlY3Mh|实例自定义数据，需要以Base64方式编码，原始数据最多为16 KB。

 |
|VSwitchId|String|否|vsw-bp1s5fnvk4gn2tws03ziX|创建 VPC 类型实例时需要指定虚拟交换机 ID。

 |
|VersionDescription|String|否|LTFinanceJoshua|实例启动模板版本1描述。长度为 2~256 个英文或中文字符，不能以 http:// 和 https:// 开头。

 |
|VpcId|String|否|vpc-bp12433upq1y5sceni07X|专有网络 VPC ID。

 |
|ZoneId|String|否|cn-hangzhou-g|实例所属的可用区 ID。

 |

## 返回参数 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|LaunchTemplateId|String|lt-m5eiaupmvm2op9dxxxxx|实例启动模板 ID。

 |
|RequestId|String|473469C7-AA6F-4DC5-B3DB-A3DC0DE3C83E|请求 ID。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

https://ecs.aliyuncs.com/?Action=CreateLaunchTemplate
&LaunchTemplateName=JoshuaWinPrePaid
&RegionId=cn-hangzhou
&TemplateTag.1.Key=LTFinance
&TemplateTag.1.Value=LTFinanceJoshua
&VersionDescription=LTFinanceJoshua
&ImageId=win2008r2_64_ent_sp1_en-us_40G_alibase_20170915.vhd
&InstanceType=ecs.g5.large
&SecurityGroupId=sg-bp15ed6xe1yxeycg7ov3
&VpcId=vpc-bp12433upq1y5sceni07X
&VSwitchId=vsw-bp1s5fnvk4gn2tws03ziX
&InstanceName=JoshuaHost
&Description=FinaceDept
&InternetMaxBandwidthIn=200
&InternetMaxBandwidthOut=5
&HostName=JoshuaHost
&ZoneId=cn-hangzhou-g
&SystemDisk.Category=cloud_ssd
&SystemDisk.Size=40
&SystemDisk.DiskName=cloud_ssdSystem
&SystemDisk.Description=FinanceDept
&DataDisk.1.Size=2000
&DataDisk.1.SnapshotId=s-bp17441ohwka0yuhx3h0
&DataDisk.1.Category=cloud_ssd
&DataDisk.1.Encrypted=false
&DataDisk.1.DiskName=cloud_ssdData
&DataDisk.1.Description=FinanceDept
&DataDisk.1.DeleteWithInstance=true
&IoOptimized=optimized
&NetworkInterface.1.PrimaryIpAddress=192.168.2.XXX
&NetworkInterface.1.VSwitchId=vsw-bp1s5fnvk4gn2tws03ziX
&NetworkInterface.1.SecurityGroupId=sg-bp15ed6xe1yxeycg7ov3
&NetworkInterface.1.1etworkInterfaceName=FinnanceJoshua
&NetworkInterface.1.Description=FinnanceDept
&InstanceChargeType=PrePaid
&Period=1
&InternetChargeType=PayByTraffic
&NetworkType=vpc
&UserData=ZWNobyBoZWxsbyBlY3Mh
&KeyPairName=Instancetest
&RamRoleName=FinanceDept
&AutoReleaseTime=2018-01-01T12:05:00Z
&SpotStrategy=NoSpot
&SpotPriceLimit=0.97
&SecurityEnhancementStrategy=Deactive
&Tag.1.Key=FinanceDept
&Tag.1.Value=FinanceDept.Joshua
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<CreateLaunchTemplateResponse>
  <RequestId>04F0F334-1335-436C-A1D7-6C044FExxxxx</RequestId>
  <LaunchTemplateId>lt-m5eiaupmvm2op9dxxxxx</LaunchTemplateId>
</CreateLaunchTemplateResponse>

```

`JSON` 格式

``` {#json_return_success_demo}
{
	"LaunchTemplateId":"lt-m5eiaupmvm2op9dxxxxx",
	"RequestId":"04F0F334-1335-436C-A1D7-6C044FExxxxx"
}
```

## 错误码 { .section}

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InvalidRegion.NotExist|%s|指定的Region不存在。|
|403|LaunchTemplateLimitExceed|%s|启动模板数量达到上限。|
|403|LaunchTemplateName.Duplicated|%s|模板名称不能相同。|
|400|MissingParameter|%s|缺失必需参数。|
|400|InvalidParameter|%s|参数格式不正确。|
|400|InvalidLaunchTemplateName.Malformed|The specified parameter LaunchTemplateName is not valid.|指定的模板名称格式无效。|
|400|InvalidDescription.Malformed|The specified parameter "VersionDescription" is not valid.|指定的模板版本描述格式无效。|
|403|InnerServiceFailed|%s|内部服务调用失败。|
|400|InvalidUserData.SizeExceeded|%s|UserData大小超出限制。|
|400|InvalidUserData.Base64FormatInvalid|%s|指定的模板版本描述格式无效。|

[查看本产品错误码](https://error-center.aliyun.com/status/product/Ecs)


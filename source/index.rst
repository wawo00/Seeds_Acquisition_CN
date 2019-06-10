广告种子获取API对接指引
=======================

本文档是 UPLTV 提供的 广告种子用户数据（下称“广告种子”）
获取API的操作文档。

1. Key 与 IP白名单
------------------

获取Key
^^^^^^^

在对接API之前，您需要先拿到专属于贵公司的
Key，其中包含两个重要参数：Client ID 和 Secret。获取方式：

1. 登陆 UPLTV 后台，在 “高级功能 - 用户级数据” 页面中，找到
   “获取广告种子” 功能模块；
2. 在 “API方式获取” 标签页中，点击 “将密钥发送到安全邮箱”；
3. 您的 Key 将会以邮件形式发送到您指定的安全邮箱中。

.. figure:: img/seed-pic3.png
   :scale: 70 %
   :alt: picture 3

IP白名单
^^^^^^^^

您还需将使用的 IP 配置到白名单中，UPLTV 会屏蔽所有不在白名单中的 IP所发出的接口请求。配置方式：

1. 登陆 UPLTV 后台，在 “高级功能 -用户级数据” 页面中，找到 “获取广告种子” 功能模块；
2. 在 “API方式获取”标签页中，可以看到 “IP白名单管理” 编辑框；
3. 点击编辑图标进行编辑，多条IP 使用换行分割，限最多10条IP。

.. figure:: img/seed-pic4.png
   :scale: 70 %
   :alt: picture 4

2. 获取 access_token
--------------------

**请求地址：**\ https://open-api.upltv.com/oauth/access_token

**请求方式：**\ POST

**请求参数：**

============= ====== ======== =============================================
参数          类型   是否必填 描述说明
============= ====== ======== =============================================
grant_type    String 是       客户端调用类型，固定为 **client_credentials**
client_id     String 是       通过邮件方式获取的 Client ID
client_secret String 是       通过邮件方式获取的 Secret
============= ====== ======== =============================================

**CURL请求示例：**
::

    curl -d 'grant_type=client_credentials&client_id=**<CLIENT_ID>**&client_secret=**<CLIENT_SECRET>**' https://open-api.upltv.com/oauth/access_token 

**返回参数：**

============ ====== ===================================
参数         类型   描述
============ ====== ===================================
expires_in   int    当前 Token 的有效时长，以“秒”为单位
access_token String 获取广告种子数据接口的 Token
============ ====== ===================================

**返回示例：**

::

   {
       "expires_in": 3600,
       "access_token": "xxxxxxxxxx"
   }

3. 获取广告种子数据
-------------------

**请求地址：**\ https://open-api.upltv.com/api/v.1.0.0/app/**<upltv_app_id>**/core/**<page>**/**<page_size>**

**请求方式：**\ GET

**header 信息：**\ 需设置 header 信息 “Authorization:Bearer **<access_token>**”

**相关参数：**

============ ====== ======== ==============================================
参数         类型   是否必须 描述说明
============ ====== ======== ==============================================
access_token String 是       接口 Token，获取方式见于前文
upltv_app_id     -      是       应用在 UPLTV 后台的 App ID，可在应用列表中查阅
page         -      是       分页页数（返回数据为分页形式）
page_size    -      是       每页数据条数
============ ====== ======== ==============================================

**CURL请求示例：**
::

    curl -H "Authorization:Bearer **<ACCESS_TOKEN>**" https://open-api.upltv.com/api/v.1.0.0/app/**<upltv_id>**/core/**<page>**/**<page_size>**

**返回参数：**

========= ====== =====================
参数      类型   描述
========= ====== =====================
country   String 用户所属国家的iso代码
device    String 用户的设备id
page      int    当前页码
pageCount int    总页数
========= ====== =====================

**返回示例：**

::

   {
       "code": 200,
       "data": {
           "list": [
               {
                   "country": "cn",
                   "device": "xxxxxxxxxxxxxxxxxxxxxxx"
               },
               {
                   "country": "us",
                   "device": "xxxxxxxxxxxxxxxxxxxxxxx"
               }
           ],
           "page": 1,
           "pageCount": 1
       }
   }

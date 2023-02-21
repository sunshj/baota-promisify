# 宝塔面板API-Promise版

本项目基于 [baota](https://www.npmjs.com/package/baota)，
[`宝塔面板官网`](https://www.bt.cn/?invite_code=MV96c3RtcW4=)，
[`官网api接口文档`](https://www.bt.cn/bbs/forum.php?mod=viewthread&tid=20376&page=1)<br/>

## 安装

```bash
npm install baota-promisify --save
```

使用前：在 **面板设置** 里，打开API接口，添加ip白名单，复制接口密钥。<br/>
参数说明：`?` 表示可选参数。

## 使用

```typescript
const Baota = require('baota-promisify');
const config = {
    host: 'http://127.0.0.1:8888',       // 请修改成自己宝塔面板地址
    key: 'your api key',                // 在 面板设置 里查看
    proxy? : 'http://127.0.0.1:9999',    // 代理，如不需代理，请勿填写
}
const baota = new Baota(config);
baota.create(Baota.getSystemTotal).then(res => {
    console.log(res)
})
```

响应示例：

```js
{
  memTotal: 3708,
  memFree: 1730,
  memBuffers: 242,
  memCached: 1289,
  memRealUsed: 447,
  cpuNum: 2,
  cpuRealUsed: 1,
  time: '11天',
  system: 'Alibaba Cloud  (Aliyun ) 2.1903 LTS (Hunting Beagle) x86_64(Py3.7.9)',
  isuser: 0,
  isport: false,
  version: '7.9.0'
}
```

### 基础信息

<details>
<summary>1.获取系统基础统计</summary>
<span id="getSystemTotal"></span><h4 >1.获取系统基础统计</h4>

```js
baota.create(Baota.getSystemTotal).then(res => {
    console.log(res)
})
```

响应示例：

```js
{
  memTotal: 3708,
  // 总内存
  memFree: 1730,
  memBuffers: 242,
  memCached: 1289,
  memRealUsed: 447,
  // 占用内存
  cpuNum: 2,
  // CPU内核
  cpuRealUsed: 1,
  time: '11天',
  // 不间断运行天数
  system: 'Alibaba Cloud  (Aliyun ) 2.1903 LTS (Hunting Beagle) x86_64(Py3.7.9)',
  isuser: 0,
  isport: false,
  version: '7.9.0'
  // 宝塔面板版本
}
```

</details>

<!-- 2.获取磁盘分区信息 -->
<details>
<summary>2.获取磁盘分区信息</summary>
<h4 id="getDiskInfo">2.获取磁盘分区信息</h4>

```js
baota.create(Baota.getDiskInfo).then(res => {
    console.log(res)
})
```

响应示例：

```js
[
  {
    filesystem: '/dev/vda1',
    // 文件系统
    type: 'ext4',
    // 类型
    path: '/',
    // 挂载点
    size: [
      '40G',
      '4.8G',
      '33G',
      '13%'
    ],
    // 容量，已用，可用，使用率
    inodes: [
      '2621440',
      '159820',
      '2461620',
      '7%'
    ]
    // inodes总数，已用，可用，使用率
  },
  {
    filesystem: '06a0000029-llw96.cn-hangzhou.nas.aliyuncs.com:/',
    // 阿里云NAS
    type: 'nfs',
    path: '/mnt',
    size: [
      '1.0P',
      '624M',
      '1.0P',
      '1%'
    ],
    inodes: [
      '1000000000',
      '49899',
      '999950101',
      '1%'
    ]
  }
]
```

</details>

<!-- 3.获取实时状态信息(CPU、内存、网络、负载) -->
<details>
<summary>3.获取实时状态信息(CPU、内存、网络、负载)</summary>
<h4 id="getNetWork">3.获取实时状态信息(CPU、内存、网络、负载)</h4>

```js
baota.create(Baota.getNetWork).then(res => {
    console.log(res)
})
```

响应示例：

```js
{
  network: {
    lo: {
      upTotal: 148728,
      downTotal: 148728,
      up: 0,
      down: 0,
      downPackets: 1135,
      upPackets: 1135
    },
    eth0: {
      upTotal: 812858716,
      downTotal: 832436955,
      up: 3.69,
      down: 2,
      downPackets: 2264129,
      upPackets: 1568873
    }
  },
  upTotal: 813007444,
  // 总发送 
  downTotal: 832585683,
  // 总接收 832585683÷1024÷1024=794.02MB
  up: 3.69,
  // 上行速率 KB/s
  down: 2,
  // 下行速率 KB/s
  downPackets: 2265264,
  upPackets: 1570008,
  cpu: [
    7,
    2,
    [
      0,
      0
    ],
    'Intel(R) Xeon(R) Platinum 8163 CPU @ 2.50GHz * 1',
    1,
    1
  ],
  cpu_times: {
    user: 2,
    nice: 0,
    system: 1.1,
    idle: 96.9,
    iowait: 0,
    irq: 0,
    softirq: 0,
    steal: 0,
    guest: 0,
    guest_nice: 0,
    '总进程数': 106,
    '活动进程数': 1
  },
  load: {
    one: 0.35,
    five: 0.11,
    fifteen: 0.02,
    max: 4,
    limit: 4,
    safe: 3
  },
  // 最近1分钟|5分钟|15分钟平均负载 
  mem: {
    memTotal: 3708,
    // 总内存
    memFree: 1750,
    memBuffers: 242,
    memCached: 1290,
    memRealUsed: 426
    // 实际占用内存
  },
  version: '7.9.0',
  // 宝塔面板版本
  disk: [
    // 磁盘分区信息
    {
      filesystem: '/dev/vda1',
      type: 'ext4',
      path: '/',
      size: [
        '40G',
        '4.8G',
        '33G',
        '13%'
      ],
      inodes: [
        '2621440',
        '159820',
        '2461620',
        '7%'
      ]
    },
    {
      filesystem: '06a0000029-llw96.cn-hangzhou.nas.aliyuncs.com:/',
      type: 'nfs',
      path: '/mnt',
      size: [
        '1.0P',
        '624M',
        '1.0P',
        '1%'
      ],
      inodes: [
        '1000000000',
        '49899',
        '999950101',
        '1%'
      ]
    }
  ],
  title: '宝塔面板',
  // 面板别名，可在“面板设置”里修改
  time: '11天',
  // 不间断运行时间
  site_total: 3,
  // 网站数量(PHP项目+JAVA项目+Node项目)
  ftp_total: 3,
  // FTP数量
  database_total: 3,
  // 数据库数量
  system: 'Alibaba Cloud  (Aliyun ) 2.1903 LTS (Hunting Beagle) x86_64(Py3.7.9)',
  // 系统
  installed: true,
  user_info: {
    status: true,
    msg: '获取成功!',
    data: {
      username: '138****8888'
    }
  },
  // 宝塔账号信息
  iostat: {
    ALL: {
      read_count: 0,
      write_count: 2,
      read_bytes: 0,
      write_bytes: 16384,
      read_time: 0,
      write_time: 1,
      read_merged_count: 0,
      write_merged_count: 0
    },
    vda: {
      read_count: 0,
      write_count: 1,
      read_bytes: 0,
      write_bytes: 8192,
      read_time: 0,
      write_time: 1,
      read_merged_count: 0,
      write_merged_count: 0
    },
    vda1: {
      read_count: 0,
      write_count: 1,
      read_bytes: 0,
      write_bytes: 8192,
      read_time: 0,
      write_time: 0,
      read_merged_count: 0,
      write_merged_count: 0
    }
  }
}
```

</details>

<!-- 检查是否有安装任务 -->
<details>
<summary>4.检查是否有安装任务</summary>
<h4 id="getTaskCount">4.检查是否有安装任务</h4>

```js
baota.create(Baota.getTaskCount).then(res => {
    console.log(res)
})
```

</details>

<!-- 5.检查面板更新 -->
<details>
<summary>5.检查面板更新</summary>
<h4 id="updatePanel">5.检查面板更新</h4>

```js
baota.create(Baota.updatePanel).then(res => {
    console.log(res)
})
```

响应示例：

```js
{
  status: false,
  msg: {
    force: false,
    version: '7.9.0',
    downUrl: 'http://103.224.251.67/install/update/LinuxPanel-7.9.0.zip',
    updateMsg: '【重构】重构面板设置页面<br>\n'
    +
    '【调整】调整部分UI显示效果<br>\n'
    +
    '【调整】不再提供入口关闭功能<br>\n'
    +
    '【调整】phpMyAdmin使用面板端口访问，不再依赖888端口<br>\n'
    +
    '......',
    uptime: '2022/02/18',
    is_beta: 0,
    adviser: -1,
    btb: '<span>每升级一个宝塔测试版版本可以随机获得10-50个宝塔币，宝塔币可用于兑换礼品，欢迎您加入测试版测试。<a href="https://www.bt.cn/bbs/thread-21014-1-1.html" rel="noreferrer noopener" target="_blank" class="btlink">宝塔币兑换</a></span>',
    beta: {
      version: '7.9.28',
      updateMsg: '【增加】增加网站安全扫描模块(在网站-设置)<br>\n'
      +
      '【优化】进一步减少对云端的依赖<br>\n'
      +
      '【优化】优化Node/Java项目管理器<br>\n'
      +
      '......',
      uptime: '2022/05/11',
      downUrl: 'http://103.224.251.67/install/update/LinuxPanel-7.9.28.zip'
    }
  }
}
```

</details>

<details>
<summary>6.安全风险扫描</summary>
<h4 id="getRiskList">6.安全风险扫描</h4>

```js
baota.create(Baota.getRiskList).then(res => {
    console.log(res)
})
```

响应示例：

```js
{
  ignore: [],
  // 已忽略
  security: [
    // 安全项
    {
      title: '数据库备份权限检测',
      m_name: 'sw_mysql_priv',
      ps: '检测MySQL root用户是否具备数据库备份权限',
      version: 1,
      level: 3,
      ignore: false,
      date: '2020-09-19',
      tips: [
        Array
      ],
      help: '',
      status: true,
      msg: '无风险',
      check_time: 1650413734,
      taking: 0.000547
    },
    {
      title: 'Redis安全检测',
      m_name: 'sw_redis_port',
      ps: '检测当前Redis是否安全',
      version: 1,
      level: 3,
      ignore: false,
      date: '2020-08-04',
      tips: [
        Array
      ],
      help: '',
      status: true,
      msg: '无风险',
      check_time: 1650413734,
      taking: 0.000006
    },
    ...
  ],
  risk: [
    // 风险项
    {
      title: '系统防火墙检测',
      m_name: 'sw_firewall_open',
      ps: '检测是否开启系统防火墙',
      version: 1,
      level: 2,
      ignore: false,
      date: '2020-08-05',
      tips: [
        Array
      ],
      help: '',
      status: false,
      msg: '未开启系统防火墙，存在安全风险',
      check_time: 1650413734,
      taking: 0.015507
    },
    {
      title: '面板端口安全',
      m_name: 'sw_panel_port',
      ps: '检测当前面板端口是否安全',
      version: 1,
      level: 2,
      ignore: false,
      date: '2020-08-03',
      tips: [
        Array
      ],
      help: '',
      status: false,
      msg: '面板端口为默认端口(8888), 这可能造成不必要的安全风险',
      check_time: 1650413734,
      taking: 0.000044
    },
    {
      title: 'SSH端口安全',
      m_name: 'sw_ssh_port',
      ps: '检测当前服务器的SSH端口是否安全',
      version: 1,
      level: 1,
      ignore: false,
      date: '2020-08-04',
      tips: [
        Array
      ],
      help: '',
      status: false,
      msg: '默认SSH端口(22)未修改，且未做访问IP限定配置，有SSH暴破风险',
      check_time: 1650413734,
      taking: 0.011211
    }
  ]
}
```

</details>

### 网站

<details>
<summary>1.获取网站分类列表</summary>
<h4 id="getSiteTypes">1.获取网站分类列表</h4>

```js
baota.create(Baota.getSiteTypes).then(res => {
    console.log(res)
})
```

响应示例：

```js
[
  {
    id: 0,
    name: '默认分类'
  },
  {
    id: 1,
    name: '分类一'
  },
  {
    id: 2,
    name: 'aaa'
  }
]
```

</details>

<details>
<summary>2.新建网站分类</summary>
<h4 id="addSiteTypes">2.新建网站分类</h4>

```js
const name = '分类三';    // 分类名称

baota.create(Baota.addSiteTypes, name).then(res => {
    console.log(res)
})
```

响应示例：

```js
{
  "status": true,
  "msg": "添加成功!"
}
```

</details>

<details>
<summary>3.删除网站分类</summary>
<h4 id="delSiteTypes">3.删除网站分类</h4>

```js
const id = 3;          // 分类id

baota.create(Baota.delSiteTypes, id).then(res => {
    console.log(res)
})
```

响应示例：

```js
{
  "status": true,
  "msg": "分类已删除!"
}
```

</details>

<details>
<summary>4.获取已安装的 PHP 版本列表</summary>
<h4 id="getPHPVersion">4.获取已安装的 PHP 版本列表</h4>

```js
baota.create(Baota.getPHPVersion).then(res => {
    console.log(res)
})
```

响应示例：

```js
[
  {
    "version": "00",
    "name": "纯静态"
  },
  {
    "version": "56",
    "name": "PHP-56"
  },
  {
    "version": "72",
    "name": "PHP-72"
  }
]
```

</details>

<!-- 1.获取网站列表 -->
<details>
<summary>5.获取网站列表</summary>
<h4 id="getSitesList">5.获取网站列表</h4>

```typescript
const sitesQuery = {
    p? : number,        // 页数，默认为1
    limit? : number,    // 每页数量，默认20
    type? : number,     // 网站分类 -1全部  0默认分类
    order? : 'id desc' | 'id asc' | 'name desc' | 'name asc' | 'status desc' | 'status asc' | 'edate desc' | 'edate asc',
    // 排序：id降序|升序，网站名、状态、到期时间
    tojs? : string,     // 分页 JS 回调,若不传则构造 URI 分页连接
    search? : string,   // 搜索网站名
}

baota.create(Baota.getSitesList, sitesQuery).then(res => {
    console.log(res)
})
```

响应示例：

```js
{
  id: 1,
  name: 'www.test.com',
  // 网站名
  path: '/www/wwwroot/www.test.com',
  // 根目录
  status: '1',
  // 0:已停止 1:运行中
  ps: 'www.test.com',
  // 备注
  addtime: '2022-05-10 12:08:46',
  // 添加时间
  edate: '0000-00-00',
  // 到期时间 0000-00-00表示永久
  backup_count: 0,
  // 备份数
  quota: [
    Object
  ],
  // 容量
  domain: 2,
  // 域名数量
  ssl: -1,
  // SSL，-1表示未启用SSL
  php_version: '静态'
}
```

</details>

<details>
<summary>6.创建网站</summary>
<h4 id="addSite">6.创建网站</h4>

```typescript
const webName = {
    domain: string,         // 主域名 如：www.test.com
    domainlist: string[],   // 除了主域名外的其他域名 如：['aa.test.com','www.aaa.com']
}
const newSite = {
    webname: webName,       // 网站主域名和域名列表
    path: string,           // 网站根目录
    type: 'PHP',            // 项目类型，请传 PHP
    type_id: number,        // 网站分类id
    version: string,        // PHP版本 请从 PHP 版本列表中选择；00为纯静态
    port: number,           // 端口，如：80
    ps: string,             // 网站备注
    ftp: boolean,           // 是否开启FTP
    ftp_username? : string,  // FTP用户名, 在开启FTP时必填
    ftp_password? : string,  // FTP密码, 在开启FTP时必填
    sql: boolean,           // 是否创建数据库
    codeing? : string,       // 数据库字符集 在要创建数据库时必传,如：utf8
    datauser? : string,      // 数据库用户名 在要创建数据库时必传
    datapassword? : string,  // 数据库密码 在要创建数据库时必传
}

baota.create(Baota.addSite, newSite).then(res => {
    console.log(res)
})
```

响应示例：

```js
{
  siteStatus: true,
  // 网站状态
  siteId: 1,
  // 网站id
  ftpStatus: false,
  // 是否开启FTP
  databaseStatus: false
  // 是否创建数据库
}
```

</details>

<details>
<summary>7.删除网站</summary>
<h4 id="delSite">7.删除网站</h4>

```typescript
const options = {
    id: number,      // 网站id
    webname: string, // 网站主域名
    ftp? : 1,         // 删除ftp，如不需要删除，请不要传该参数
    database? : 1,    // 删除数据库，如不需要删除，请不要传该参数
    path? : 1,        // 删除网站根目录，如不需要删除，请不要传该参数
}

baota.create(Baota.delSite, options).then(res => {
    console.log(res)
})
```

响应示例：

```js
{
  "status": true,
  "msg": "站点删除成功!"
}
```

</details>

<details>
<summary>8.停用网站</summary>
<h4 id="siteStop">8.停用网站</h4>

```typescript
const site = {
    id: number,      // 网站id
    name: string,    // 网站主域名
}

baota.create(Baota.siteStop, site).then(res => {
    console.log(res)
})
```

响应示例：

```js
{
  "status": true,
  "msg": "站点已停用"
}
```

</details>

<details>
<summary>9.启用网站</summary>
<h4 id="siteStart">9.启用网站</h4>

```typescript
const site = {
    id: number,      // 网站id
    name: string,    // 网站主域名
}

baota.create(Baota.siteStart, site).then(res => {
    console.log(res)
})
```

响应示例：

```js
{
  "status": true,
  "msg": "站点已启用"
}
```

</details>

<details>
<summary>10.设置网站到期时间</summary>
<h4 id="setSiteEndDate">10.设置网站到期时间</h4>

```typescript
const formData = {
    id: number,       // 网站id
    edate: string,    // 到期日期，格式: '2023-05-17'；永久：'0000-00-00'
}

baota.create(Baota.setSiteEndDate, formData).then(res => {
    console.log(res)
})
```

响应示例：

```js
{
  "status": true,
  "msg": "设置成功,站点到期后将自动停止!"
}
```

</details>

<details>
<summary>11.修改网站备注</summary>
<h4 id="setSitePs">11.修改网站备注</h4>

```typescript
const formData = {
    id: number,       // 网站id
    ps: string,       // 备注
}

baota.create(Baota.setSitePs, formData).then(res => {
    console.log(res)
})
```

响应示例：

```js
{
  status: true,
  msg: '修改成功'
}
```

</details>

<!-- FTP -->

### FTP

注：需先安装Pure-Ftpd。
<details>
<summary>1.获取FTP列表（可搜索）</summary>
<h4>1.获取FTP列表（可搜索）</h4>

```typescript
let formData = {
    p? : 1,              // 分页，可选，默认为1
    limit? : 20,        // 每页数量，可选，默认为20
    search? : 'abc',   // 搜索ftp的username，可选，默认为空
}

baota.create(Baota.getFtpList, formData).then(res => {
    console.log(res)
})
```

响应示例：

```js
[
  {
    id: 1,
    // id
    pid: 0,
    name: 'aaa',
    // 用户名
    password: 'Jbhb86h3jbmyzmAP',
    // 密码
    status: '1',
    // 0:停用，1:启用
    ps: 'aaa',
    // 备注
    addtime: '2022-05-17 09:51:29',
    // 添加时间
    path: '/mnt/aaa',
    // 路径
    quota: {
      size: 0,
      used: 0,
      free: 0
    }
    // 容量；注：企业版专享功能
  }
]
```

</details>



<details>
<summary>2.添加FTP</summary>
<h4>2.添加FTP</h4>

```typescript
let formData = {
    username: string,  // FTP用户名
    password: string,  // FTP密码
    path: string,      // FTP路径
    ps? : string,       // 备注，可选
}

baota.create(Baota.addFtp, formData).then(res => {
    console.log(res)
})
```

响应示例：

```js
{
  status: true,
  msg: '添加成功'
}
```

</details>

<details>
<summary>3.设置FTP状态，停用/启用</summary>
<h4>3.设置FTP状态，停用/启用</h4>

```typescript
let formData = {
    id: number,        // FTP id
    username: string,  // FTP 用户名
    status: 0 | 1,      // 设置FTP状态，0:停用，1:启用
}

baota.create(Baota.setFtpStatus, formData).then(res => {
    console.log(res)
})
```

响应示例：

```js
{
  status: true,
  msg: '操作成功'
}
```

</details>

<details>
<summary>4.删除FTP</summary>
<h4>4.删除FTP</h4>

```typescript
let formData = {
    id: number,        // FTP id
    username: string,  // FTP 用户名
}

baota.create(Baota.delFtp, formData).then(res => {
    console.log(res)
})
```

响应示例：

```js
{
  status: true,
  msg: '删除成功'
}
```

</details>

<details>
<summary>5.修改FTP密码</summary>
<h4>5.修改FTP密码</h4>

```typescript
let formData = {
    id: number,           // FTP id
    username: string,     // FTP 用户名
    newPassword: string,  // 新密码，可使用baota.randomPassword()生成
}

baota.create(Baota.setFtpPassword, formData).then(res => {
    console.log(res)
})
```

响应示例：

```js
{
  status: true,
  msg: '修改成功'
}
```

</details>

<details>
<summary>6.修改FTP端口</summary>
<h4>6.修改FTP端口</h4>
注意：该操作会影响所有FTP

```js
let port = 21;        // 新的FTP端口

baota.create(Baota.setFtpPort, port).then(res => {
    console.log(res)
})
```

响应示例：

```js
{
  status: true,
  msg: '修改成功'
}
```

</details>

<details>
<summary>7.修改FTP备注</summary>
<h4>7.修改FTP备注</h4>

```js
let formData = {
    id: number,        // FTP id
    ps: string         // 备注
}

baota.create(Baota.setFtpPs, formData).then(res => {
    console.log(res)
})
```

响应示例：

```js
{
  status: true,
  msg: '修改成功'
}
```

</details>

<details>
<summary>8.FTP容量</summary>
<h4>8.FTP容量</h4>
注：此功能为企业版专享功能； 如需取消容量配额，请设为“0”。

```js
let formData = {
    size: number,     // 容量，单位为 MB
    path: string      // 路径
}

baota.create(Baota.setFtpQuota, formData).then(res => {
    console.log(res)
})

```

</details>

<!-- 监控 -->

### 监控

<details>
<summary>1.获取监控状态</summary>
<h4>1.获取监控状态</h4>

```js
baota.create(Baota.getMonitorStatus).then(res => {
    console.log(res)
})
```

响应示例：

```js
{
  day: 30,
  // 监控保存时间
  status: true
  // 是否开启监控
}
```

</details>

<details>
<summary>2.设置监控状态、保存时间</summary>
<h4>2.设置监控状态、保存时间</h4>

```typescript
let formData = {
    status: 0 | 1,      // 0:开启  1:关闭
    day? : number      // 保存天数，可选，默认为30天
}

baota.create(Baota.setMonitorStatus, formData).then(res => {
    console.log(res)
})
```

响应示例：

```js
{
  status: true,
  msg: '设置成功'
}
```

</details>

<details>
<summary>3.平均负载</summary>
<h4>3.平均负载</h4>

```typescript
let range = {
    start? : number,      // 时间戳，精确到秒，如：1652500800
    end? : number         // 时间戳，精确到秒，如：1652587200
}
// start和end需同时传入，如都留空，则默认为： start：当前时间-1天；end:当前时间
baota.create(Baota.getLoadAvg, range).then(res => {
    console.log(res)
})
```

响应示例：

```js
[
  {
    id: 1,
    pro: 2.5,
    // 资源使用率
    one: 0.1,
    // 一分钟负载
    five: 0.07,
    // 五分钟负载
    fifteen: 0.01,
    // 十五分钟负载
    addtime: '05/17 16:55'
  },
  {
    id: 2,
    pro: 2.25,
    one: 0.09,
    five: 0.07,
    fifteen: 0.01,
    addtime: '05/17 16:56'
  }
]
```

</details>

<!-- CPU及内存使用率 -->
<details>
<summary>4.CPU及内存使用率</summary>
<h4>4.CPU及内存使用率</h4>

```js
baota.create(Baota.getCPUIO).then(res => {
    console.log(res)
})
```

响应示例：

```js
[
  {
    id: 1,
    pro: 4,
    // CPU使用率
    mem: 12.511435536443068,
    // 内存使用率
    addtime: "05/15 11:21"
  },
  {
    id: 2,
    pro: 3.5,
    mem: 12.537839984661316,
    addtime: "05/15 11:22"
  }
  ...
]
```

</details>

<!-- 磁盘IO -->
<details>
<summary>5.磁盘IO</summary>
<h4>5.磁盘IO</h4>

```typescript
let range = {
    start? : number,      // 时间戳，精确到秒，如：1652500800
    end? : number         // 时间戳，精确到秒，如：1652587200
}
// start和end需同时传入，如都留空，则默认为： start：当前时间-1天；end:当前时间
baota.create(Baota.getDiskIO, range).then(res => {
    console.log(res)
})
```

响应示例：

```js
[
  {
    id: 1,
    read_count: 0,
    // 读取数
    write_count: 36,
    // 写入数
    read_bytes: 0,
    // 读取速率 KB/s
    write_bytes: 240844,
    // 写入速率 KB/s
    read_time: 0,
    // 读取延迟 ms
    write_time: 21,
    // 写入延迟 ms
    addtime: "05/17 11:21"
  },
  {
    id: 2,
    read_count: 0,
    write_count: 11,
    read_bytes: 0,
    write_bytes: 98304,
    read_time: 0,
    write_time: 4,
    addtime: "05/17 11:22"
  }
]
```

</details>

<!-- 网络IO -->
<details>
<summary>6.网络IO</summary>
<h4>6.网络IO</h4>

```typescript
let range = {
    start? : number,      // 时间戳，精确到秒，如：1652500800
    end? : number         // 时间戳，精确到秒，如：1652587200
}
// start和end需同时传入，如都留空，则默认为： start：当前时间-1天；end:当前时间
baota.create(Baota.getNetIO, range).then(res => {
    console.log(res)
})
```

响应示例：

```js
[
  {
    "id": 1,
    "up": 11.58,
    // 上行 KB/s
    "down": 8.67,
    // 下行 KB/s
    "total_up": 18
    0195
    4215,
    // 总发送 B
    "total_down": 19
    7486
    4203,
    // 总接收 B
    "down_packets": {
      "eth0": 8.67,
      "lo": 0
    },
    "up_packets": {
      "eth0": 11.58,
      "lo": 0
    },
    "addtime": "05/15 11:21"
  },
  {
    "id": 2,
    "up": 2.05,
    "down": 0.31,
    "total_up": 1802023020,
    "total_down": 1974903569,
    "down_packets": {
      "eth0": 0.31,
      "lo": 0
    },
    "up_packets": {
      "eth0": 2.05,
      "lo": 0
    },
    "addtime": "05/15 11:22"
  }
]

```

</details>


<!-- 终端 -->

### 终端

<details>
<summary>1.获取SSH终端列表</summary>
<h4>1.获取SSH终端列表</h4>

```js
baota.create(Baota.getSSHList).then(res => {
    console.log(res)
})
```

响应示例：

```js
[
  {
    "host": "127.0.0.1",
    "port": 22,
    "ps": "本地服务器",
    "sort": 0
  }
]
```

</details>

<details>
<summary>2.添加/修改SSH</summary>
<h4>2.添加/修改SSH</h4>

```typescript
let formData = {
    host: string,           // host，如: 127.0.0.1
    port: number,           // 端口，通常为 21
    username: string,       // 用户名，如：root
    password? : string,      // 密码：可选，密码和私钥二选一
    pkey? : string,          // 私钥：可选，密码和私钥二选一
    ps? : string             // 备注，可选
}

baota.create(Baota.addSSH, formData).then(res => {
    console.log(res)   // { "status": true, "msg": "添加成功" }
})
```

</details>

<details>
<summary>3.删除SSH</summary>
<h4>3.删除SSH</h4>

```typescript
let formData = {
    host: string,           // host，如: 127.0.0.1
}

baota.create(Baota.delSSH, formData).then(res => {
    console.log(res)     // { "status": true, "msg": "删除成功" }
})
```

</details>


<details>
<summary>4.获取常用命令</summary>
<h4>4.获取常用命令</h4>

```js
baota.create(Baota.getCommandList).then(res => {
    console.log(res)
})
```

响应示例：

```js
[
  {
    "title": "查看pm2列表",
    "shell": "pm2 list"
  }
]
```

</details>

<details>
<summary>5.添加常用命令</summary>
<h4>5.添加常用命令</h4>

```typescript
const command = {
    shell: string,        // ssh语句
    title: string,        // title
}

baota.create(Baota.createCommand, command).then(res => {
    console.log(res)    // { "status": true, "msg": "添加成功" }
})
```

</details>

<details>
<summary>6.修改常用命令</summary>
<h4>6.修改常用命令</h4>

```typescript
const command = {
    shell: string,       // ssh语句，如不需修改，则传入原来的语句；如需修改，请传入修改后的语句
    title: string,       // 原title
    newTitle? : string，  // 新title，如不需修改，不填或传入原title
}

baota.create(Baota.modifyCommand, command).then(res => {
    console.log(res)  // { "status": true, "msg": "修改成功" }
})
```

</details>

<details>
<summary>7.删除常用命令</summary>
<h4>7.删除常用命令</h4>

```typescript
const command = {
    title: string,       // title
}

baota.create(Baota.getSystemTotal, command).then(res => {
    console.log(res) // { "status": true, "msg": "删除成功" }
})
```

</details>


<!-- 软件相关操作 -->

### 软件相关操作

<details>
<summary>1.获取软件列表 (可搜索)</summary>
<h4>1.获取软件列表 (可搜索)</h4>

```typescript
let formData = {
    p? : number,       // 页码，可选，默认为1；每页数量为15
    type? : number     // 分类，可选，默认为0；0:全部 -1:已安装 5:运行环境 6:系统工具 7:宝塔插件 8:专业版插件 10:第三方应用 11: 一键部署 12:企业版插件
    query? : string,   // 搜索内容，可选
}

baota.create(Baota.getSoftList, formData).then(res => {
    console.log(res)
})
```

</details>

# 其他

1.宝塔官网API文档：[`https://www.bt.cn/api-doc.pdf`](https://www.bt.cn/api-doc.pdf)。注：部分API与官方文档里有所不同。

2.该npm包是参照宝塔Linux 7.9.0版本写的，其他版本的API可能会有不同，请留意。


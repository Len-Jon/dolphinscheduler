# Apache Dolphinscheduler 3.2.2-Datax组件DAMENG定制

## 关于

3.2.X版本数据源新增了达梦（DM），但是Datax节点中不可选择，只能使用自定义配置的方案，因此打算魔改一下此版本

> 为什么基于3.2.X？
> 
> 因为 3.1.X 数据源类型还没接入DAMENG；
> 
> 从 3.3.0 版本开始，二进制包不再提供插件依赖

- 主要改前端支持，后端datax节点switch分支default调用的都是RDBMS的READER/WRITER，不用改，本版本只改了前端显式调用DAMENG
- 已部署项目，可以编译dolphinscheduler-ui部分，dist的内容替换api-server/ui/的内容（推荐）
- 未部署项目，可以使用官网的二进制包/源码包，然后参考已部署项目方式修改，二进制包部署参照官方文档有一堆问题
- 未部署项目，可以直接使用本项目release（todo，没有就是没传）

## 前端编译说明

### 一、环境要求

1. Node.js 16.x.x
2. pnpm 7.x.x

### 二、环境安装

1. 安装Node.js

访问 [Node.js 官网](https://nodejs.org/) 下载 16.x.x 版本并安装。

> 也可以选择装个nvm，多版本管理比较好

```bash
node --version
# 输出: v16.20.2
```

2. 安装 pnpm

```bash
npm install -g pnpm@7
```

3. 安装依赖

```bash
pnpm install
```

### 三、构建生产环境

跳过类型检查直接构建:

```bash
npx vite build --mode production
```

构建完成后，产物位于 `dist/` 目录:
```
dist/
├── assets/           # 静态资源 (JS, CSS, 图片等)
├── images/          # 图片资源
├── favicon.ico     # 网站图标
├── index.html      # 入口 HTML
└── lodash.min.js   # 工具库
```

---

### 四、生产环境部署


1. 复制新构建产物

```bash
# 将 dist/ 目录内容复制到api-server/ui/下
scp -r dist/* dolphinscheduler@xxx:/path/to/dolphinscheduler/api-server/ui/
```

2. 重启服务（不重启好像也行）

```bash
# 重启 api-server
bash bin/dolphinscheduer-daemon.sh stop api-server
bash bin/dolphinscheduer-daemon.sh start api-server
```

3. 浏览器缓存清除

> 或使用**无痕/隐私模式**访问页面。


## PS

> 可选：关于Datax，建议去[达梦官网](https://eco.dameng.com/download/)下载新的驱动
>
> datax很久没更新，用的Dm7JdbcDriver17-7.6.0.142.jar，连接Dm8可能会有点问题，用DmJdbcDriver8.jar替换掉原来的
>
> 这里可以用AI生成对应的教程，注意AI返回的命名，因为达梦驱动的命名方式变更过，和java的1.8改成8类似，驱动名18改成8
>
> 项目部署比较着急可以忽略
# a-mpx

> A mpx project

## Dev

```bash
# install dep
npm i

# for dev
npm run watch

# for online
npm run build
```

npm script规范 [build|watch]:[dev|prod]:[cross|web|none]

build默认prod，watch默认dev。另单独提供了build:dev和watch:prod，用于单次构建分析看未压缩代码分析问题和持续压缩代码便于大体积项目真机调试。

建议自行调整cross的目标。npm-run-all是为了兼容windows下无法同时执行两个npm script，若不需要转web平台，可考虑去掉。


## MPX 中 Page 页面为啥要使用 Component 构造器来创建页面？小程序中没有 computed、watch 需要自己实现

见：https://developers.weixin.qq.com/community/develop/article/doc/0000a8d54acaf0c962e820a1a5e413



<!-- 运行时依赖替换 -->

## 响应式系统重构————基于 Proxy

getDefaultOptions => initProxy => context.__mpxProxy = new MpxProxy(rawOptions, context) => context.__mpxProxy.created()


```JS
  created () {
    if (__mpx_mode__ !== 'web') {
      setCurrentInstance(this)
      this.initProps()
      this.initSetup()
      this.initData()
      this.initComputed()
      this.initWatch()
      unsetCurrentInstance()
    }

    this.state = CREATED
    this.callHook(CREATED)

    if (__mpx_mode__ !== 'web') {
      this.initRender()
    }

    if (this.reCreated) {
      nextTick(this.mounted.bind(this))
    }
  }
```

### this.initProps()做了什么？？ 

调用 reactive，将其变成响应式对象。

[![zkcatK.md.png](https://s1.ax1x.com/2022/11/14/zkcatK.md.png)](https://imgse.com/i/zkcatK)


```JS
this.initProps()

```


### this.initData()做了什么？？

同样调用 reactive，将其变成响应式对象。

```JS
this.initData()
```


### 暂不关心以下

```JS
    this.initComputed()
    this.initWatch()
    unsetCurrentInstance()
```


### 执行 initRender

```JS
    if (__mpx_mode__ !== 'web') {
      this.initRender()
    }
```
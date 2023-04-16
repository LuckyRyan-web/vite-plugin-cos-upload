## 文档

具体的腾讯 cos 上传方法可以查看 [文档](https://cloud.tencent.com/document/product/436/64980)

使用 cos 上传请注意官网的注意事项：

* Key（文件名）不能以/结尾，否则会被识别为文件夹。

* Key（文件名）同名上传默认为覆盖操作。若您未开启版本控制且不想覆盖云上文件时，请确保上传时的Key不重复。

## 参数说明

```ts
interface Options {
    SecretId: string
    SecretKey: string
    Bucket: string
    Region?: string
    exclude?: RegExp
    /** 并发数 */
    concurrent?: number
    /** key 前缀 */
    prefix?: string
    /** 打包文件目录 */
    distDir?: string
    /** 是否输出日志 */
    log?: boolean
    /** cos 大文件上传方法，不传就用普通方法 */
    cosUploadFileParams?: SliceUploadFileParams
}

```

## 用法

```ts
yarn add vite-plugin-cos-upload -D
```

```ts
// vite.config.ts

import qiniuPlugin from '@ryan-liu/vite-plugin-qiniu-cdn-upload'
import pkg from './package.json'

// config or .env
const cosConfig = {
    SecretId: '',
    SecretKey: '',
    Bucket: '',
    Region: '',
}

/** 项目文件夹地址 */
const basePath = pkg.name

/** cdn 地址 */
const cdnPrePath = ''

function uploadFileToCos(isUpload: boolean) {
    if (!isUpload) {
        return
    }

    return cosPlugin({
        ...cosConfig,
        prefix: basePath,
    })
}

export default defineConfig({
    plugins: [
        ...,
        base: process.env.NODE_ENV == 'production' ? cdnPrePath : `/`,
        uploadFileToCos(process.env.NODE_ENV == 'production')
        ...
    ]
})
```

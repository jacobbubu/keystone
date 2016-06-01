![KeystoneJS](http://keystonejs.com/images/logo.svg)
===================================
Branch `v0.3.x-qiniuImage` 是在 [KeystoneJS](http://keystonejs.com) 0.3.19 版本的基础上添加了上传图片（gif, png, jpeg, bmp）到 [“七牛云存储”](http://www.qiniu.com) 服务的字段类型。

代码基本都修改自原本 CloudinaryImage 字段的实现。

## 样例

样例代码在 https://github.com/jacobbubu/qiniu-keystone 。

## 配置

在你的 keystone 主运行文件中，需要添加你的七牛云存储的配置信息：
```
	...
    'qiniu config': {
        access_key: 'Your Access Key'
        secret_key: 'Your Secret'
        // http host
        host: 'http://7u2r45.com1.z0.glb.clouddn.com'
        // https host, can be omitted
        secure_host: 'https://o813qq1pg.qnssl.com'
    },
```

## Field Option：

```
bucket: 七牛的 bucket 名字，必须配置。
autoCleanup: default: false 移除字段图片的时候否同时删除七牛上的对应文件，否则仅仅删除 mongodb 中的信息。
public_id: 如果指定了名字，那就用 list 中另外一个字段的名字作为图片在七牛上保存的文件名。
filenameAsPublicID: default: false, 如果为 true，则用原始文件名作为七牛上的文件名。
prefix: default: ''，在七牛上的文件名前面加上该前缀，下划线分割。
```

## Schema
写到 MongoDB 的字段内容为，举例：
```
{
    "hash" : "Fm6ZWRpiaCXQM73slQMDErekFTDw",
	"public_id" : "fcd2d0abac153bd886af107b3d841f17.png",
	"originalname" : "Icon-40@2x.png",
	"extension" : "png",
	"mimetype" : "image/png",
	"size" : 2535,
    "url" : "http://7u2r45.com1.z0.glb.clouddn.com/fcd2d0abac153bd886af107b3d841f17.png",
    "thumb_url" : "http://7u2r45.com1.z0.glb.clouddn.com/fcd2d0abac153bd886af107b3d841f17.png?imageView2/1/w/128/h/128",
    "secure_url" : "https://o813qq1pg.qnssl.com/fcd2d0abac153bd886af107b3d841f17.png",
    "secure_thumb_url" : "https://o813qq1pg.qnssl.com/fcd2d0abac153bd886af107b3d841f17.png?imageView2/1/w/128/h/128"
}
```

其中 hash 值是七牛计算的，算法在：http://developer.qiniu.com/article/kodo/kodo-developer/appendix.html#qiniu-etag 。

`secure_url` 和 `secure_thumb_url` 可能没有，如果你没有在 'qiniu config' 中设置 `secure_host` 的话。

`public_id` 的生成时的规则和 CloudinaryIamge 类似，可以指定为 List 中另外一个一个字段的值，或者基于上传的原始文件名，或者一个上传的随机文件名。

当为字段设置了 `prefix` 配置时，`public_id` 的内容就是 `${prefix}_${public_id}`。
##

## License

(The MIT License)

Copyright (c) 2015 Jed Watson

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

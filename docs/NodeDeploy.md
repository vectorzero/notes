# 前端服务器部署

1.将开发完的前端代码，利用webpack打包成静态压缩文件

2.在服务器上，利用pm2负载均衡器来执行以下的代码来开启服务器:

*deploy.js*
```js
const express = require('express');
const proxy = require('http-proxy-middleware');
const app = express();

app.use('/api',proxy({
	target: '', // 后台服务地址
	changeOrigin: true,
	pathRewrite: {
		'^/api': ''
	}
}));

app.use(express.static('dist'));

app.get('*',function(req,res) {
	res.sendfile('./dist/index.html')
});

app.listen(8099,function() {
	console.log('链接成功')
});

```

开启：`pm2 start deploy.js`

停止：`pm2 stop deploy`
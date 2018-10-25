### 遍历文件并显示可copy的文件名列表
```js
#!/usr/bin/env node
const fs = require('fs')

let files = []
function ScanDir(path) {
  let that = this
  if (fs.statSync(path).isFile()) {
    return files.push(path)
  }
  try {
    fs.readdirSync(path).forEach(function (file) {
      ScanDir.call(that, file)
    })
  } 
  catch (e) {
    console.log(e)
  }
}

ScanDir(process.cwd())
console.log(files)

let copyList = '';
files.forEach((item,index) => {
	copyList += `
	<div class="wrap">
	        <div id='copy${index}'>${item}</div>
		<div><button onClick="copyFn('copy${index}')">Copy</button></div>
	</div>
	`
})
let htmlContent = `
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>test</title>
		<style>
			.wrap {
				border: 1px solid #ccc;
				border-top: 0;
				display:flex;
				padding: 5px;
				height: 30px;
				align-items: center;
				width: 500px;
				justify-content: space-between;
			}
			.wrap:nth-child(1) {
				border-top: 1px solid #ccc;
			}
		</style>
	</head>
	<body>
		${copyList}
		<script>
			function copyFn(param) {
				let val = document.getElementById(param);
				window.getSelection().selectAllChildren(val);
				document.execCommand ("Copy");
			}
		</script>
	</body>
</html>
`;

fs.writeFile("path.html", htmlContent, function(err) {
    if(err) {
        return console.log(err);
    }
    console.log("The file was saved!");
});
```

### hupu
```js
'use strict';

// 引入模块
var https = require('https');
var fs = require('fs');
var path = require('path');
var cheerio = require('cheerio');
var html = ''; // 保存抓取到的 HTML 源码
var itemlist = [];  // 保存解析 HTML 后的数据
var templist = '';
var htmlContent = '';
// 爬虫的 URL 信息
const mainUrl = "https://bbs.hupu.com/4847";
// 创建 http get 请求
https.get(mainUrl, function(res) {
    
    // 设置编码
    res.setEncoding('utf-8');

    // 抓取页面内容
    res.on('data', function(chunk) {
        html += chunk;
    });

    res.on('end', function() {
        // 使用 cheerio 加载抓取到的 HTML 代码
        // 然后就可以使用 jQuery 的方法了
        // 比如获取某个 class：$('.className')
        // 这样就能获取所有这个 class 包含的内容
        var $ = cheerio.load(html);
       	
        // 解析页面
        // 每个电影都在 item class 中
        $('.for-list li').each(function() {
            let item = {
                title: $('.titlelink>a', this).text(), // 获取名称
                link: 'https://bbs.hupu.com' + $('.titlelink>a', this).attr('href'), // 获取详情页链接
            };
            itemlist.push(item);
        });
        saveHtml();
        saveData('data.json', itemlist);
    });
}).on('error', function(err) {
    console.log(err);
});


/**
 * 保存数据到本地
 *
 * @param {string} path 保存数据的文件
 * @param {array} itemlist 信息数组
 */
function saveData(path, itemlist) {
    // 调用 fs.writeFile 方法保存数据到本地
    fs.writeFile(path, JSON.stringify(itemlist, null, 4), function(err) {
        if (err) {
            return console.log(err);
        }
        console.log('Data saved');
    });
}

function saveHtml() {
	itemlist.forEach((item,index) => {
		templist += 
		`
			<div class="wrap">
				<a href="${item.link}">${item.title}</a>
			</div>
	    `
	})
	
	htmlContent = 
	`
	<!DOCTYPE html>
	<html>
		<head>
			<meta charset="utf-8">
			<title>test</title>
			<style>
				.wrap {
					border: 1px solid #ccc;
					border-top: 0;
					display:flex;
					padding: 5px;
					height: 20px;
					align-items: center;
					width: 800px;
					justify-content: space-between;
				}
				.wrap:nth-child(1) {
					border-top: 1px solid #ccc;
				}
			</style>
		</head>
		<body>
			${templist}
		</body>
	</html>
    `
	fs.writeFile("path.html", htmlContent, function(err) {
	    if(err) {
	        return console.log(err);
	    }
		console.log("The file was saved!");
	});
}

```

### 命令行的输入输出
```js
const readline = require('readline');
const rl = readline.createInterface(process.stdin, process.stdout);
let firstInput;
let secondInput;

rl.question('what is your name? \t', function(inputI) {
    firstInput = inputI;
    rl.question('How old are you? \t', function(inputII) {
        secondInput = inputII;
		rl.close();
    })
});
```

### 调用cmd命令
```js
const {exec} = require('child_process');

exec('calc',function (err1) {
    if(err1) {
        console.log(1,err1);
    }else {
        exec('errorCommand',function (err2) {
            if(err2) {
                console.log(2,err2)
            }else {
                exec('notepad',function (err3) {
                    if(err3) {
                        console.log(3,err3);
                    }
                })
            }
        })
    }
});
```
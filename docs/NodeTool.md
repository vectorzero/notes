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

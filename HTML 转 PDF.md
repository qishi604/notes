# HTML 转 PDF

[github](https://github.com/marcbachmann/node-html-pdf)

## 安装

```shell
npm install -g html-pdf
```

## 命令行

```shell
html-pdf filename.html filename.pdf
```

## API

```shell
var pdf = require('html-pdf');
pdf.create(html).toFile([filepath, ]function(err, res){
  console.log(res.filename);
});

pdf.create(html).toStream(function(err, stream){
  stream.pipe(fs.createWriteStream('./foo.pdf'));
});

pdf.create(html).toBuffer(function(err, buffer){
  console.log('This is a buffer:', Buffer.isBuffer(buffer));
});


// for backwards compatibility
// alias to pdf.create(html[, options]).toBuffer(callback)
pdf.create(html [, options], function(err, buffer){});
```
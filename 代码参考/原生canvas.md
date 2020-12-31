# uniapp 压缩图片

#### 现在手机拍照的像素都很高，导致图片很大，再上传多张图片时很容易失败，于是研究下压缩图片，最终解决办法如下

```javascript
// 先拿到临时路径
chooseSfzImg(e) {
	    let that = this;
	    console.log(e);
	    uni.chooseImage({
	    count: 1, //默认9
	    sizeType: ['compressed'],
	        success: function(res) {
	                // 压缩
		        that.getImageInfo(res.tempFilePaths[0]).then(res => {
			    that.sfzImg = res;
		        });
	        }
	    });
},
复制代码
// 压缩图片
getImageInfo(src) {
	let _this = this;
	return new Promise((resolve, reject) => {
		uni.showLoading({
		    title: '压缩中...',
		    icon: 'none'
		});
		uni.getImageInfo({
		    src: src,
		    success(res) {
			console.log('压缩前', res);
			let canvasWidth = res.width; //图片原始长宽
			let canvasHeight = res.height;
			let img = new Image();
			img.src = res.path;
			let canvas = document.createElement('canvas');
			let ctx = canvas.getContext('2d');
			canvas.width = canvasWidth / 2;
			canvas.height = canvasHeight / 2;
			console.log(img);
			ctx.drawImage(img, 0, 0, canvasWidth / 2, canvasHeight / 2);
			canvas.toBlob(function(fileSrc) {
			let imgSrc = window.URL.createObjectURL(fileSrc);
			console.log('压缩后', imgSrc);
			resolve(imgSrc);
			uni.hideLoading();
		    });
		}
	       });
	});
},
```
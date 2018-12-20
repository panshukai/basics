## 这里记录了一些常用的css技巧
### 单选框，复选框的样式
  > [unicode](https://link.juejin.im?target=http%3A%2F%2Funicode-table.com%2Fen%2F)这里是Unicode编码
  ```html
  	<style>
		.input input{
			/*display: none;*/
		}
		.input .checkbox input + label:before{
			display: inline-block;
			content: '\2713';
			width: 20px;
			height: 20px;
			border: 1px solid #000;
			text-align: center;
			line-height: 20px;
			color: transparent;
			transition: color .3s;
			vertical-align: middle;
		}
		.input .checkbox input:checked + label:before{
			color: #000;
		}
		.input .radio input + label:before{
			display: inline-block;
			content: '\26AB';
			width: 20px;
			height: 20px;
			border: 1px solid #000;
			border-radius: 50%;
			line-height: 20px;
			text-align: center;
			font-size: 0px;
			transition: fontSize .3s;
			vertical-align: middle;
		}
		.input .radio input:checked + label:before{
			font-size: 14px;
		}
	</style>
    <div class="input">
		<div class="checkbox checkbox1">
			<input type="checkbox" name="checkbox1" id="checkbox1" />
			<label for="checkbox1">checkbox1</label>	
		</div>
		<div class="checkbox checkbox2">
			<input type="checkbox" name="checkbox2" id="checkbox2" />
			<label for="checkbox2">checkbox2</label>	
		</div>
		<div class="radio radio1">
			<input type="radio" name="radio" id="radio1" />
			<label for="radio1">radio1</label>
		</div>
		<div class="radio radio2">
			<input type="radio" name="radio" id="radio2" />
			<label for="radio2">radio2</label>
		</div>
	</div>
  ```
  
### position sticky用法
  > 常用于吸顶的导航条（同时达到了static和fixed的效果）
  > 原理：父元素为基准，相对距离超过设定值为fixed，未超过为static。sticky只会存在top,bottom,left,right存在时才会生效。
  ```html
  	<style>
		.head{
			width: 100%;
			height: 30px;
			background: #00f;
		}
		.header{
			position: sticky;
			top: 0px;
			width: 100%;
			background: rgba(0,0,0,0.5);
		}
		p{
			height: 15vh;
		}
	</style>
    <div class="head"></div>
	<div class="header">
		aaaaaa
	</div>
	<p>ssss</p>
	<p>ssss</p>
	<p>ssss</p>
	<p>ssss</p>
	<p>ssss</p>
	<p>ssss</p>
	<p>ssss</p>
	<p>ssss</p>
	<p>ssss</p>
	<p>ssss</p>
	<p>ssss</p>
  ```
  
### webkit自定义滚动条样式
  ```html
    <style>
		.test{
		    width: 30px;
		    height: 200px;
		    overflow: auto;
		    margin: 5px;
		    border: none;
		}
		.scrollbar{
		    width: 50px;
		    height: 300px;
		    margin: 0 auto;
		 
		}
		.test-1::-webkit-scrollbar {/*滚动条整体样式*/
		        width: 10px;     /*高宽分别对应横竖滚动条的尺寸*/
		        height: 1px;
		    }
		.test-1::-webkit-scrollbar-thumb {/*滚动条里面小方块*/
		        border-radius: 10px;
		         -webkit-box-shadow: inset 0 0 5px rgba(0,0,0,0.2);
		        background: #535353;
		    }
		.test-1::-webkit-scrollbar-track {/*滚动条里面轨道*/
		        -webkit-box-shadow: inset 0 0 5px rgba(0,0,0,0.2);
		        border-radius: 10px;
		        background: #EDEDED;
		    }
	</style>
	<div class="test test-1">
    	<div class="scrollbar"></div>
	</div>
  ```

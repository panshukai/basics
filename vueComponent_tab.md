# vue_tab组件
### 实现方案：通过nav传递和内容分发的形式，数据传递使用provide/inject<br/>优点：nav和content分开的形式，可以容纳更为复杂的content
```vue
//调用页面
<template>
  <div id="app">
    <Tabs>
        <Tab label="tab1" id="tab1" disabled>content1</Tab>
        <Tab label="tab2" id="tab2">content2</Tab>
        <Tab label="tab3" id="tab3">content3</Tab>
    </Tabs>
  </div>
</template>
<script>
import Tabs from './components/Tabs'
import Tab from './components/Tab'
export default {
  name: 'App',
  components: {
    Tabs,
    Tab
  }
}
```
```vue
//Tabs页面
<template>
	<div class="tab-wrap">
		<ul :class="['tab-head']">
			<li v-for="(item,index) in childrens" :class="[{'active':item.id == activeId},{'disabled':item.disabled}]" @click="!item.disabled&&handleClick(item)">
				<span>{{item.label}}</span>
			</li>
		</ul>
		<div class="tab-content">
			<slot></slot>
		</div>
	</div>
</template>
<script>
	export default{
		data(){
			return {
				childrens:[],
				activeId:''
			}
		},
		mounted(){
			this.getChildrens();
		},
		provide(){
			return {
				Tabs:this
			}			
		},
		methods:{
			getTabs(){
				//获取子tab component
				return this.$children.filter(function(item){
					return item.$options._componentTag == 'Tab';
				})
			},
			getChildrens(){
				var _this = this;
				//初始化 子tab对象&activeId
				this.getTabs().forEach(function(item,index){
					_this.childrens.push({
						label:item.label,
						disabled:item.disabled,
						id:item.id
					});
					if(!item.disabled&&_this.activeId == ''){
						_this.activeId = item.id;
					}					
				})
			},
			handleClick(item){
				if(this.activeId != item.id){
					this.activeId = item.id;
				}
			}
		}
	}
</script>
<style lang="less">
	ul{
		display: flex;
		justify-content: center;
	}
	li{
		list-style: none;
		padding: 10px;
		cursor: pointer;
		&.active{
			color: #f00;
		}
		&.disabled{
			pointer-events: none;
			color: #ccc;
		}
	}
</style>
```
```vue
//Tab页面
<template>
	<div v-if="isActive">
		<slot></slot>
	</div>
</template>
<script>
	export default{
		inject:['Tabs'],
		methods:{

		},
		computed:{
			isActive(){
				return this.Tabs.activeId == this.id;
			},
		},
		props:{
			label: { type: String, required: true },
		    id: { type: [String, Number], required: true },
		    disabled: { type: Boolean, default: false },
		    closable: { type: Boolean, default: false }
		},
		mounted(){
			console.log(this.Tabs == this.$parent)
			//console.log(this.Tabs.activeId,this.id)
		}
	}
</script>
```

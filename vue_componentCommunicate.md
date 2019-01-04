# VUE相关
## vue组件通信
  <br/>需求：点击按钮，弹出弹层，弹层内部按钮，点击关闭
  1. v-on + $emit + prop
  ```vue
  //父页面
    <button @click='openLayer'>openLayer</button>
    <Layer :showLayer='showLayer' @greet='greet'></Layer>
    import Layer from './components/Layer'
    export default {
      name: 'App',
      components: {
        Layer
      },
      data(){
        return {
          showLayer:false
        }
      },
      methods:{
        openLayer:function(){          
          this.showLayer = !this.showLayer;
        },
        greet:function(val){
          this.showLayer = val;
        }
      }
    }
  //layer页面
    <template>
        <div class="layer" v-show="showLayer">
            <div class="content">
                <span @click='click'>close</span>
            </div>
        </div>	
    </template>
    <script>
        export default{
            //获取父组件props的状态
            props:['showLayer'],
            methods:{
                click:function(){
                    //通知父组件去更改状态
                    this.$emit('greet',false);
                }
            }
        }
    </script>
  ```
  2. ref
  ```vue
    //父页面
    <template>
      <div>
        <Layer ref='layer'></Layer>
        <button @click='openLayer'>openLayer</button>
      </div>
    </template>
    <script>
    import Layer from './components/Layer'
    export default {
      name: 'App',
      components: {
        Layer
      },
      methods:{
        openLayer:function(){
          this.$refs.layer.click();
        }
      }
    }
    </script>
    //layer页面
    <template>
        <div class="layer" v-show="showLayer">
            <div class="content">
                <span @click='click'>close</span>
            </div>
        </div>	
    </template>
    <script>
        export default{
            data(){
                return {
                    showLayer:false
                }
            },
            methods:{
                click:function(){
                    this.showLayer = !this.showLayer;
                }
            }
        }
    </script>
  ```
  3. v-model
  ```vue
    //父页面
    <template>
      <div>
        <Layer v-model="showLayer"></Layer>
        <button @click='openLayer'>openLayer</button>
      </div>
    </template>
    <script>
    import Layer from './components/Layer'
    export default {
      name: 'App',
      components: {
        Layer
      },
      data(){
        return {
          showLayer:false
        }
      },
      methods:{
        openLayer:function(){
          this.showLayer = !this.showLayer;
        }
      }
    }
    </script>
    //layer页面
    <template>
        <div class="layer" v-show="value">
            <div class="content">
                <span @click='click'>close</span>
            </div>
        </div>	
    </template>
    <script>
        export default{
            //示例：如若发生冲突，可自定义修改model内部的prop和event
            /*model: {
              prop: 'show',
              event: 'close'
            },*/
            //v-model默认绑定了prop:"value",event:"input"
            props:['value'],
            methods:{
                click:function(){
                    this.$emit('input',false);
                }
            }
        }
    </script>
  ```
  4. sync + $emit
  > 语法糖解析：```<demo :foo.sync="something"></demo>``` => ```<demo :foo="something" @update:foo="val => something = val"></demo>```
  ```vue
    //父页面
    <template>
      <div>
        <Layer :showLayer.sync="showLayer"></Layer>
        <button @click='openLayer'>openLayer</button>
      </div>
    </template>
    <script>
    import Layer from './components/Layer'
    export default {
      name: 'App',
      components: {
        Layer
      },
      data(){
        return {
          showLayer:false
        }
      },
      methods:{
        openLayer:function(){
          this.showLayer = !this.showLayer;
        }
      }
    }
    </script>
    //layer页面
    <template>
        <div class="layer" v-show="showLayer">
            <div class="content">
                <span @click='click'>close</span>
            </div>
        </div>	
    </template>
    <script>
        export default{
            props:['showLayer'],
            methods:{
                click:function(){
                    this.$emit('update:showLayer',false);
                }
            }
        }
    </script>
  ```
## 兄弟组件通信
  1. v-on + $on + $emit
  > 原理：B -> A -> C(A为父组件，B、C为子组件)
  ```vue
  //父组件A
  <template>
    <div id="app">
      <first @parent="first"></first>
      <second ref="second"></second>
    </div>
  </template>
  <script>
  import first from './components/first'
  import second from './components/second'
  export default {
    name: 'App',
    components: {
      first,
      second
    },
    methods: {
      first(msg){
          this.$refs.second.$emit('second',msg);
      }
    }
  }
  </script>
  //子组件A first
	<template>
		<div>
			<button @click='click'>click</button>
		</div>
	</template>
	<script>
		export default{
			methods:{
				click(){
					this.$emit('parent','from first');
				}
			}
		}
	</script>
  //子组件B second
	<template>
		<div>
			<p>{{data}}</p>
		</div>
	</template>
	<script>
		export default{
			data(){
				return{
					data:'111'
				}
			},
			mounted(){
				this.$on('second',function(msg){
					console.log(msg);
					this.data = msg;
				})
			}
		}
	</script>
  ```
  2. 中央事件
  > 此方法不仅适用于父子通信，兄弟通信，同样适用于深层次组件通信
  ```vue
  //bus.js
  import Vue from 'vue';
  export default new Vue();
  //父组件A
  <template>
    <div id="app">
      <first></first>
      <second></second>
    </div>
  </template>
  <script>
  import first from './components/first'
  import second from './components/second'
  export default {
    name: 'App',
    components: {
      first,
      second
    }
  }
  </script>
  //子组件A first
	<template>
		<div>
			<button @click='click'>click</button>
		</div>
	</template>
	<script>
    import Bus from './static/bus'
		export default{
			methods:{
				click(){
					Bus.$emit('parent','from first');
				}
			}
		}
	</script>
  //子组件B second
	<template>
		<div>
			<p>{{data}}</p>
		</div>
	</template>
	<script>
    import Bus from './static/bus'
		export default{
			data(){
				return{
					data:'111'
				}
			},
			mounted(){
        var _this = this;
				Bus.$on('parent',function(msg){
					console.log(msg);
					_this.data = msg;
				})
			}
		}
	</script>
  ```

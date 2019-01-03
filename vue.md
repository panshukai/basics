# VUE相关
## vue组件通信
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
        <div class="layer" :class="{'active':showLayer}">
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
        <div class="layer" :class="{'active':showLayer}">
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

<template>
  <div id="app">
    <h1 v-text="title"></h1>
    <input v-model="newItems" @keyup.enter="addNewItem" placeholder="输入你要做的事件"/>
    <ul>
      <li class="todoItem" v-for="item in items"  v-bind:class="{finished: item.isFinished}" >
        <input type="checkbox"  class="toggle"  v-on:click="toggleFinished(item)" v-model="">
        {{ item.label }}
        <button class="destroy" v-on:click="removeTodo(item)"></button>
      </li>
    </ul>
  </div>
  
</template>

<script>
import Store  from './store'
// import ComponentsA from './components/ComponentsA'
export default {
  // components: { ComponentsA },
  data() {
      return {
          title: 'TodoList',
          items:Store.fetch(), //从 LocalStore 中取值
          newItems:'',
      }
  },
  watch: {
    items: {
      handler: function(items) {
        Store.save(items)
      },
      deep: true   //深复制
    }
  },
  methods: {
            toggleFinished: function(item) {
              // console.log(item)
              item.isFinished = !item.isFinished //状态反转
            },
            addNewItem:function() {
              this.items.push({
                label: this.newItems,  //事件名
                isFinished: false      //事件状态（完成或未完成）
              }) 
              this.newItems = '' //敲下回车后，清空输入内容
              Store.save(); //把数据存入 LocalStroge
            },
            removeTodo:function(item){
              // console.log()
              this.items.$remove(item);
            }
  }
}
</script>

<style>
html {
  height: 100%;
}

body {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
}

ul ,li{
  margin:0;
  padding:0;
}

li {
  list-style-type:none;
}

li:hover .destroy {
    display:inline;
}

li:hover .destroy:after {
  content:"x";
  color:red;
}

#app {
  color: #2c3e50;
  margin-top: -100px;
  max-width: 600px;
  font-family: Source Sans Pro, Helvetica, sans-serif;
  text-align: center;
}

#app a {
  color: #42b983;
  text-decoration: none;
}

.todoItem{
  position: relative;
	font-size: 24px;
	border-bottom: 1px solid #ededed;
}

.toggle {
  text-align: center;
	width: 40px;
	/* auto, since non-WebKit browsers doesn't support input styling */
	height: auto;
	/*position: absolute;*/
	top: 0;
	bottom: 0;
	margin: auto 0;
	border: none; /* Mobile Safari */
	-webkit-appearance: none;
	appearance: none;
}

.finished {
  text-decoration:line-through;
  color:#9e9e9e;
}

.destroy {
    display:none;
    float:right;
}



</style>

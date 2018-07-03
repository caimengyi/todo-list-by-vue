<template>
  <div id="app" v-bind:style="appStyle">
    <div id="header" v-bind:style="headerStyle">
      <div v-if="!skinShow" class="icon icon-menu" v-on:click="showSidebar"></div>
      <div v-if="skinShow" class="icon icon-cheveron-left" v-on:click="moveOut"></div>
      <h1>Todo-List </h1>
    </div>

    <div id="sidebar" v-bind:style="sidebarStyle">
        <ul>
            <li class="icon-options icon-time" v-on:click="changetoTodo">待做事项</li>
            <li class="icon-options icon-clipboard" v-on:click="changetoDone">完成事项</li>
            <li class="icon-options icon-apparel" v-on:click="changetoSkin">皮肤</li>
            <li class="icon-options icon-information-outline" v-on:click="changetoAbout">关于</li>
        </ul>
    </div>

    <div id="addNewItems>">

      <div class="buttons">
        <div v-if="confirmButton" id="confirm" v-on:click="confirmNew">确认</div>
        <div v-if="addButton" class="add" v-on:click="showNewItem">+</div>
        <div v-else class="add" v-on:click="showNewItem">x</div>
      </div>

      <div id="addInput" v-bind:style="newItemStyle">
         <input @blur="focusState = false" v-focus="focusState" id="newItem" v-model="newItem" placeholder="添加新任务" v-on:keyup.enter="addNew()"/>
      </div>

      <div class="changeOptions">
          <ul v-if="type === 'todo'" class="todoLists" v-bind:style="ulStyle">
              <li v-for="(item,index) in items">
                {{item.text}}
                <span class="delete" v-on:click="deleteItems(index)">删除</span>
                <span class="complete" v-on:click="finishClick(index)">完成</span>
              </li>
          </ul>
          <ul v-if="type === 'done'" class="doneLists" v-bind:style="ulStyle">
              <li v-for="(doneitem,index) in finishedItems" v-bind:class="{ finished : doneitem.isFinished }">
                {{doneitem.text}}
                <span class="delete" v-on:click="deleteFinished(index)">删除</span>
              </li>
          </ul>
          <ul class="changeskin" v-bind:style="skinStyle">
              <li v-on:click="changeGreen">小清新<span class="green"></span><span class="lightgreen"></span></li>
              <li v-on:click="changeRed">热情火<span class="red"></span><span class="lightred"></span></li>
              <li v-on:click="changeBlue">天空蓝<span class="blue"></span><span class="lightblue"></span></li>
          </ul>

          <div class="aboutme" v-bind:style="aboutStyle">
            <h2>这是一个任务清单</h2>
            <p>我的第一个Vue程序</p>
            <p>制作人：@2018 <a href="###">Sherry</a></p>
          </div>

      </div>

    </div>       
  </div>
</template>

<script>
export default {
  name: 'App',
  data: function() {
    return {
      items: JSON.parse(window.localStorage.getItem('todoList') || '[]'),
      finishedItems:JSON.parse(window.localStorage.getItem('doneList') || '[]'),
      newItem: "",
      focusState:false,
      addButton:true,
      confirmButton:false,
      type: 'todo',
      skinShow: false,
      sidebarStyle: {
        display:'none'
      },
      newItemStyle: {
        display: 'none'
      },
      appStyle: {
        backgroundColor:'#ececeb',
      },
      headerStyle: {
        display:'block',
        backgroundColor:'#61b292'
      },
      ulStyle: {
        display:'block',
        opacity: 1
      },
      skinStyle: {
        display: 'none'
      },
      aboutStyle: {
        display:'none'
      }

    }
  },
  methods: {
    toggleFinish: function(item){
        item.isFinished = !item.isFinished
    },
    confirmNew: function(item) {
      if(this.newItem !== ''){
          this.items.push({
            text: this.newItem,
            isFinished: false
          });
          this.newItem = "";
          this.newItemStyle.display = 'none';
          this.appStyle.backgroundColor = '#ececeb';
          this.confirmButton = false;
          this.addButton = true;
          this.headerStyle.display = 'block';
          this.ulStyle.display = 'block';
          this.type='todo';
      }

    },
    showSidebar: function(){
      if(this.sidebarStyle.display === 'none'){
        this.sidebarStyle.display = 'block';
        this.appStyle.backgroundColor = '#aaaaaa';
        this.ulStyle.opacity = 0.1;
      }else{
        this.sidebarStyle.display = 'none';
        this.appStyle.backgroundColor = '#ececeb';
        this.ulStyle.opacity = 1;
      }
      
    },
    showNewItem: function(){
      if(this.newItemStyle.display === 'none'){
        this.newItemStyle.display = 'block';
        this.addButton = false;
        this.confirmButton = true;
        this.headerStyle.display = 'none';
        this.appStyle.backgroundColor = 'white';
        this.ulStyle.display = 'none';
        this.focusState = true;
        this.sidebarStyle.display = 'none';
      }else{
        this.newItemStyle.display = 'none';
        this.addButton = true;
        this.confirmButton = false;
        this.headerStyle.display = 'block';
        this.appStyle.backgroundColor = '#ececeb';
        this.ulStyle.display = 'block';
      }
    },
    deleteItems: function(index){
      this.items.splice(index,1);
    },
    deleteFinished: function(index){
      this.finishedItems.splice(index,1);
    },
    finishClick: function(index){
      this.finishedItems.push({
            text: this.items[index].text,
            isFinished: true
          });
      this.items.splice(index,1);
    },
    changetoTodo:function(){
      this.type = 'todo';
      this.sidebarStyle.display = 'none';
      this.appStyle.backgroundColor = '#ececeb';
      this.ulStyle.opacity = 1;
    },
    changetoDone: function(){
      this.type= 'done';
      this.sidebarStyle.display = 'none';
      this.appStyle.backgroundColor = '#ececeb';
      this.ulStyle.opacity = 1;
    },
    changetoSkin: function(){
      this.skinStyle.display = 'block';
      this.ulStyle.opacity = 0.1;
      this.sidebarStyle.display = 'none';
      this.skinShow = true;
    },
    changetoAbout: function(){
      this.aboutStyle.display = 'block';
      this.ulStyle.opacity = 0.1;
      this.sidebarStyle.display = 'none';
      this.skinShow = true;
    },
    moveOut: function(){
      this.skinStyle.display = 'none';
      this.aboutStyle.display='none';
      this.ulStyle.opacity = 1;
      this.appStyle.backgroundColor = '#ececeb';
      this.skinShow = false;
    },
    changeGreen: function(){
      this.headerStyle.backgroundColor = '#61b292'
    },
    changeBlue: function(){
      this.headerStyle.backgroundColor = '#11cbd7'
    },
    changeRed: function(){
      this.headerStyle.backgroundColor = '#fa4659'
    }

  },
  directives: {
    focus: {
      update: function (el, {value}) {
        if (value) {
          el.focus()
        }
      }
    }
  },
  watch: {
    items:{
      handler: function(items){
        window.localStorage.setItem('todoList',JSON.stringify(items))
      },
      deep: true
    },
  finishedItems:{
      handler: function(finishedItems){
        window.localStorage.setItem('doneList',JSON.stringify(finishedItems))
      },
      deep: true
    }
  }
}
</script>

<style>

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
  background-color:#ececeb;
  margin:0px;
  position:absolute;
  top:0px;
  left:0px;
  width:100%;
  height:100%;
}


#header {
  width:100%;
  height:80px;
  background-color:#B8DFF0;
  position:absolute;
  top:0px;
  left:0px;
  text-align: center;
  z-index:70;
}

#header h1 {
  font-size:32px;
  color:white;
}

#sidebar {
  position:absolute;
  top:0px;
  left:0px;
  width:250px;
  height:100%;
  background-color:white;
  display:block;
  z-index:50;
}

#sidebar ul{
  text-align:left;
  padding:80px 0px 0px 0px;
}


#sidebar ul li{
  list-style:none;
  padding:25px;
  font-size:18px;
  text-align:left;
  font-color:#ccc;
  margin:0px;
}

#sidebar ul li:hover{
  background-color:#bff4ed;
}

.buttons {
  position:absolute;
  right:5%;
  bottom:20px;
  text-align:center;
  z-index:200;
}

.add {
  width:70px;
  height:70px;
  border-radius:50%;
  background-color:#f9ce00;
  text-align:center;
  line-height:70px;
  font-size:50px;
  font-weight:10px;
  color:white;
  cursor:pointer;
}

#confirm {
  width:60px;
  height:60px;
  border-radius:50%;
  background-color:#7f7f7f;
  text-align:center;
  line-height:60px;
  font-size:18px;
  font-weight:10px;
  color:white;
  cursor:pointer;
  margin-bottom:40px;
}

#addInput {
  width:100%;
  height:100%;
  position:absolute;
  top:0px;
  left:0px;
  z-index:100;
}

#newItem {
  position:relative;
  left:5%;
  width:90%;
  height:70px;
  border:none;
  font-size:24px;
  top:120px;
  border-bottom:#aaa 3px solid;
  outline:none;
}

.todoLists,.doneLists {
  position:relative;
  top:90px;
}

.todoLists li,.doneLists li{
  position:relative;
  left:2%;
  width:90%;
  height:60px;
  line-height:60px;
  font-size:20px;
  background-color:white;
  padding:10px 20px; 
  list-style:none;
  margin:15px 0px;
}


.finished {
  color:#c1c1c1;
  text-decoration:line-through;
}

.delete,.complete {
  position:absolute;
  right:30px;
  font-size:16px;
  cursor:pointer;

}

.todoLists .delete {
  right:70px;
}

.doneLists .delete{
  right:30px;
}

.changeskin,.aboutme {
  position:absolute;
  top:160px;
  padding:30px 60px;
  background-color:white;
  width:80%;
  left:4%;
  font-size:18px;
  margin:0px;
}

.changeskin li {
  position:relative;
  list-style:none;
  width:80%;
  padding:15px 40px;
  margin:0 auto;
  cursor:pointer;
}

.changeskin li span {
  padding:0px 18px;
  margin:20px 0px;
  position:relative;
  left:70%;
}

.green{
  background-color:#61b292;
  border-radius:40% 0 0 40%;

}

.lightgreen{
  background-color:#c6f1e7;
  border-radius:0 40% 40% 0;
}

.red{
  background-color:#fa4659;
  border-radius:40% 0 0 40%;
}

.lightred{
  background-color:#ffe6eb;
  border-radius:0 40% 40% 0;
}

.blue{
  background-color:#11cbd7;
  border-radius:40% 0 0 40%;
}

.lightblue{
  background-color:#c7f3ff;
  border-radius:0 40% 40% 0;
}

.aboutme {
  text-align:center;
}

.aboutme a{
  text-decoration:none;
}

@font-face {
  font-family: 'icomoon';
  src:  url('fonts/icomoon.eot?9pala9');
  src:  url('fonts/icomoon.eot?9pala9#iefix') format('embedded-opentype'),
    url('fonts/icomoon.ttf?9pala9') format('truetype'),
    url('fonts/icomoon.woff?9pala9') format('woff'),
    url('fonts/icomoon.svg?9pala9#icomoon') format('svg');
  font-weight: normal;
  font-style: normal;
}

.icon {
  font-family: 'icomoon' !important;
  speak: none;
  font-style: normal;
  font-weight: normal;
  font-variant: normal;
  text-transform: none;
  line-height: 80px;
  font-size:32px;
  position:absolute;
  left:5%;

  /* Better Font Rendering =========== */
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.icon-cheveron-left:before {
  content: "\e900";
  color:white;
}
.icon-menu:before {
  content: "\e901";
  color:white;
}

@font-face {
  font-family: 'icomoon-options';
  src:  url('fonts/options.eot');
  src:  url('fonts/options.eot#iefix') format('embedded-opentype'),
    url('fonts/options.ttf') format('truetype'),
    url('fonts/options.woff') format('woff'),
    url('fonts/options.svg#icomoon') format('svg');
  font-weight: normal;
  font-style: normal;
}

.icon-options {
  /* use !important to prevent issues with browser extensions that change fonts */
  font-family: 'icomoon-options' !important;
  speak: none;
  font-style: normal;
  font-weight: normal;
  font-variant: normal;
  text-transform: none;
  line-height: 1;

  /* Better Font Rendering =========== */
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.icon-apparel:before {
  content: "\e900";
  padding:0 15px;
}
.icon-clipboard:before {
  content: "\e901";
  padding:0 15px;
}
.icon-information-outline:before {
  content: "\e902";
  padding:0 15px;
}
.icon-time:before {
  content: "\e903";
  padding:0 15px;
}

</style>

<template>
  <div id="app">
    <div class="header">
      <p>ToDoList</p>
      <input class='nav' type="text" v-model="todo" @keydown="addData($event)"  placeholder="添加ToDoList"/>

    </div>

    <br>
    <h2>正在进行</h2>     <div class="count"  > {{number}}</div>

    <ul class="ing">

      <li v-for="(item,key) in list" v-if="!item.checked">
        <el-checkbox v-model="item.checked"  @change="saveData()"></el-checkbox>   {{item.title}}
        <el-button id="but" type="primary" icon="el-icon-delete" @click="deleteData(key)" size="mini"></el-button>
      </li>

    </ul>
    <br>
    <h2>已经完成</h2>      <div class="count1"> {{count1}}</div>
    <ul class="done">
      <li v-for="(item,key) in list" v-if="item.checked">
        <el-checkbox v-model="item.checked"  @change="saveData()"  ></el-checkbox>{{item.title}}
        <el-button id="but2" type="primary" icon="el-icon-delete" @click="deleteData(key)" size="mini"></el-button>
      </li>

    </ul>
    <br>

    <el-button id='but3' @click="clear()" type="text">clear</el-button>
  </div>
</template>

<script>
  import storage from './model/storage.js';
export default {
  name: 'app',
  data () {
    return {
      msg: '',
      todo:'',
      list:[],
      number:0,
      count1:0
    }
  },
  methods:{
    addData(e){
      if(e.keyCode==13){
        this.list.push({title:this.todo,
          checked:false});
        this.todo='';
      }
      this.shushu();
      storage.set('list',this.list);
    },
    deleteData(key){
      this.list.splice(key,1);
      this.shushu();
      storage.set('list',this.list);

    },
    clear(){
      var len=this.list.length;
      this.list.splice(0,len);
      localStorage.clear();


    },
    saveData() {
      this.shushu();
      storage.set('list', this.list)
    },
    shushu(){
      var len=this.list.length;
      var j=0;
      var k=0;
      for(var i=0;i<len;i++)
      {
        if(this.list[i].checked)
        {
          k++;
        }
        else{
          j++;
        }
      }
      this.count1=k;
      this.number=j;
    }
  },
  mounted(){
    var list=storage.get('list');
    if(list)
    {
      this.list=list;
    }
    this.shushu();

  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  height:800px;
  background: #ccc;
}
  .header{
    height:60px;
    line-height:17px;
    font-size: 26px;
    background: #000;
    display: flex;
    border-radius:5px;
  }

  .header p{
    color:white;
    margin-left: 30%;

  }
.header input{
  height:22px;
  width:350px;
  margin-top:19px;
  margin-left:10%;
  border-radius: 5px;
  position: relative;
}
#but,#but2{
  position: absolute;
  display:inline-block;
  left: 92%;
  top:3px;
}
  h2 ,ul {
    margin-left: 30%;
    padding:0;
  }
  .count{
    margin-left:71%;
    margin-top: -3%;
    width:1rem;
    height:1rem;
    text-align: center;
    top:8rem;
    border-radius:50%;
    background: cornflowerblue;
    color:#fff;

  }
.count1{
  margin-left:71.0%;
  margin-top: -3%;
  width:1rem;
  height:1rem;
  text-align: center;
  top:8rem;
  border-radius:50%;
  background: cornflowerblue;
  color:#fff;}
.ing li{

  list-style: none;
  height:32px;
  line-height:32px;background: #fff;
  position:relative;
  margin-bottom: 10px;
  padding:0 ;
  width:600px;
  border-radius:5px;
  border-left: 5px solid darkslategray;
}
  .done li{
    list-style: none;
    height:32px;
    line-height:32px;
    background: #fff;
    position:relative;
    margin-bottom: 10px;
    padding:0 ;
    width:600px;
    border-radius:5px;
    border-left: 5px solid darkslategray;
    opacity: 0.5;

  }

  #but3{
    margin-left: 50%;
    color:gray;
    border-radius: 5px;
    opacity: 0.5;
  }
</style>

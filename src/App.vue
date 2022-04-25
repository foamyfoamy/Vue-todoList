<template>
  <div class="todoListBox">
    <div class="header">
    <div class="header-left">
      <div>+</div>
      <h2>Todo List</h2>
    </div>
    <div class="header-right">
      <button class="allSelect" @click="handleAllSelect">select_all</button>
      <button class="addOne" @click="handleAdd">add_one</button>
    </div>
    </div>
    <div class="content">
      <div class="contentItems" 
      v-for="(item,index) in todoList" :key=item.id>
        <div class="contentLeft" @click="handleSelect(index,item.id)">
          <span
            :style="item.isCheck ? 'opacity : 1' : 'opacity : 0'"
          ></span>
         </div>

        <input 
        type="text" 
        class="contentInput" 
        v-model="item.text"
        :disabled="item.isCheck"
        :class="item.isCheck ? 'LineThrough' : '' "
        @blur="handleBlur"
        ref="inputBox"
        >

        <div class="contentRight">
        <p>{{item.time}}</p>
        <button @click="handleDelItem(index,item.id)">delete</button>
      </div>
    </div>
    </div>
  </div>
</template>

<script>
import dayjs from "dayjs"
export default {
  name: 'App',
  data(){
    return{
      todoList:[]
    }
  },
  created(){
    let getList = JSON.parse(window.localStorage.getItem("listTodo1"))
    if (getList === null) {
      this.todoList = [{
        id: this.randomID(),
        text: "请点击上方的添加按钮添加事件",
        isSelected: false,
        time: dayjs(new Date).format('YY-MM-DD HH:mm')
      }]
    } else {
      this.todoList = getList
    }
  },
  methods: {
    handleAdd(){
      this.todoList.unshift({
          id:this.randomID(),
          isCheck:false,
          text:"",
          time:dayjs(new Date).format('YY-MM-DD HH:mm'),
      })

      const inputLength = this.todoList.length - 1
      // 获取焦点
      this.$nextTick(() => {
        this.$refs.inputBox[inputLength].focus()
      })

    },
    handleDelItem(index,id){
      if(this.todoList[index].id === id){
        this.todoList.splice(index,1)
        this.storage()
      }
    },
    handleSelect(index,id){
      if(this.todoList[index].id === id){
        this.todoList[index].isCheck = !this.todoList[index].isCheck
        this.storage()
      }
    },
    handleAllSelect(){
      this.todoList.forEach(item => {
        item.isCheck = !item.isCheck
        this.storage()
      })
    },
    storage(){
      window.localStorage.setItem('listTodo1',JSON.stringify(this.todoList))
    },
    handleBlur(){
      this.storage()
    },
    randomID(){
      return Number(Math.random().toString().substr(3,100) + Date.now()).toString(36);
    }
  }

}
</script>

<style lang="less">
button{
  padding: 5px 10px;
  border: none;
  border-radius: 4px;
  margin-left: 10px;
 } 

.todoListBox{
  width: 800px;
  height: 600px;
  background-color: #3C3E4F;
  border-radius: 10px;
  position: absolute;
  top:50%;
  left:50%;
  transform: translate(-50%,-50%);
  padding: 20px;
  box-sizing: border-box;
  color: #fff;
  .header{
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-bottom: #ccc 1px solid;
    .header-left{
      display: flex;
      align-items: center;
      div{
        width: 50px;
        height: 50px;
        border-radius: 50%;
        background: #9999E6;
        font-size: 30px;
        text-align: center;
        margin-right: 15px;
      }
    }


    .header-right{
      .allSelect{
          background: #C43F38;
        }
      .addOne{
          background: #70B870;
        }
    }
  }

  .content{
    margin-top: 20px;
    height: 450px;
    overflow-y: scroll;


    .contentItems{
      width: 100%;
      display: flex;
      justify-content: space-between;
      background: #6B6F70;
      align-items: center;
      border-radius: 8px;
      padding: 10px 20px;
      box-sizing: border-box;
      margin-top: 10px;
      .contentLeft{
        width: 30px;
        height: 30px;
        border: 1px solid #000;
        border-radius: 50%;
        position: relative;
        span{
          display: inline-block;
          width: 20px;
          height: 20px;
          background: #9999E6;
          border-radius: 50%;
          position: absolute;
          top:50%;
          left:50%;
          transform: translate(-50%,-50%);
        }
      }
      .contentInput{
        flex:1;
        margin: 0 10px;
        outline: none;
        background: transparent;
        border: none;
        border-bottom: 1px solid #ccc;
        padding: 5px;
        color: #ccc;
      }

      .LineThrough{
        color: rgba(255, 255, 255, 0.3);
        text-decoration: line-through rgba(255, 255, 255, 0.8);
      }

      .contentRight{
        display: flex;
        align-items: center;
        button{
          background: #C43F38;
        }
      }
    }
  }
}
</style>

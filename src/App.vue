<template>
  <div id="todoList">
    <div class="top">
      <h1>Todo List</h1>
      <p>Get things done, one item at a time</p>
      <p class="line"></p>
    </div>
    <template v-if="todoByStatus.length">
      <transition-group name="animation" tag="ul">
      <li class="todo" v-for="todoList in todoByStatus" :key="todoList">
        <p :class="{ 'todo-line': todoList.isActive }">{{ todoList.text }}</p>
        <div class="action">
          <button
            @click="checkAction(todoList)"
            :class="{ 'change-bgr': todoList.isActive }"
          >
            <i class="fa" :class="{ 'fa-check': todoList.isActive }"></i>
          </button>
          <button @click="removeHandle(todoList)">
            <i class="fa fa-remove"></i>
          </button>
        </div>
      </li>
    </transition-group>
      <div class="sort">
        <p>Move done items at the end</p>
        <div class="slider" @click="isToggle = !isToggle">
          <div
            class="slider-circle"
            :class="{ 'slider-circle-on': isToggle }"
          ></div>
        </div>
      </div>
    </template>
    <div class="todo-empty" v-else>
      <p>The todo list is empty</p>
    </div>
    <div class="bottom">
      <p>Add the todo list</p>
      <div class="add-todo">
        <input class="add-item" type="text" v-model="value" />
        <button @click="addItemHandle()">
          <p>ADD ITEM</p>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      todoLists: [
        { id: 1, isActive: true, text: "Learn VueJs" },
        { id: 2, isActive: false, text: "Learn PHP" },
        { id: 3, isActive: true, text: "Learn React" },
      ],
      isToggle: false,
      value: "",
    };
  },
  computed: {
    todoByStatus: function() {
        if(!this.isToggle) {
          return this.todoLists
        }
        var sortedTodoList = []
        sortedTodoList = this.todoLists
        let listCheck = this.todoLists.filter(item => item.isActive === true)
        let listNotCheck = this.todoLists.filter(item => item.isActive === false)
        sortedTodoList = [...listCheck, ...listNotCheck]
        return sortedTodoList
    }
  },
  methods: {
    checkAction(todoList) {
      todoList.isActive = !todoList.isActive;
    },
    removeHandle(todoList) {
      this.todoLists = this.todoLists.filter((item) => item.id != todoList.id);
    },
    addItemHandle() {
      if (this.value == "") {
        return;
      }
      let itemId = Math.floor(Math.random() * 1000) + 10;
      this.todoLists.push({
        id: itemId,
        isActive: false,
        text: this.value,
      });
      this.value = "";
      console.log('fdfdf', this.todoLists);
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  color: white;
  font-family: Arial, Helvetica, sans-serif;
  font-weight: 100;
}
h1 {
  color: white;
  font-size: 50px;
}
#todoList {
  background-color: rgb(18, 189, 166);
  position: relative;
  width: 500px;
  margin: 0 auto;
  margin-top: 40px;
  padding-top: 40px;
  padding-bottom: 40px;
}
#todoList:before {
  content: "";
  position: absolute;
  width: 500px;
  height: 100%;

  background-color: rgb(209, 201, 201);
  top: -10px;
  left: -10px;
  z-index: -99;
}
.top {
  padding: 5px 50px;
}
.todo {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: rgba(255, 255, 255, 0.4);
  padding: 15px 50px;
  margin: 5px 0;
}
.todo-empty{
  padding: 15px 50px;
  font-size: 30px;
}
.change-bgr {
  background-color: white !important;
}
.fa-check {
  color: rgb(18, 189, 166);
}
.todo-line {
  position: relative;
}
.todo-line::before {
  content: "";
  position: absolute;
  top: 50%;
  left: -10px;
  border-bottom: 1px solid white;
  animation: moveLine 0.3s ease-in-out 0s forwards;
}
.action button {
  width: 30px;
  height: 30px;
  background-color: rgb(18, 189, 166);
  border: 2px white solid;
  margin-left: 6px;
  border-radius: 5px;
}
.sort {
  text-align: right;
  padding: 5px 50px;
  display: flex;
  justify-content: end;
  align-items: center;
}
.sort p {
  margin-right: 10px;
}
div .slider {
  position: relative;
  width: 60px;
  height: 30px;
  background-color: white;
  border-radius: 15px;
}
div .slider:hover {
  cursor: pointer;
}
div .slider-circle {
  position: absolute;
  padding: 0px;
  width: 26px;
  height: 26px;
  margin: 2px;
  border-radius: 100px;
  background-color: rgb(146, 247, 234);
  transition: all 0.3s ease-in-out;
}
div .slider-circle-on {
  transform: translateX(30px);
  background-color: rgb(18, 189, 166);
}

.bottom {
  padding: 0px 50px;
  margin-top: 50px;
}
.bottom input {
  width: 100%;
  color: black;
  height: 40px;
  font-weight: 40px;
  font-size: 20px;
  padding: 5px 40px;
}
.bottom > p {
  font-size: 20px;
  margin-bottom: 5px;
  padding: auto;
}
.add-todo {
  display: flex;
  justify-content: space-between;
}
.add-todo button {
  width: 150px;
  background-color: rgb(18, 189, 166);
  margin-left: 5px;
  border: 1px solid white;
  box-sizing: border-box;
}
button:hover {
  cursor: pointer;
}

@keyframes moveLine {
  0% {
    width: 0;
  }
  100% {
    width: calc(100% + 20px);
  }
}
.animation-move /* apply transition to moving elements */
 {
  transition: all 0.5s ease;
}

</style>

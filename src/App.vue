<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <template>
      <form @submit.prevent="handleSubmit">
        <label for="todo-input">Your new todo: </label>
        <input 
          id="todo-input"
          :value="inputText"
          @change="handleInputChange"
        />
        <button id="form__btn-submit" @click="handleSubmit">Add</button>
      </form>
    </template>

    <template>
      <section v-if="isListNotEmpty">
        <div id="tab">
          <div 
            class="tab__item"
            :class="{'tab__item--selected': filterItem.id === filterOptions.currentFilter}"
            v-for="filterItem in filterOptions.filterData"
            :key="filterItem.title"
          >
            <div @click="handleSelectFilterTab(filterItem.id)">{{ filterItem.title }}</div>
          </div>
        </div>
        <div id="todo">
          <div class="todo__title--wrapper">
            <div
              class="todo__title"
              v-for="titleItem in titleList"
              :key="titleItem"
            >
              {{ titleItem }}
            </div>
          </div>
          <div
            class="todo__item"
            v-for="(todoItem, index) in filteredTodoList"
            :key="index"
          >
            <div @click="handleIsStarted(todoItem.id)">
              <div v-if="todoItem.isStarted">
                &#9733;
              </div>
              <div v-else>
                &#9734;
              </div>
            </div>
            <div
              :class="{'todo__item--completed': todoItem.isCompleted === true }"
            >
              {{ todoItem.title }}
            </div>
            <!-- <div v-if="todoItem.isCompleted">
              Completed
            </div>
            <div v-else>
              Todo
            </div> -->
            <div>
              <input
                type="checkbox"
                :checked="todoItem.isCompleted"
                @change="handleIsCompletedChange(todoItem.id)"
              >
            </div>
          </div>
        </div>
      </section>
      <section v-else>
        There's no todo task
      </section>
    </template>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'

var randomDecision = function () {
  return Math.floor(Math.random() * 100) % 2  === 1
}

var todoItemTemplate = function (inputText) {
  return {
    id: (new Date()).getTime(),
    title: inputText,
    isCompleted: randomDecision(),
    isStarted: randomDecision(),
  }
}

export default {
  name: 'App',
  data: function() {
    return {
      inputText: "",
      filterOptions: {
        currentFilter: 1,
        filterData: [
          { id: 1, title: "All" },
          { id: 2, title: "Completed" },
          { id: 3, title: "Doing" },
        ],
      },
      titleList: ["Start", "Content", "Completed"],
      todoList: [],
    }
  },
  methods: {
    handleInputChange: function(e) {
      // console.log(e.target.value)
      this.inputText = e.target.value
    },
    handleSubmit: function () {
      // console.log(this.inputText)
      if (this.inputText) {
        this.todoList = this.todoList.concat(todoItemTemplate(this.inputText))
        console.log(this.todoList)
      }
      this.inputText = ""
    },
    handleIsStarted: function (itemId) {
      this.todoList = this.todoList.map(function(todoItem) {
        if (todoItem.id === itemId) {
          return {
            ...todoItem,
            isStarted: !todoItem.isStarted,
          }
        } else {
          return todoItem
        }
      })
    },
    handleIsCompletedChange: function (itemId) {
      this.todoList = this.todoList.map(function(todoItem) {
        if (todoItem.id === itemId) {
          return {
            ...todoItem,
            isCompleted: !todoItem.isCompleted,
          }
        } else {
          return todoItem
        }
      })
    },
    handleSelectFilterTab: function (tabId) {
      const { filterData } = this.filterOptions
      const filteredData = filterData.filter(function (filterItem) {
          return filterItem.id === tabId
      })
      this.filterOptions.currentFilter = filteredData[0].id
    },
  },
  computed: {
    isListNotEmpty: function () {
      return this.todoList.length >= 1
    },
    filteredTodoList: function () {
      var todoType = this.filterOptions.currentFilter
      if (todoType === 2) {
        return this.todoList.filter(function (todoItem) {
          return todoItem.isCompleted === true
        })
      } else if (todoType === 3) {
        return this.todoList.filter(function (todoItem) {
          return todoItem.isCompleted === false
        })
      } else {
        return this.todoList
      }
    },
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#tab {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  margin: 10px;
}

.tab__item {
  display: inline;
  width: 100px;
  height: 100%;
  margin: 5px;
}

.tab__item--selected {
  text-decoration: underline;
}

#form__btn-submit {
  height: 25px;
  widows: 150px;
  margin-left: 10px;
}

#todo {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  /* background-color: cyan; */

}

.todo__title--wrapper {
  width: 30%;
  display: flex;
  justify-content: row;
  justify-content: space-between;
  align-items: center;
}

.todo__title {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}

.todo__item {
  width: 30%;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}

.todo__item--completed {
  text-decoration: line-through;
}
</style>



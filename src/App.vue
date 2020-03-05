<template>
<div id="container">
    <div class="aa">
        <h3>Todo List | Vue.js</h3>
    </div>
    <hr>
    <div>
        <input class="aaa" type="text" v-model="todoText">
    </div>
    <div>
        <button @click="addTodo()" id="cc" class="btn btn-primary">Ekle</button>
    </div>
    <hr>
    <div class="todo-container">
        <Todo @deleteTodo="deleteTodo($event)" v-for="todo in todoList" :todo="todo" :key="todo.id" />
    </div>
</div>
</template>

<script>
import Todo from "@/components/Todo"
import axios from "axios"
export default {
    components: {
        Todo
    },
    data() {
        return {
            todoText: "",
            todoList: []
        }
    },
    methods: {
        addTodo() {
            axios.post("https://todovuejs-77718.firebaseio.com/todoList.json", {
                    text: this.todoText
                })
                .then(response => {
                    this.todoList.push({
                        id: response.data.name,
                        text: this.todoText
                    })
                })
                .catch(e => {
                    console.log(e)
                })
        },
        deleteTodo(todoId) {
            axios.delete("https://todovuejs-77718.firebaseio.com/todoList/" + todoId + ".json")
                .then(() => {
                    let index = this.todoList.findIndex(i => {
                        return i.id == todoId
                    })

                    this.todoList.splice(index, 1)
                })
                .catch(e => {
                    console.log(e)
                })
        }
    },
    created() {
        axios.get("https://todovuejs-77718.firebaseio.com/todoList.json")
            .then(response => {
                console.log(response.data)
                for (let key in response.data) {
                    console.log(response.data[key])
                    // this.todoList.push(response.data[key])
                    let todo = {
                        text: response.data[key].text,
                        id: key
                    }
                    this.todoList.push(todo)
                }

            })
            .catch()
    },

}
</script>

<script setup>
import { ref,watch,onMounted,computed } from 'vue';

const name = ref('');
const todos = ref([]);
const input_Category = ref(null);
const input_Content = ref('');

const todo_Asc = computed(()=>{
    todos.value.sort((a,b)=>{
       return b.createdAt - a.createdAt
    })
    }
)

const add_to_do = ()=>{
    if(input_Content.value.trim() === '' || input_Category.value === null ){
        return
    } todos.value.push({
        category : input_Category.value,
        content : input_Content.value,
        done : false,
        createdAt : new Date().getTime
    })
    }

    const remove = (todo)=>{
        todos.value = todos.value.filter(t=> t !== todo)
    }

watch(name,newValue=>{
    localStorage.setItem("name",newValue)
})

watch(todos,newValue=>{
    localStorage.setItem("todos",JSON.stringify(newValue))
},{deep:true})

onMounted(()=>{
    name.value = localStorage.getItem("name") || '';
    todos.value = JSON.parse(localStorage.getItem("todos")) || []
})

</script>

<template>
    <main @keyup.enter="add_to_do">
        <div class="input">
            <label class="name-input">
                What's up, <input type="text" placeholder="Your Name" v-model="name">
            </label>
            <h2>Write here</h2>
        </div>
        <div class="todo-input" >
                <input type="text" placeholder="Eg. Read books" v-model="input_Content">
        </div>
        <h3>Pick a Category</h3>
        <div class="input-category">
            <label class="category1">
                <input type="radio" name="category" value="business" v-model="input_Category" :class="input_Category">
                <span>Business</span>
            </label>
            <label class="category2">
                <input type="radio" name="category" value="personal" v-model="input_Category" :class="input_Category">
                <span>Personal</span>
            </label>
            <input type="submit" @click="add_to_do" value="Add to do" class="submit">
        </div>

        <section class="list-section">
            <div class="list" v-for="(todo, index) in todos" :key="index">
                <div class="todo">
                    <label :class="`${todo.category == 'business'? 'blue':'pink'}`">
                       <span><input type="checkbox" v-model="todo.done" :class="todo.category"></span> 
                        <input type="text" v-model="todo.content" class="todo-inp" :class="`${todo.done?'done':''}`">
                        <button @click="remove(todo)">Remove</button>
                    </label>
                </div>
            </div>
        </section>
    </main>
</template>

<style>
*{
    box-sizing: border-box;
    border: 0;
    font-family: Poppins,sans-serif;
}
body{
    background-color: rgb(232, 230, 230);
    display: flex;
    flex-direction: column;
    align-items: center
}

.input{
    margin-top: 1.5rem;
}
.input label{
    font-size: 1.7rem;
}
.input input{
    font-size: 1.5rem;
    background-color: rgb(232, 230, 230);
    width: fit-content;
}
input:focus{
    outline: none;
}
.todo-input{
    width: 100%;
    margin-top: 1.5rem;
}
.todo-input input{
    display: inline-block;
    height: 30px;
    width: 100%;
    background-color: rgb(250, 246, 246);
    box-shadow: 2px 2px 1px rgb(176, 173, 173);
    border-radius: 5px;
    padding: 20px;
}
.todo-input input:hover{
    transform: scale(1.05) translateY(-2px);
    transition: 300ms ease-in-out;
}
.input-category{
    display: grid;
    grid-template-columns: repeat(2,1fr);
    grid-template-rows: repeat(2,);
    grid-template-areas: 
    "business      personal"
    "add           add";
    margin-top: 10px;
    gap: 10px;
}
.category1, .category2{
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 100%;
    margin: auto;
    background-color:  rgb(250, 246, 246);
    border-radius: 5px;
    padding: 10px;
    gap: 8px;
    cursor: pointer;
    box-shadow: 2px 2px 1px rgb(176, 173, 173);
}
.category1{
    grid-area: business;
}
.category1:hover{
    background-color:  rgb(122, 122, 242);
    color: whitesmoke;
    transform: scale(1.05) translateY(-2px);
    transition: 300ms ease-in-out;
}
.category2{
    grid-area: personal;
}
.category2:hover{
    background-color:  rgb(242, 85, 197);
    color: whitesmoke;
    transform: scale(1.05) translateY(-2px);
    transition: 300ms ease-in-out;
}
.business{
    accent-color: blue;
}
.personal{
    accent-color: rgb(242, 85, 197);
}
.submit{
    grid-area: add;
    height: 2rem;
    border-radius: 5px;
    background-color:rgb(254, 56, 198) ;
    color: white;
    box-shadow: 2px 2px 1px rgb(204, 200, 200);
}
.submit:hover{
    transform: scale(1.05) translateY(-3px);
    transition: 300ms ease-in-out
}
.list-section{
    margin-top: 10px;
}
.todo label{
    display: grid;
    grid-template-columns: 10% 70% 20%;
    margin-bottom: 5px;
    align-items: center;
    background-color:rgb(250, 246, 246) ;
    padding-left: 3px;
    border-radius: 5px;
    box-shadow: 2px 2px 1px rgb(207, 204, 204);
}
.todo-inp{
    padding: 8px 4px;
}
.todo:hover{
    transform: scale(1.05) translateY(-2px);
    transition: 300ms ease-in-out;
}
button{
    padding: 4px 5px;
    background-color: tomato;
    color: white;
    translate: -5px;
    border-radius: 8px;
}
button:hover{
    transform: translatex(-8px);
    transition: 300ms ease-in-out
}
.blue{
    border-bottom: 1px solid blue;
}
.pink{
    border-bottom: 1px solid rgb(242, 85, 197);
}
.done{
    text-decoration: line-through;
}
</style>
<template>
    <div class="app">
        <form>
            <h4>Сщздание поста</h4>
            <input class="input" type="text" placeholder="Название">
            <input class="input" type="text" placeholder="Описание">
            <button class="btn" @click="createPost">Создать</button>
        </form>

        <div class="post" v-for="post in posts">
            <div><strong>Название:</strong> {{post.title}}</div>
            <div><strong>Описание:</strong> {{post.body}}</div>
        </div>
    </div>
</template>
<script>
export default {
    data(){
        return {
            posts:[
                {id: 1, title: 'Title 1', body:  'Описание 1'},
                {id: 2, title: 'Title 2', body:  'Описание 2'},
                {id: 3, title: 'Title 3', body:  'Описание 3'}
            ]
        }
    },
    methods:{
        createPost(){

        }
    }
}
</script>
<style>
*{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
.app{
    padding: 20px;
}
form{
    display: flex;
    flex-direction: column;
}
    .post{
        padding: 15px;
        border: 2px solid teal;
        margin-top: 15px;
    }
    .input{
        width: 100%;
        border: 1px solid teal;
        padding: 10px 15px;
        margin-top: 15px;
    }
    .btn{
        margin-top: 15px;
        align-self: flex-end;
        padding: 10px 15px;
        background: none;
        color: teal;
        border: 1px solid teal;
    }
</style>
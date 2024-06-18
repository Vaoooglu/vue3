<template>
    <div class="app">
        <h1>Страница с постами</h1>
        <my-button
        @click="showDialog"
        style="margin-top: 15px;margin-bottom: 15px;"
        >Создать пост</my-button>
        <my-dialog v-model:show="dialogVisible">
            <post-form
                    @create="createPost"
            />
        </my-dialog>

        <post-list
        :posts="posts"
        @remove = "removePost"
        />


    </div>
</template>
<script>
    import PostForm  from "./components/PostForm"
    import PostList from "./components/PostList";
    export default {
        components:{
            PostForm, PostList
        },
        data() {
            return {
                posts: [
                    {id: 1, title: 'Title 1', body: 'Описание 1'},
                    {id: 2, title: 'Title 2', body: 'Описание 2'},
                    {id: 3, title: 'Title 3', body: 'Описание 3'}
                ],
                dialogVisible:false,

        }
        },
        methods: {
            createPost(post) {
                this.posts.push(post);
                this.dialogVisible = false;
            },
            removePost(post) {
                this.posts = this.posts.filter(p => p.id != post.id);
            },
            showDialog(){
               this.dialogVisible = true;
            }
        }
    }
</script>
<style>
    * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
    }

    .app {
        padding: 20px;
    }
</style>
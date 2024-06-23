<template>
    <div class="app">
        <h1>Страница с постами</h1>
        <my-input
                v-model="searchQuery"
                placeholder="Поиск.."
        ></my-input>
        <div class="app_btns">
            <my-button
                    @click="showDialog"
            >Создать пост
            </my-button>
            <my-select
                    v-model="selectedSort"
                    :options="sortOptions"
            />
        </div>

        <my-dialog v-model:show="dialogVisible">
            <post-form
                    @create="createPost"
            />
        </my-dialog>

        <post-list
                :posts="sortedAndSearchPosts"
                @remove="removePost"
                v-if="!isPostLoading"
        />
        <div v-else>Loading...</div>
        <div class="page__wrapper">
            <div
                    v-for="pageNumber in totalPage"
                    :key="pageNumber"
                    class="page"
                    :class="{
    'current-page': page === pageNumber
    }"
                    @click="changePage(pageNumber)"
            >{{ pageNumber }}
            </div>
        </div>
    </div>
</template>
<script>
    import PostForm  from "./components/PostForm";
    import PostList from "./components/PostList";
    import axios from 'axios';


    export default {
        components:{
            PostForm, PostList
        },
        data() {
            return {
                posts: [],
                dialogVisible:false,
                isPostLoading: false,
                selectedSort: '',
                page: 1,
                totalPage: 0,
                limit: 10,
                searchQuery: '',
                sortOptions:[
                    {value:'title', name: 'По названию'},
                    {value:'body', name: 'По содержимому'},
                ]
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
            },
            changePage(pageNumber){
              this.page = pageNumber;
            },
            async fetchPosts(){
                try {
                    this.isPostLoading = true;
                    const response = await axios.get('https://jsonplaceholder.typicode.com/posts',{
                        params: {
                            _page: this.page,
                            _limit: this.limit
                        }
                    });
                    this.totalPage = Math.ceil(response.headers['x-total-count'] / this.limit);
                    this.posts = response.data;

                } catch (e) {
                    alert ('Error')
                } finally {
                    this.isPostLoading = false;
                }
            }
        },
        mounted(){
            this.fetchPosts();
        },
        computed:{
            sortedPosts(){
                return [ ...this.posts].sort((post1,post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]))
            },
            sortedAndSearchPosts(){
                return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
            },
        },
        watch: {
            page(){
                this.fetchPosts();
            }
//             selectedSort(newValue){
//                 this.posts.sort((post1,post2)=>{
// return post1[newValue]?.localeCompare(post2[newValue])
//                 })
//             },

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
    .app_btns{
        margin: 15px 0;
        display: flex;
        justify-content: space-between;
    }
    .page__wrapper{
        display: flex;
        margin-top: 15px;
    }
    .page{
        border: 1px solid black;
        padding: 5px 10px;
    }
    .current-page{
        background-color: #777777;
        color: #fff;
    }
</style>
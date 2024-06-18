<template>
<div v-if="posts.length > 0">
    <h3>Список записей</h3>
    <post-item
    v-for="post in posts"
    :post="post"
    :key="post.id"
    @remove="$emit('remove',post)"
    />
</div>
    <h2 v-else style="color:red">Список записей пуст</h2>
</template>

<script>
    import PostItem from "./PostItem";
    export default {
        components: {PostItem},
        props:{
          posts :{
              type: Array,
              required: true
          }
        },
        name: "PostList"
    }
</script>

<style scoped>

</style>
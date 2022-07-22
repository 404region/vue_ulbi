<template>
    <div class="app">
        <h1>Страница с постами</h1>
        <!-- <my-button @click="fetchPosts">Получить посты</my-button> -->
        <div class="app-btns">
            <my-button
                @click="showDialog"
            >
            Создать пост
            </my-button>
            <my-select
                v-model="selectedSort"
                :options="sortOptions"
            ></my-select>
        </div>

        <my-dialog v-model:show="dialogVisible">
            <post-form
                @create="createPost"
            />
        </my-dialog>
        <post-list 
            :posts="posts"
            @remove="removePost"
            v-if="!isPostsLoading"
        />
        <div v-if="isPostsLoading">Идет загрузка...</div>
    </div>
</template>

<script>
    import PostForm from "@/components/PostForm";
    import PostList from "@/components/PostList";
    
    import axios from 'axios';
    
    export default {
        components: {
            PostList, PostForm
        },
        data() {
            return {
                posts: [],
                dialogVisible: false,
                isPostsLoading: false,
                selectedSort: '',
                sortOptions: [
                    {value: 'title', name: 'По названию'},
                    {value: 'body', name: 'По содержимому'}
                ]
            }
        },
        methods: { 
            createPost(post) {
                this.posts.push(post);
                this.dialogVisible = false;
            },
            removePost(post) {
                this.posts = this.posts.filter(p => p.id !== post.id)
            },
            showDialog() {
                this.dialogVisible = true;
            },
            async fetchPosts(){
                try {
                    this.isPostsLoading = true;
                    setTimeout(async () => {
                        const response = await axios.get('https://jsonplaceholder.typicode.com/posts?_limit=10');
                        this.posts = response.data;

                        this.isPostsLoading = false;
                    }, 1000);

                } catch(e) {
                    alert('Ошибка');
                } finally {
                    //this.isPostsLoading = false;
                }
            }
        },
        mounted() {
            this.fetchPosts();
        },
        watch: {
            selectedSort(newValue) {
                console.log(newValue);
            },
            dialogVisible(newValue){

                console.log(newValue);
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
.app-btns {
    display: flex;
    justify-content: space-between;
}
</style>
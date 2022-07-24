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
        <div class="page__wrapper">
            <div 
                v-for="pageNumber in totalPages"
                 :key="pageNumber"
                 class="page"
                 :class="{
                    'current-page': page === pageNumber
                 }"
                 @click="changePage(pageNumber)"
            >
            {{ pageNumber }}
            </div>
        </div>
    </div>
</template>

<script>
    import PostForm from "@/components/PostForm";
    import PostList from "@/components/PostList";
    
    import axios from 'axios';
    
    export default {
        components: {
            PostList,
            PostForm,
        },
        data() {
            return {
                posts: [],
                dialogVisible: false,
                isPostsLoading: false,
                selectedSort: '',
                searchQuery: '',
                page: 1,
                limit: 10,
                totalPages: 0,
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
            changePage(pageNumber) {
                this.page = pageNumber;

            },
            async fetchPosts(){
                try {
                    this.isPostsLoading = true;
                    setTimeout(async () => {
                        const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
                            params: {
                                _page: this.page,
                                _limit: this.limit
                            }
                        });
                        // 101:
                        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit);
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
        copmputed: {
            /*sortedPosts(){
                return [...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]))
            },
            */
            /*
            sortedAndSearchedPosts() {
                return this.sortedPosts.filter(post => post.title.includes(this.searchQuery))
            }*/
        },

        watch: {
            selectedSort(newValue) {
                this.posts.sort((post1, post2) => {
                    return post1[newValue]?.localeCompare(post2[newValue])
                })
            },
            dialogVisible(newValue){

                console.log(newValue);
            },
            page() {
                this.fetchPosts();
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
.page__wrapper {
    display: flex;
    margin-top: 15px;
}
.page {
    border: 1px solid black;
    padding: 10px;
}
.current-page {
    border: 2px solid teal;
}
</style>
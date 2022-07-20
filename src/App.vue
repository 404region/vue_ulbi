<template>
    <div class="app">
        <post-form
        
            @create="createPost"
        />
        <post-list 
        :posts="posts"/>
        
    </div>
</template>

<script>
    import PostForm from "@/components/PostForm";
    import PostList from "@/components/PostList";
    
    export default {
        components: {
            PostList, PostForm
        },
        data() {
            return {
                posts: [ 
                    {id: 1, title: 'fdjhfdhj 1', body: 'hgjhjghh'},
                    {id: 2, title: 'fdjhfdhj 2', body: 'hgjhjghh'},
                    {id: 3, title: 'fdjhfdhj 3', body: 'hgjhjghh'},
                ]
            }
        },
        methods: { 
            createPost(post) {
                this.posts.push(post);
            },
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
.input {
  width: 100%;
  border: 1px solid teal;
  padding: 10px 15px;  
  margin-top: 15px;
}
form {
    display: flex;
    flex-direction: column;
}
.btn {
    margin-top: 15px;
    align-self: flex-end;
    padding: 10px 15px;
    background: none;
    color: teal;
    border: 1px solid teal;
}
</style>
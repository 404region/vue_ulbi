<template>
    <form @submit.prevent>
        <h4>Создание поста</h4>
        <my-input
            v-model="post.title"
            class="input"
            type="text"
            placeholder="Название"
        ></my-input>
        <my-input 
            v-model="post.body"
            class="input"
            type="text"
            placeholder="Описание"
        ></my-input>

        <my-button 
            @click="createPost" 
            class="btn"
            style="align-self: flex-end; margin-top: 15px;"
        >
            Создать
        </my-button>
    </form>
</template>

<script>
//import MyButton from "@/components/UI/MyButton.vue";



export default {
   /*components: {
        MyButton
    },*/
    data() {
        return {
            post: {
                title: '',
                body: ''
            }
        }
    },
    methods: {
        createPost() {
            this.post.id = Date.now();
            this.$emit('create', this.post);
            this.post = {
                title: '',
                body: ''
            }
        }
    },
    watch: {
        //Отслеживание когда пост создается
        post(newVal) {
            console.log(newVal);
        }
        /*
        //глубокое отслеживание - отслеживает не только когда пост создается,
        // но и при изменении полей всплывающего окна
        post: {
            handler(newVal) {
                console.log(newVal);
            },
            deep: true
        }
        */
    }
}
</script>

<style scoped>
    form {
        display: flex;
        flex-direction: column;
    }
</style>
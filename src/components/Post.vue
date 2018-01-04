<template>
    <div>
        <q-search v-model="q" @keyup.enter="search" debounce="1000" />
        <q-card v-for="(post,index) in posts" :key="post.id">
            <q-card-title>
                {{post.title}}
            </q-card-title>
            <q-card-media>
                <img :src="post.image">
            </q-card-media>

            <q-card-separator />
            <q-card-main>
                <p class="thin-paragraph" v-html="nl2br(post.excerpt)"></p>
            </q-card-main>

            <q-card-separator />
            <q-card-actions>
                <q-btn color="primary" @click="editPost(post)">Edit Post</q-btn>
            </q-card-actions>

        </q-card>

        <q-pagination v-model="page" :max="maxPages" />

    </div>
</template>

<script>
const APIURL = process.env.APIURL
import Vue from 'vue'
import {
    QSearch,
    QCard,
    QCardTitle,
    QCardSeparator,
    QCardMain,
    QCardMedia,
    QCardActions,
    QIcon,
    QPopover,
    QList,
    QItem,
    QItemMain,
    QBtn,
    QPagination,
} from 'quasar'

export default {
    components: {
        QSearch,
        QCard,
        QCardTitle,
        QCardSeparator,
        QCardMain,
        QCardMedia,
        QCardActions,
        QIcon,
        QPopover,
        QList,
        QItem,
        QItemMain,
        QBtn,
        QPagination,
    },
    data() {
        return {
            q: '',
            posts: [],
            page: 1,
            maxPages: 0,
            params:{}
        }
    },

    watch: {
        page() {
        	this.loadData()
        },
        q(){
        	this.loadData()
        }
    },

    methods: {
        loadData() {
            let vm = this
            
            this.params.page = this.page
            this.params.q = this.q

            Vue.axios.get(APIURL + '/post', {
                    params: this.params
                })
                .then(response => {
                    console.log(response.data)
                    vm.posts = response.data.data
                    vm.page = response.data.current_page
                    vm.maxPages = response.data.last_page

                })
        },
        
        search(){
        	console.log()
        },

        nl2br(str) {
            var breakTag = '<br>';
            return (str + '').replace(/([^>\r\n]?)(\r\n|\n\r|\r|\n)/g, '$1' + breakTag + '$2');
        },

        editPost(row) {
            this.$router.push({
                path: '/post/' + row.id + '/edit'
            })
        }
    },

    mounted() {
        this.loadData();
    },

    beforeDestroy() {

    }
}
</script>

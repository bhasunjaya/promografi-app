<template>
    <!-- if you want automatic padding use "layout-padding" class -->
    <div class="layout-padding">
        <q-card v-for="(raw,index) in raws" :key="raw.id">
            <q-card-title>
                {{raw.author}}
                <q-icon slot="right" name="more_vert">
                    <q-popover ref="popover">
                        <q-list link class="no-border">
                            <q-item to="/post/create">
                                <q-item-main label="Create Post" />
                            </q-item>
                            <q-item @click="deletePost">
                                <q-item-main label="Delete" />
                            </q-item>
                        </q-list>
                    </q-popover>
                </q-icon>
            </q-card-title>
            <q-card-media>
                <img :src="raw.image">
            </q-card-media>

            <q-card-separator />
            <q-card-main>
                <p class="thin-paragraph" v-html="nl2br(raw.content)"></p>
            </q-card-main>

            <q-card-separator />
            <q-card-actions>
                <q-btn color="primary" @click="goto(raw.id)">Create Post</q-btn>
                <q-btn color="negative" @click="deletePost(raw.id,index)">Delete</q-btn>
            </q-card-actions>

        </q-card>
    </div>
</template>

<script>
const APIURL = process.env.APIURL
import {
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
} from 'quasar'

export default {
    components: {
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
    },
    data() {
        return {
            raws: []
        }
    },

    methods: {
        deletePost(id,index) {
        	let vm = this
        	this.raws.splice(index,1)
            this.$http.put(APIURL + '/raw/' + id,{is_read:true})
                .then(response => {
                })
        },
        goto(id) {
            this.$router.push({
                path: 'post/create/' + id
            })
        },

        loadData() {
            this.$http.get(APIURL + '/raw')
                .then(response => {
                    this.raws = response.data
                })
                // this.$http.api
                //     .get('/raw')
                //    
        },
        nl2br(str) {
            var breakTag = '<br>';
            return (str + '').replace(/([^>\r\n]?)(\r\n|\n\r|\r|\n)/g, '$1' + breakTag + '$2');
        }
    },

    mounted() {
        this.loadData()
    },
    beforeDestroy() {

    }
}
</script>

<style>
</style>

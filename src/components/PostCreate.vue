<template>
    <!-- if you want automatic padding use "layout-padding" class -->
    <div class="layout-padding">
        <q-card inline>
            <q-card-media>
                <img src="https://scontent.cdninstagram.com/t51.2885-15/s640x640/sh0.08/e35/25007049_842655262573692_2284714244018536448_n.jpg">
            </q-card-media>
            <q-card-main>
                <q-select v-model="category_id" float-label="Category" :options="categories" />
                <q-field label="Post Title">
                    <q-input v-model="title" />
                </q-field>
                <q-input v-model="excerpt" type="textarea" float-label="Excerpt" :min-rows="2" />
                <q-input v-model="content" type="textarea" float-label="Content" :min-rows="4" />
                <q-select multiple float-label="Mall" v-model="malls" :options="listMalls" />

                <q-datetime-range float-label="Periode" type="date" v-model="range" />
                <q-checkbox v-model="is_featured" label="Featured?" />
                <br>
                <br>
                <q-checkbox v-model="is_publish" label="Publish?" />

            </q-card-main>
            <q-card-separator />
            <q-card-actions>
                <q-btn color="primary" @click="createPost">Create Post</q-btn>
            </q-card-actions>
        </q-card>
    </div>
</template>


<script>
import Vue from 'vue'
import {
    Toast
} from 'quasar'
const APIURL = process.env.APIURL
import {
    QCard,
    QCardMedia,
    QCardMain,
    QCardActions,
    QCardSeparator,
    QIcon,
    QField,
    QInput,
    QDatetimeRange,
    QCheckbox,
    QSelect,
    QBtn,
} from 'quasar'

export default {
    components: {
        QCard,
        QCardMedia,
        QCardMain,
        QCardActions,
        QCardSeparator,
        QIcon,
        QField,
        QInput,
        QDatetimeRange,
        QCheckbox,
        QSelect,
        QBtn,
    },
    data() {
        return {
            errors: null,
            category_id: null,
            title: '',
            excerpt: '',
            content: '',
            image: '',
            start_at: '',
            end_at: '',
            raw_id: '',
            source: '',
            is_featured: false,
            is_publish: true,
            range: {
                from: null,
                to: null,
            },
            categories: [],
            malls: [],
            listMalls: [{
                label: 'Google',
                value: 'goog'
            }, {
                label: 'Facebook',
                value: 'fb'
            }]
        }
    },

    methods: {
        deletePost() {
            console.log('delete post')
        },

        createPost() {
            // console.log(this.data);

            let vm = this
            Vue.axios.post(APIURL + '/post', {
                    category_id: this.category_id,
                    title: this.title,
                    excerpt: this.excerpt,
                    content: this.content,
                    image: this.image,
                    start_at: this.range.from,
                    end_at: this.range.to,
                    raw_id: this.$route.params.id,
                    is_featured: this.is_featured,
                    is_publish: this.is_publish,
                    malls: this.malls

                })
                .then(response => {
                    this.$route.push({path:'/draft'})
                })
                .catch(function(error) {
                    // console.log(error.response.data.errors.length)
                    let msg = '';
                    let oErrors = Object.values(error.response.data.errors)
                    // Vue.set(vm,'errors',error.response.data.errors);
                    for (var i = 0; i < oErrors.length; i++) {
                        msg = msg+oErrors[i][0]+'<br>'
                        // console.log(oErrors[i][0])
                    }
                    Toast.create.negative(msg)
                });
        },
        getMall() {
            return Vue.axios.get(APIURL + '/mall')
        },
        getRaw() {
            return Vue.axios.get(APIURL + '/raw/' + this.$route.params.id)
        },
        getCategories() {
            return Vue.axios.get(APIURL + '/category')
        }
    },

    mounted() {
        let vm = this
        Vue.axios.all([this.getMall(), this.getCategories(), this.getRaw()])
            .then(Vue.axios.spread(function(mall, cat, raw) {

                for (var i = 0; i < cat.data.length; i++) {
                    vm.categories.push({
                        label: cat.data[i].title,
                        value: cat.data[i].id,
                    })
                }
                for (var i = 0; i < mall.data.length; i++) {
                    vm.listMalls.push({
                        label: mall.data[i].title,
                        value: mall.data[i].id,
                    })
                }

                vm.title = raw.data.title
                vm.excerpt = raw.data.content
                vm.content = raw.data.content
                vm.image = raw.data.image
                vm.raw_id = raw.data.id
                vm.source = raw.data.source
            }));

    },
    beforeDestroy() {

    }
}
</script>
<style>
</style>

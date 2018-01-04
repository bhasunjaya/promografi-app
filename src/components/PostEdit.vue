<template>
    <!-- if you want automatic padding use "layout-padding" class -->
    <div class="layout-padding">
        <q-card inline>
            <q-card-media>
                <img src="">
            </q-card-media>
            <q-card-main>
                <q-select v-model="form.category_id" float-label="Category" :options="categories" />
                <q-field label="Post Title">
                    <q-input v-model="form.title" />
                </q-field>
                <q-input v-model="form.excerpt" type="textarea" float-label="Excerpt" :min-rows="2" />
                <q-input v-model="form.content" type="textarea" float-label="Content" :min-rows="4" />
                <q-select multiple float-label="Mall" v-model="form.selectedmalls" :options="listMalls" />

                <q-datetime-range float-label="Periode" type="date" v-model="form.range" />

                <q-checkbox v-model="form.is_featured" label="Featured?" />
                <br>
                <q-checkbox v-model="form.is_publish" label="Publish?" />

            </q-card-main>
            <q-card-separator />
            <q-card-actions>
                <q-btn color="primary" @click="editPost">Edit Post</q-btn>
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
            form: {
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
                malls: [],
                selectedmalls: [],
                range: {
                    from: null,
                    to: null,
                },
            },

            categories: [],
            listMalls: []
        }
    },
    methods: {
        deletePost() {
            console.log('delete post')
        },

        loadData() {
            let vm = this
            Vue.axios.all([this.getMall(), this.getCategories(), this.getPost()])
                .then(Vue.axios.spread(function(mall, cat, post) {

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

                    vm.form = post.data
                    vm.form.is_publish = Boolean(post.data.is_publish)
                    vm.form.is_featured = Boolean(post.data.is_featured)

                }));
        },

        editPost() {
            // console.log(this.data);
            console.log(this.form)

            let vm = this
            Vue.axios.put(APIURL + '/post/'+this.$route.params.id, this.form)
                .then(response => {
                    Toast.create.positive('data terupdate')
                })
                .catch(function(error) {
                    let msg = '';
                    let oErrors = Object.values(error.response.data.errors)
                        // Vue.set(vm,'errors',error.response.data.errors);
                    for (var i = 0; i < oErrors.length; i++) {
                        msg = msg + oErrors[i][0] + '<br>'
                            // console.log(oErrors[i][0])
                    }
                    Toast.create.negative(msg)
                });
        },
        getMall() {
            return Vue.axios.get(APIURL + '/mall')
        },
        getPost() {
            return Vue.axios.get(APIURL + '/post/' + this.$route.params.id)
        },
        getCategories() {
            return Vue.axios.get(APIURL + '/category')
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

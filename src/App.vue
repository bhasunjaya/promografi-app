<template>
    <!-- Don't drop "q-app" class -->
    <div id="q-app">
        <q-layout ref="layout" view="lHh Lpr fff" :left-class="{'bg-grey-2': true}">
            <q-toolbar slot="header" class="glossy">
                <q-btn flat @click="$refs.layout.toggleLeft()">
                    <q-icon name="menu" />
                </q-btn>

                <q-toolbar-title>Promografi</q-toolbar-title>
            </q-toolbar>
            <div slot="left">
                <q-list no-border link inset-separator>
                    <q-list-header>Essential Links</q-list-header>
                    <q-side-link item to="/draft">
                        <q-item-side icon="school" />
                        <q-item-main label="Draft" sublabel="draft post yang harus dimanage" />
                    </q-side-link>
                    <q-side-link item to="/post">
                        <q-item-side icon="rss feed" />
                        <q-item-main label="Posting" sublabel="postingan yang sudah dibuat" />
                    </q-side-link>
                </q-list>
            </div>

            <router-view/>
        </q-layout>
    </div>
</template>

<script>
import {
    QLayout,
    QToolbar,
    QToolbarTitle,
    QBtn,
    QIcon,
    QList,
    QListHeader,
    QItem,
    QItemSide,
    QItemMain,
    QSideLink

} from 'quasar'
export default {
    components: {
        QLayout,
        QToolbar,
        QToolbarTitle,
        QBtn,
        QIcon,
        QList,
        QListHeader,
        QItem,
        QItemSide,
        QItemMain,
        QSideLink

    },

    methods: {}
}
</script>

<style></style>

<template>
    <el-container class="m-layout-container">
        <el-aside class="m-layout-aside" :width="asideWidth+'px'">
            <slot name="aside"></slot>
        </el-aside>
        <el-container>
            <el-header class="m-layout-header" :style="{height:headerHeight+'px'}">
                <slot name="header"></slot>
            </el-header>
            <el-main class="m-layout-main">
                <tabs v-if="needNavTab"></tabs>
                <keep-alive :include="tagsList" v-if="needNavTab">
                    <router-view></router-view>
                </keep-alive>
                <router-view v-else></router-view>
            </el-main>
        </el-container>
    </el-container>
</template>

<script>
    import config from '../../config'
    import Tabs from '../navTabs/Tags'
    import bus from '../navTabs/bus'

    export default {
        name: "LeftUpDownLayout",
        props: {
            asideWidth: {
                type: Number,
                default: 240
            },
            headerHeight: {
                type: Number,
                default: 60
            },
        },
        components: {
            tabs: Tabs
        },
        data() {
            return {
                tagsList: [],
                needNavTab: false,
            }
        },
        created() {
            this.needNavTab = !!config.needNavTab;
            if (this.needNavTab) {
                bus.$on('tags', msg => {
                    let arr = []
                    for (let i = 0, len = msg.length; i < len; i++) {
                        msg[i].name && arr.push(msg[i].name)
                    }
                    this.tagsList = arr
                })
            }
        },
    }
</script>

<style scoped>

</style>
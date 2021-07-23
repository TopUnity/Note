<template>
    <div id="app">
        <n-layout>
            <n-layout-header>
                <h1>极简笔记</h1>
                <div class="actions">
                    <n-button @click="switchTitle">
                        <span>{{ routeName }} </span>
                    </n-button>
                </div>
            </n-layout-header>
            <n-layout-content>
                <n-message-provider>
                    <router-view />
                </n-message-provider>
            </n-layout-content>
        </n-layout>
    </div>
</template>

<script setup>
import { computed, onMounted, ref } from "vue";
import { useStore } from "vuex";
import {
    NLayout,
    NLayoutHeader,
    NLayoutContent,
    NGridItem,
    NButton,
    NGrid,
    NH1,
    NGradientText
} from "naive-ui";
import router from "./router/index.js"
import { useRoute } from "vue-router";
const store = useStore();
const route = useRoute()
const routeName = computed(() => {
    if(route.name === "main") return "预览"
    if(route.name === "preview") return "编辑"
})
const switchTitle = () => {
    if(route.name === "main"){
        router.push("preview")
    };
    if(route.name === "preview"){
        router.push("main");
    } 
}
onMounted(()=>{
    let myData= localStorage.getItem("myData");
    if(myData){
        myData = JSON.parse(myData)
        store.commit("updateContents",myData[0])
    }
})

window.onbeforeunload = (event) =>{
    let contentList = store.state.contentList;
    let MdeList = store.state.MdeList;
    let editListNumber = store.state.editListNumber;
    let myData = [contentList,MdeList,editListNumber];
    localStorage.setItem("myData",JSON.stringify(myData));
}
</script>

<style lang="postcss" scoped>
#app {
    background: #f5f5f5;
    & .n-layout-header {
        /* text-align: center; */
        border-bottom: 1px solid #e6e6e6;
        display: flex;
        align-items: center;
        justify-content: center;
        & h1 {
            flex: 1;
            text-align: center;
        }
        & .actions {
            position: absolute;
            right: 0;
            padding-right: 16px;
        }
    }
    & .n-layout-content {
        margin: auto;
        padding: 16px;
        max-width: 720px;
    }
}
</style>
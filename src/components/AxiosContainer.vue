<template>
    <div>
        <h1>API container</h1>
        <h2>Hello, {{username}}</h2>
        <button @click="getQuote">Get quote</button>
        <h2>QOTD: {{quoteText}}</h2>
        <h3 v-if="showLoading">Loading</h3>
        <button @click="setCookie">Set a cookie</button>
        <button @click="printCookie">Print the cookie value</button>
        <button @click="deleteCookie">Delete the cookie</button>
    </div>
</template>

<script>
import {useMainStore} from '@/stores/main'
import {mapState} from 'pinia';
import cookies from 'vue-cookies';

    export default {
        name : "AxiosContainer",
        computed: {
            // State
            ...mapState(useMainStore,['username']),

            // Getters
            ...mapState(useMainStore,['firstInital','quoteText','quoteHttpCode']),

            // Actions
            ...mapState(useMainStore,['getQuote'])

        },
        data() {
            return {
                store: undefined,
                showLoading : false
            }
        },
        methods: {
            setCookie() {
                cookies.set('testCookie', 'my cookie');
            },
            deleteCookie(){
                cookies.remove('testCookie');
            },
            printCookie(){
                console.log(cookies.get('testCookie'));
            }
        },
        mounted () {
            this.store = useMainStore();
            useMainStore().$onAction(({name, after})=>{
                if (name == "getQuote"){
                    after(()=>{
                        this.showLoading = true;
                    })
                } else if (name == "newQuoteNotification"){
                    this.showLoading = false;
                }
            })
        },
    }
</script>

<style scoped>

</style>
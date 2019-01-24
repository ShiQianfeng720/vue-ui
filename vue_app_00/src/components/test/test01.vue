<template>
    <div>
    <h3>这是01页面.vue</h3>
    <h3>方式一</h3>
    <router-link  :to=" '/test02?id='+id">跳到test02</router-link>
    <h3>方式二</h3>
    <h1 @click="jump">跳到test02</h1>
    </div>
</template>
<script>
export default {
    methods: {
        jump(){
            // this.$router.push('/test02?id='+this.id)
            this.$router.push("/test02/19")
        }
    },
    data(){
        return{
            id:9
        }
    }
}
</script>
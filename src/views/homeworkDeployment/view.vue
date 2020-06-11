<template>
    <div class="deploymentManagement">
        <One v-if="tabShow"
             @checkFun="checkFun"></One>
        <Two v-else
             @twoCheckFun="twoCheckFun"></Two>
    </div>
</template>
<script>
    import One from './one'
    import Two from './two'
    export default {
        name: "homeworkDeployment",
        data() {
            return {
                tabShow: true
            }
        },
        components: {
            One,
            Two
        },
        methods: {
            checkFun(data) {
                console.log(data)
                this.tabShow = false
            },
            twoCheckFun(data) {
                console.log(data)
                this.tabShow = true
            }
        }
    }
</script>
<style lang="scss" scoped>
    .deploymentManagement {
        height: 100%;
    }
</style>
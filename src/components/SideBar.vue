<template>
    <div class="sidebar">
        <el-menu unique-opened :default-active="$route.path" class="my-menu" router background-color="#324157" text-color="#fff" >
            <template  v-for="(item , index) in $router.options.routes" v-if="item.meta.menuShow">
                <el-submenu :index="item.path" v-if="item.children && item.children.length > 1">
                    <template slot="title">
                        {{item.meta.menuName}}
                    </template>
                    <el-menu-item v-for="(itemChild , index) in item.children" :index="itemChild.path" :key="index" v-if="itemChild.meta.menuShow">
                        <span>{{itemChild.meta.menuName}}</span>
                    </el-menu-item>
                </el-submenu>
                <el-menu-item :index="item.children[0].path" v-else>
                    {{item.children[0].meta.menuName}}
                </el-menu-item>
            </template>
        </el-menu>
    </div>
</template>
<script>
    export default{
        data(){
            return {
                account: {

                }
            }
        },
        methods: {

        },

        methods:{
            
        },
        mounted(){
            // this.account = this.$store.state.account;
        },
    }
</script>
<style scoped>
    .sidebar{
        position:absolute;
        width:160px;
        height:calc(100% - 60px);
        top:60;
        left:0;
        box-sizing: border-box;
        overflow-y: auto;
        overflow-x: hidden;
    }
    .my-menu{
        height:100%;
    }
    .iconfont{
        color:white;
        margin-right:10px;
    }
</style>
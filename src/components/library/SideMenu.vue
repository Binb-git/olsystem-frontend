<template>
    <el-menu
        class="categories"
        default-active="0"
        @select="handleSelect"
        active-text-color="red">
        <el-menu-item index="0">
            <i class="el-icon-menu"></i>
            <span slot="title">全部</span>
        </el-menu-item>
        <el-menu-item index="1">
            <i class="el-icon-menu"></i>
            <span slot="title">主食</span>
        </el-menu-item>
        <el-menu-item index="2">
            <i class="el-icon-menu"></i>
            <span slot="title">快餐</span>
        </el-menu-item>
        <el-menu-item index="3">
            <i class="el-icon-menu"></i>
            <span slot="title">饮品</span>
        </el-menu-item>
        <el-menu-item index="4">
            <i class="el-icon-menu"></i>
            <span slot="title">甜点</span>
        </el-menu-item>
        <el-menu-item index="5">
            <i class="el-icon-menu"></i>
            <span slot="title">套餐</span>
        </el-menu-item>
        <el-menu-item index="6">
            <i class="el-icon-menu"></i>
            <span slot="title">小吃</span>
        </el-menu-item>
    </el-menu>
</template>

<script>
export default {
    name: 'SideMenu',
    data() {
        return {
            cid: ''
        }
    },
    methods: {
        handleSelect(key, keyPath) {
            this.cid = key
            this.$emit('indexSelect')
        }
    }
}
</script>

<style scoped>
.categories {
    position: fixed;
    margin-left: 0;
    /*left: -700px;*/
    top: 100px;
    width: 150px;
}
</style>


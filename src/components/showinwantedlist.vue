<template>
    <el-container>
        <el-main>
            <el-row>
                <el-col :span="8" style="margin-left: 25px">
                    <el-card :body-style="{ padding: '0px' }" style="width: 400px;height: 460px;">
                        <img :src="book.cover" class="image">
                    </el-card>
                </el-col>
                <el-col :span="12" style="margin-left: 75px">
                    <el-card :body-style="{ padding: '0px' }" style="height: 460px;">
                        <p class="ph1">
                            {{ book.bookname }}
                        </p>

                        <p class="ph2">
                            简介 ： {{ book.abs }}
                        </p>
                        <p class="ph3">
                            价格 ： {{ book.price }}
                        </p>
                        <p class="ph4">
                            类别 ： {{ book.category.name }}
                        </p>
                        <p class="ph5">
                            用户名 ： {{un}}
                        </p>
                        <p class="ph7">
                            电话号码: {{form.phone}}
                        </p>
                        <p class="ph6">
                            配送地址 ： {{ user_book.adress }}
                        </p>
<!--                        <el-form ref="form" :model="form" label-width="80px">-->
<!--                            <el-form-item label="配送地址">-->
<!--                                <el-input v-model="form.adress"></el-input>-->
                        <div style="padding-top: 15px; ">
                            <span style="width:500px; font-size: 28px">我的订单</span>
                            <div class="bottom clearfix">
                                <el-button type="info" disabled class="button"
                                           style="height: 35px;width: 180px" @click="wantedlist(book.id)">已经在收藏夹中了
                                </el-button>
                                <el-button type="primary" class="button1" icon="el-icon-back"
                                           style="height: 35px;width: 40px" @click="back()">
                                </el-button>
                            </div>
                        </div>
                    </el-card>
                </el-col>
            </el-row>
        </el-main>
    </el-container>
</template>

<script>

import {MessageBox} from 'element-ui'
import {Message} from 'element-ui'

export default {
    name: "showinwantedlist",
    //components: {NavMenu},
    data() {
        return {
            book: {},
            user_book:{},
            currentDate: new Date(),
            un: JSON.parse(window.localStorage.getItem('username' || '[]')),
            form: {

            },
        }
    },
    mounted() {
        this.getCover()
        this.getAdress()
        this.query()
    },
    methods: {
        query() {
            let _this = this
            this.$axios.post('userinfo', {username:_this.$store.state.username}).then(
                resp => {
                    if (resp.data.code === 200) {
                        this.form = resp.data.data
                    }
                }
            )
        },
        getAdress() {
            this.bid = this.$route.query.id
            let _this = this;
            this.$axios.post('/getadressbybid', {
                bid: this.bid,
                username: JSON.parse(window.localStorage.getItem('username' || '[]'))
            }).then(resp => {
                if (resp && resp.status === 200) {
                    console.log("ad" + resp.data.data)
                    _this.user_book = resp.data.data
                }
            })
        },

        getCover() {
            this.id = this.$route.query.id
            let _this = this;
            this.$axios.post('/getbookbybid', {
                id: this.id
            }).then(resp => {
                if (resp && resp.status === 200) {
                    _this.book = resp.data.data
                }
            })
        },
        wantedlist(id) {
            MessageBox.confirm('确定要添加到订单中吗?', '提示', {
                confirmButtonText: '确定',
                cancelButtonText: '取消',
                type: 'warning'
            }).then(() => {
                this.$axios.post('/addwantedlist', {
                    bid: id,
                    username: JSON.parse(window.localStorage.getItem('username' || '[]'))
                }).then(resp => {
                    if (resp && resp.status === 200) {
                        Message.success("添加成功")
                    }
                })
                this.isLiked = 1
            }).catch(() => {
                Message.info("已取消操作")
            })
        },
        back() {
            //返回上一级
            this.$router.go(-1)
        }
    }
}
</script>

<style scoped>
.el-row {
    margin-left: 180px;
    margin-top: 55px;
}

.ph1 {
    height: 20px;
    font-size: 24px;

}

.ph2 {
    height: 40px;
    text-indent: 2em;
    text-align: justify;
    margin-left: 15px;
    margin-right: 15px;
    font-size: 16px;
}

.ph3 {
    height: 35px;
    margin-left: 15px;
    text-align: left;
}

.ph4 {
    height: 35px;
    margin-left: 15px;
    text-align: left;
}
.ph5 {
    height: 35px;
    margin-left: 15px;
    text-align: left;
}
.ph6 {
    height: 35px;
    margin-left: 15px;
    text-align: left;
}
.ph7 {
    height: 35px;
    margin-left: 15px;
    text-align: left;
}

time {
    font-size: 13px;
    color: #999;
}

.bottom {
    margin-top: 13px;
    line-height: 12px;
}

.button {
    padding: 0;
    float: right;
    font-size: 20px;
    margin-right: 25px;
}

.button1 {
    padding: 0;
    float: left;
    font-size: 24px;
    margin-right: 15px;
}

.image {
    width: 100%;
    height: 460px;
    display: block;
}

.clearfix:before,
.clearfix:after {
    display: table;
    content: "";
}

.clearfix:after {
    clear: both
}

</style>


<!--
 * @Author: your name
 * @Date: 2021-04-29 21:29:19
 * @LastEditTime: 2021-05-25 21:05:32
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \Vue3project\ewshop\src\views\profile\Profile.vue
-->
<template>
    <div id="box">
        <nav-bar class="nav-bar">
            <template v-slot:default>个人中心</template>
        </nav-bar>
        <div class="user-box">
            <div class="user-info">
                <div class="info">
                    <img src="@/assets/images/user.png" alt="">
                    <div class="user-desc">
                        <span>昵称：{{user.name}}</span>
                        <span>登录邮箱：{{user.email}}</span>
                        <span class="Signature">个性签名：距离成功只差一步之遥</span>
                    </div>
                </div>
            </div>
            <ul class="user-list">
                <li class="van-hairline--bottom" @click="GoTo('/collect')">
                    <span>我的收藏</span>
                    <van-icon name="arrow" />
                </li>
                <li class="van-hairline--bottom" @click="GoTo('/order')">
                    <span>我的订单</span>
                    <van-icon name="arrow" />
                </li>
                <li class="van-hairline--bottom" @click="GoTo('/account')">
                    <span>账号管理</span>
                    <van-icon name="arrow" />
                </li>
                <li class="van-hairline--bottom" @click="GoTo('/address')">
                    <span>地址管理</span>
                    <van-icon name="arrow" />
                </li>
                <li class="van-hairline--bottom" @click="GoTo('/about')">
                    <span>关于我们</span>
                    <van-icon name="arrow" />
                </li>
            </ul>
        </div>
        <div class="logout">
            <van-button @click="tologout" round block color="#44BA80">退出登录</van-button>
        </div>
    </div>
</template>

<script>
import NavBar from "../../components/common/navbar/NavBar";
import { logout, getuser } from '../../network/user';
import { Toast } from 'vant';
import { useRouter } from 'vue-router';
import { useStore } from 'vuex';
import { onMounted, reactive, toRefs } from 'vue';
export default {
    name: "Profile",
    components: {
        NavBar,
    },
    setup() {
        const router = useRouter();
        const store = useStore();

        const state = reactive({
            user: {},
        })

        onMounted(() => {
            getuser().then(res => {
                console.log(res);
                state.user = res;
            })
        })

        const tologout = () => {
            logout().then(res => {
                if(res.status == '204') {
                    Toast.success('退出成功');
                    //清除(window.localStorage里的)token
                    window.localStorage.setItem('token', '');
                    store.commit('setIsLogin', false);
                    setTimeout(() => {
                        router.push({path: '/login'});
                    }, 500);
                }
            })
        }

        //跳转方法 路径    参数
        const GoTo = (path, query) => {
            router.push({path, query: query || {}});
        }

        return {
            tologout,
            ...toRefs(state),
            GoTo,
        }
    }
}
</script>

<style lang="scss" scoped>
#box {
    background: #fcfcfc;
    height: 90vh;
    .user-box {
        margin-top: 65px;
        .user-header {
            position: fixed;
            top: 0;
            left: 0;
            z-index: 10000;
            width: 100%;
            height: 44px;
            line-height: 44px;
            padding: 0 10px;
            color: #252525;
            background: #fff;
            border-bottom: 1px solid #dcdcdc;
            .user-Signature {
                font-size: 14px;
            }
        }
        .user-info {
            width: 94%;
            height: 115px;
            margin: 10px;
            background: linear-gradient(90deg, #31c7a7, #a1c7c7);
            box-shadow: 0 2px 5px #269090;
            border-radius: 6px;
            margin-top: 50px;
            text-align: left;
            .info {
                position: relative;
                display: flex;
                width: 100%;
                height: 100%;
                padding: 25px 20px;
                img {
                    width: 60px;
                    height: 60px;
                    border-radius: 50%;
                    margin-top: 4px;
                }
                .user-desc {
                    display: flex;
                    flex-direction: column;
                    margin-left: 10px;
                    line-height: 20px;
                    font-size: 14px;
                    color: #fff;
                    span {
                        color: #fff;
                        font-size: 14px;
                        padding: 2px 0;
                    }
                }
                .account-setting {
                    position: absolute;
                    top: 10px;
                    right: 20px;
                    font-size: 13px;
                    color: #fff;
                    .van-icon-setting-o {
                        font-size: 16px;
                        vertical-align: -3px;
                        margin-right: 4px;
                    }  
                }
            }
        }
        .user-list {
            padding: 0 8px;
            margin-top: 40px;
            li {
                display: flex;
                padding-left: 5px;
                padding-right: 5px;
                height: 40px;
                line-height: 40px;
                justify-content: space-between;
                font-size: 14px;
                .van-icon-arrow {
                    margin-top: 13px;
                }
                margin: 20px 0 !important;
                background: #FFFFFF;
                border-radius: 3px
            }
        }
    }
}
.logout {
    margin-top: 15px;
}
</style>
<!--
 * @Author: your name
 * @Date: 2021-04-30 19:48:40
 * @LastEditTime: 2021-05-30 21:40:44
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \Vue3project\ewshop\src\components\common\navbar\NavBar.vue
-->
<template>
    <div class="nav-bar">
        <div class="left" @click="goback">
            <slot name="left">
                <img src="@/assets/images/left.png" alt="">
            </slot>
        </div>
        <div class="center"><slot></slot></div>
        <div class="right">
            <slot name="right">
                <img src="@/assets/images/right.png" alt="">
            </slot>
        </div>
    </div>
</template>

<script>
import { useRouter } from "vue-router";

export default {
    name: "NavBar",
    setup() {
        
        const router = useRouter();
        const goback = () => {
            router.go(-1);
        }
        return {
            goback,
        }
    }
}
</script>

<style scoped>
.nav-bar {
    display: flex;
    background-color: var(--color-tint);
    color: #ffffff;
    /* pointer-events: fill;
    position: sticky; */
    position: fixed; 
    left: 0;
    right: 0;
    top: 0;
    z-index: 999;
    height: 45px;
    line-height: 45px;
    text-align: center;
    box-shadow: 0 2px 0px rgba(100, 100, 100, 0.1);
}
.left,.right {
    width: 60px;
}
.left img {
    width: 45px;
    padding: 12px;
}
.right img {
    width: 45px;
    padding: 12px;
}
.center {
    flex: 1;
}


</style>
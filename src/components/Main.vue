<template>
  <div id="main">
    <van-tabbar v-model="active" active-color='green'>
      <van-tabbar-item replace to="/main/home">
        <span>首页</span>
        <img slot="icon" slot-scope="props" :src="props.active ? home_icon.active : home_icon.normal">
      </van-tabbar-item>
      <van-tabbar-item replace to="/main/category">
        <span>分类</span>
        <img slot="icon" slot-scope="props" :src="props.active ? category_icon.active : category_icon.normal">
      </van-tabbar-item>
      <van-tabbar-item replace to="/main/cart" :info="goodsNum > 0 ? goodsNum : ''">
        <span>购物车</span>
        <img slot="icon" slot-scope="props" :src="props.active ? cart_icon.active : cart_icon.normal">
      </van-tabbar-item>
      <van-tabbar-item replace to="/main/mine">
        <span>我的</span>
        <img slot="icon" slot-scope="props" :src="props.active ? mine_icon.active : mine_icon.normal">
      </van-tabbar-item>
    </van-tabbar>
    <keep-alive>
      <router-view v-if='$route.meta.keepAlive'/>
    </keep-alive>
    <router-view v-if='!$route.meta.keepAlive'/>
  </div>
</template>

<script>
  export default {
    name: 'Main',
    data() {
      return {
        active: parseInt(sessionStorage.getItem('tabbarIndex')) || 0,
        home_icon: {
          normal: require('@/images/tabbar/home_default.png'),
          active: require('@/images/tabbar/home_selected.png')
        },
        category_icon: {
          normal: require('@/images/tabbar/category_default.png'),
          active: require('@/images/tabbar/category_selected.png')
        },
        cart_icon: {
          normal: require('@/images/tabbar/shoppingcart_default.png'),
          active: require('@/images/tabbar/shoppingcart_selected.png')
        },
        mine_icon: {
          normal: require('@/images/tabbar/mine_default.png'),
          active: require('@/images/tabbar/mine_selected.png')
        },
      }
    },
    watch: {
      active(value) {
        let tabbarIndex = value;
        sessionStorage.setItem('tabbarIndex', value);
      }
    },
    computed: {
      goodsNum() {

        if (this.$store.state.shopCart) {
          // 购物车商品数量
          let num = 0;
          Object.values(this.$store.state.shopCart).forEach((goods, index) => {
            if (goods.selected) {
            num += goods.count;
            }
          });
          return num;
        } else {
          return 0;
        }
      }
    },

   mounted() {
      // this.initCartData();
      this.$store.commit('initUserInfo');
      this.$store.commit('initCart');
    },
    methods: {
     /* initCartData() {
        this.$store.commit('initCart');
      } */
    }
  }
</script>

<style scoped="scoped">
  #main {
    /* width: 100%; */
    /* height: 100%; */
    /* background-color: gold; */
    margin-top: 1rem;
  }
</style>

<template>
  <div id="detail">
      <header>
          <span class="iconfont icon-left"></span>
          <span>商品列表</span>
          <span class="iconfont icon-gengduo"></span>
      </header>
    <main>
      
        <div class="tu">
            <img :src="'http://localhost:3000' + detailList.img" alt=""/>
        </div>
        <ul class="m_l">
          <li>
            <p>{{detailList.goodsname}}</p>
          </li>
          <li>
            <p>{{detailList.market_price}}</p>
          </li>
          <li class="num">
            <span>购买数量</span>
            <div>
              <button>-</button>
              <span>0</span>
              <button>+</button>
            </div>
          </li>
          <li class="search">
            <span>规格</span>
            <i v-for="(item,index) in searchlist" :key="index">{{item}}</i>
          </li>
        </ul>
      
      
    </main>
    <footer>
      <div class="cart">
        <p class="iconfont icon-gouwuche"></p>
        <span>购物车</span>
      </div>
      <div class="btn">
        <button class="yellow">加入购物车</button>
        <button class="red">结算</button>
      </div>
    </footer>
  </div>
</template>

<script>
export default {
  data() {
    return {
      id:'',
      detailList:[],
      searchlist:[]
    }
  },
  mounted() {
    this.$http.get('/getgoodsinfo',{id:this.$route.query.id}).then(res=>{
      console.log(res);
      this.detailList=res.data.list[0]
      let { specsattr }=res.data.list[0]
      this.searchlist=specsattr.split(',')
      console.log(this.searchlist);
    })
   
  },
};
</script>

<style lang="stylus" scoped>
#detail 
      width 100%
      height 100%
      display flex
      flex-direction column
      header
          width 100%
          height 0.88rem
          display flex
          justify-content space-between
          align-items center
          background-color color
          color #fff
      main 
          flex 1
          
         
      footer 
          height 1rem
          display flex
          .cart 
            height 100%
            width 1.5rem
            text-align center
          .btn 
            flex 1
            text-align center
            line-height 1rem
            font-size 0.2rem
            display flex
            .yellow 
                flex 1
                background-color #FFD700
            .red
                flex 1
                background-color #FF00FF
        
  

</style>
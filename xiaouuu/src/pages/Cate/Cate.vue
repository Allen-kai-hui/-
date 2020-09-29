<template>
  <div id="cate">
    <header>
      <span @click="back" class="iconfont icon-left"></span>
      <span>商品列表</span>
      <span class="iconfont icon-gengduo"></span>
    </header>
    <main>
      <ul id="left">
        <li
          @click="tabcate(index)"
          :class="{ active: index == ind }"
          v-for="(item, index) in cateList"
          :key="item.id"
        >
          {{ item.catename }}
        </li>
      </ul>
      <div id="right">
        <ul v-if="cateList.length > 0">
          <li
            @click="details(item.id)"
            v-for="(item) in cateList[ind].children"
            :key="item.id"
          >
            <div class="img-box">
              <img :src="'http://localhost:3000' + item.img" alt="" />
            </div>
            <p>{{ item.catename }}</p>
          </li>
        </ul>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  data() {
    return {
      cateList: [],
      ind: 0,
    };
  },
  methods: {
    tabcate(i) {
      this.ind = i;
    },
    details(id){
         this.$router.push("/home/index?id=" + id)
    },
    back(){
      console.log(this);
      this.$router.back()
    }
  },
  mounted() {
    this.$http.get("/getcatetree").then((res) => {
      console.log(res);
      this.cateList = res.data.list;
    });
  },
};
</script>

<style lang="stylus" scoped>
#cate 
  width: 100%;
  height: 100%;

  header 
    width: 100%;
    height: 0.88rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: color;
    color: #fff;

    span 
      padding-right: 0.3rem;

  main 
    width: 100%;
    height: 100%;
    display flex
    #left 
      width: 1.7rem;
      height: 100%;
      border-right: 0.01rem solid #ccc;

      li 
        height: 0.5rem;
        line-height: 0.5rem;
        margin: 0.25rem 0;
        padding-left: 0.3rem;
        border-left: 0.06rem;
        border-left: 0.06rem solid #fff
        &.active 
          border-left: 0.06rem solid color;
    #right
        padding-left .3rem
        flex 1
        ul 
            display flex
            flex-wrap wrap
            li 
                width 30%
                margin-right 3%
                height 2rem
                .img-box 
                    width 1.3rem
                    height 1.3rem
</style>
<template>
  <div class="index">
    <div v-if="!id" class="homediv">
      <header>
        <div class="logo">
          <img src="../../assets/logo.jpg" alt />
        </div>
        <input type="text" placeholder="寻找商品" />
        <span class="iconfont icon-gengduo"></span>
      </header>
      <div class="banner">
        <div class="swiper-container">
          <div class="swiper-wrapper">
            <div class="swiper-slide" v-for="item in bnanerlist" :key="item.id">
              <img :src="'http://localhost:3000' + item.img" alt />
            </div>
          </div>
        </div>
      </div>
      <nav>
        <div>
          <div class="imgs"><img src="../../assets/icon_1.jpg" alt="" /></div>
          <p>限时抢购</p>
        </div>
        <div>
          <div class="imgs"><img src="../../assets/icon_2.jpg" alt="" /></div>
          <p>积分商城</p>
        </div>
        <div>
          <div class="imgs"><img src="../../assets/icon_3.jpg" alt="" /></div>
          <p>联系我们</p>
        </div>
        <div>
          <div class="imgs" @click="goCart">
            <img src="../../assets/icon_4.jpg" alt="" />
          </div>
          <p>商品分类</p>
        </div>
      </nav>
      <div class="tab">
        <ul>
          <li
            :class="{ active: index == i }"
            @click="tabgoodsList(index)"
            v-for="(item, index) in tabList"
            :key="index"
          >
            {{ item }}
          </li>
        </ul>
        <GoodsList
          v-if="goodsList.length > 0"
          :goodsList="goodsList[i].content"
        ></GoodsList>
      </div>
    </div>
    <div class="home-list" v-if="id">
      <header>
        <i @click="back" class="iconfont icon-left"></i>
        <div class="logo2">
          <img src="../../assets/logo.jpg" alt />
        </div>
        <i class="iconfont icon-gengduo"></i>
      </header>
      <main>
        <div class="search-box">
          <input type="text" placeholder="搜索商品" />
          <i class="iconfont icon-sousuo"></i>
        </div>
        <div class="goods-list">
          <ul>
            <li
              @click="jump(item.id)"
              v-for="item in cateGoodsList"
              :key="item.id"
            >
              <div class="img-box">
                <img :src="'http://localhost:3000' + item.img" alt="" />
              </div>
              <div class="content">
                <p>{{ item.goodsname }}</p>
                <p>{{ item.price }}</p>
              </div>
            </li>
          </ul>
        </div>
      </main>
    </div>
  </div>
</template>

<script>
import Swiper from "swiper";
import GoodsList from "@/components/GoodsList";
export default {
  data() {
    return {
      id: "",
      i: 0,
      bnanerlist: [],
      goodsList: [],
      tabList: ["最热商品", "最新商品", "全部商品"],
      cateGoodsList: [],
    };
  },
  components: {
    GoodsList,
  },
  watch: {
    $route: {
      deep: true,
      handler(newVal) {
        console.log(newVal);
        this.id = newVal.query.id;
      },
    },
  },
  mounted() {
    this.id = this.$route.query.id;
    console.log(this.id);
    this.$http.get("/getgoods", { fid: this.id }).then((res) => {
      console.log(res);
      this.cateGoodsList = res.data.list;
    });
    this.$http.get("/getindexgoods").then((res) => {
      console.log(res);
      this.goodsList = res.data.list;
    });
    this.$http.get("/bannerlist").then((res) => {
      console.log(res);

      this.bnanerlist = res.data.list;
      console.log(this);
      this.$nextTick(() => {
        var mySwiper = new Swiper(".swiper-container", {
          autoplay: {
            delay: 2000,
          },
        });
      });
    });
  },
  methods: {
    tabgoodsList(i) {
      this.i = i;
    },
    goCart() {
      this.$router.push("/cate");
    },
    back() {
      console.log(this);
      this.$router.back();
    },
    jump(id) {
      this.$router.push("/detail?id=" + id);
    },
  },
};
</script>

<style lang="stylus" scoped>
.index {
  width: 100%;
  height: 100%;

  .homediv {
    header {
      width: 100%;
      height: 0.88rem;
      display: flex;
      justify-content: space-between;
      align-items: center;

      .logo {
        width: 2rem;
        height: 0.4rem;
        padding-left: 0.2rem;
      }

      input {
        width: 3rem;
        height: 0.4rem;
        background-color: #eee;
        text-align: center;
      }

      span {
        padding-right: 0.2rem;
      }
    }

    .banner {
      margin-bottom: 0.3rem;
      width: 100%;
      height: 2.9rem;

      .swiper-container, .swiper-wrapper, .swiper-slide {
        width: 100%;
        height: 100%;
      }
    }

    nav {
      display: flex;
      height: 1.18rem;
      align-items: center;
      margin: 0.2rem 0;

      div {
        flex: 1;
        text-align: center;

        .imgs {
          height: 0.8rem;
          width: 0.8rem;
          margin: auto;
        }
      }
    }

    .tab {
      width: 100%;
      height: 0.66rem;

      ul {
        display: flex;

        li {
          flex: 1;
          text-align: center;
          line-height: 0.66rem;
          border: 0.01rem solid #ccc;

          &.active {
            background-color: color;
            color: #fff;
          }
        }
      }
    }
  }

  .home-list {
    header {
      height: 0.88rem;
      background-color: #fff;
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 0 0.3rem;
      line-height: 0.88rem;

      .logo2 {
        width: 2rem;
        height: 0.4rem;
      }
    }

    main {
      .goods-list {
        li {
          display: flex;
          height: 2rem;

          .img-box {
            width: 1.7rem;
          }

          .content {
            flex: 1;
          }
        }
      }

      .search-box {
        width: 6.2rem;
        height: 0.8rem;
        margin: 0.3rem auto;
        position: relative;

        input {
          width: 100%;
          height: 100%;
          border-radius: 0.4rem;
          background-color: color;
          text-align: center;
          text-indent: 0.3rem;

          &::-webkit-input-placeholder {
            color: #fff;
          }
        }

        i {
          position: absolute;
          left: 2.1rem;
          top: 50%;
          transform: translateY(-50%);
          color: #ffffff;
          font-size: 0.4rem;
        }
      }
    }
  }
}
</style>






<template>
    <div class="home">
        <article class="newhome" v-cloak >
            <!--lbox begin-->
            <div class="lbox" v-loading="pictLoading" element-loading-background="rgba(247, 245, 245, 0.7)" element-loading-text="Loading..." >
                <div class="ad whitebg"></div>
                <template v-if="isShow" >
                    <div class="whitebg bloglist">
                        <h2 class="htitle">最新博文
                            <router-link :to="'/Editor'" class="prev" style="float:right;" rel="prev">
                                Add My Study
                            </router-link>
                        </h2>
                        <ul>
                            <div v-for="i in 6" :key="i" v-cloak>
                                <el-row :gutter="20">
                                    <el-col :span="7">
                                        <div class="grid-content bg-purple"></div>
                                    </el-col>

                                </el-row>
                                <el-row :gutter="24">
                                    <el-col :span="24">
                                        <div class="grid-content bg-purple"></div>
                                    </el-col>
                                </el-row>
                                <p class="bloginfo">
                                    <i  class="avatar"></i>
                                    <span>技术博文</span>
                                    <span>2019-01-01</span>
                                    <span>【<a  href="/">Author</a>】</span>
                                    <a href="/" class="viewmore-row">更多</a>
                                </p>
                            </div>
                        </ul>
                        <nav id="page-nav">
                            <router-link :to="'/?page=' + (page>1?page-1:1)" class="prev" rel="prev">{{(page>1? "pre": "")}}</router-link>
                            <router-link :to="'/?page=' + (page>=TotalCount? TotalCount: page+1)" class="next" rel="next">
                                {{(page>=TotalCount? "": "Next")}}
                            </router-link>
                        </nav>
                    </div>
                </template>
                <div v-else name="fade" mode="out-in"  class="whitebg bloglist">
                    <h2 class="htitle">最新博文
                        <router-link :to="'/Editor'" class="prev" style="float:right;" rel="prev">
                            Add My Study
                        </router-link>
                    </h2>
                    <ul>
                        <!--单图-->
                        <li v-for="i in list" :key="i.bID">
                            <h3 class="blogtitle">
                                <router-link :to="'/content/' + i.bID">
                                   <span  v-cloak>{{ i.btitle }}</span>
                                </router-link>
                            </h3>
                            <span class=""><i><a href="/"></a></i><a href="/" title=""></a></span>
                            <p class="blogtext" v-text="i.bRemark"></p>
                            <p class="bloginfo"><i class="avatar"></i>
                                <span  v-cloak>{{i.bcategory}}</span>
                                <span  v-cloak>{{formatCreateTime(i)}}</span>
                                <span  v-cloak>【<a href="/">{{i.bsubmitter}}</a>】</span>
                            </p>
                            <a href="/" class="viewmore">更多</a></li>

                    </ul>
                    <nav id="page-nav">
                        <router-link :to="'/?page=' + (page>1?page-1:1)" class="prev" rel="prev">
                            {{(page>1? "pre": "")}}
                        </router-link>
                        <router-link :to="'/?page=' + (page>=TotalCount? TotalCount: page+1)" class="next" rel="next">
                            {{(page>=TotalCount? "": "Next")}}
                        </router-link>
                    </nav>
                </div>
                <!--bloglist end-->
            </div>
            
        </article>
        

    </div>
</template>

<script>
// @ is an alias to /src
import util from "../date";

export default {
  name: "home",
  components: {},
  data() {
    return {
      page: 1,
      TotalCount: 1,
      pictLoading: true,
      isShow: true,
      list: []
    };
  },
  created() {},
  mounted() {
    this.getData();
  },
  methods: {
    formatCreateTime: function(row) {
      return !row.bCreateTime || row.bCreateTime == ""
        ? ""
        : util.formatDate.format(new Date(row.bCreateTime), "yyyy-MM-dd");
    },
    getData() {
      var that = this;
      var urlPage = that.$route.query.page;
      if (urlPage) {
        that.page = urlPage;
      }
      this.$api.get("Blog/?page=" + that.page, { test: 33 }, r => {
        this.list = r.data;
        this.page = r.page;
        this.TotalCount = r.pageCount;
        this.isShow = false;
        this.pictLoading = false;
      });
    }
  },
  watch: {
    $route(to) {
      this.list = [];
      this.isShow = true;
      this.page = to.query.page;
      this.getData();
    }
  }
};
</script>

<style>
.el-row {
  margin-bottom: 20px;
}

.el-col {
  border-radius: 4px;
}

.bg-purple-dark {
  background: #99a9bf;
}

.bg-purple {
  background: #d3dce6;
}

.bg-purple-light {
  background: #e5e9f2;
}

.grid-content {
  border-radius: 4px;
  min-height: 36px;
}

.row-bg {
  padding: 10px 0;
  background-color: #f9fafc;
}

.viewmore-row {
  float: right;
  background: #12b7de;
  color: #fff;
  border-radius: 3px;
  padding: 0px 10px;
  height: 30px;
}
.bg-purple {
  background: #eaeaea;
  -webkit-animation: loading 1s ease-in-out infinite;
  animation: loading 1s ease-in-out infinite;
}
@keyframes loading {
  0% {
    width: 90%;
  }
  50% {
    width: 100%;
  }
  to {
    width: 90%;
  }
}
[v-cloak] {
  display: none !important;
}
</style>

<template>
<div class="showContent">
    <!-- <div v-show="false">{{ updateArt }}</div> -->
    <template v-if="getArticleDetail!==null">
        <div class="showtitle" >
            <!-- <Tag color="orange" style="width:100%;height:80px; text-align: center;font-size:20px;line-height:80px;" >专栏标题</Tag> -->
            <div class="info_title" >
                <span style="height:30px">{{ Article.articleTitle }}</span>
            </div>
            <div class="info_time">
            时间:{{Article.createTime}}&nbsp;&nbsp;&nbsp;&nbsp;来源 : 
            <a href="">{{ Article.userId }}</a>
            </div>
        </div>
    </template>
    <xmp ref="showinfo" class="articleText">{{Article.articleContent}}</xmp>
</div>
</template>

<script>
// import { getArticle } from '../../api/articleList.js'
// import axios from 'axios'
export default {
  data () {
    return {
      Article: {
        articleTitle: '',
        userId: '',
        articleContent: '',
        createTime: ''
      }
    }
  },
  created () {
    this.getArticleDetail()
  },
  computed: {
    getArticleDetail () {
      console.log('获取到的路径参数' + this.$route.params.id)
      this.$http({
        url: this.$http.adornUrl('/article/getArticleById/' + this.$route.params.id),
        method: 'get',
        params: this.$http.adornParams({
              // 'page': this.pageIndex,
              // 'limit': this.pageSize,
              // 'roleName': this.dataForm.roleName
              // 'keyword': this.dataForm.articleName
        })
      }).then(({data}) => {
      // ArticleEntity:
      // articleContent: "测试104"
      // articleId: 1003
      // articleTitle: "测试104"
      // createTime: "2020-10-30 12:00:21"
      // userId: 1
        console.log(data)
        console.log(data != null && data.ArticleEntity.length > 0)
        if (data != null && data.ArticleEntity != null) {
          this.Article = data.ArticleEntity
          return data.ArticleEntity
        }
        console.log(this.Article)
      })
    }
        // updateArt(){
        //     getArticle(this.$route.params.id).then(res=>{
        //     // console.log(res.data);
        //          if(res.data.content != null)
        //          {
        //         this.$refs.showinfo.innerHTML  = res.data.content;
        //          }
        //         this.Article = res.data;
        //         // console.log("文章成功")
        //         // console.log("文章id"+this.$route.params.id);
        //         // console.log(res.data);
        //         // console.log(this.$route)
        //     })
        //     return this.$route.params.id
        // }
  },
  methods: {
    //     getNewwork (){
    //         axios({
    //     url: "http://192.168.1.57:8080/admin/collectdata/downloadfile?fileName=%E6%A0%A1%E5%8F%B2%E9%A6%86.png&filePath=collectdata%2F20200105%2F20200105184659923.png",
    //     method: "get",
    // }).then(res => {
    //     // console.log("请求成功")

    //     // console.log(res.data);
    //     // return Promise.resolve(res.data)

    // }, rej => {
    //     console.log(rej)
    //     return Promise.reject("请求失败")
    // })
    //     }
  }
}
</script>


<style scoped>
.info_title{
    margin: 20px 0;
    text-align: center;
    font-size: 23px;
    font-weight: 700;
    color: #D0A972;
    width: 100%;
    /* display: inline-block; */
    letter-spacing: 1px;
}
.info_time{
    color: #aaa;
    padding: 10px;
    text-align: center;
    border-bottom: solid 1px #eee;
    width: 100%;
    height: auto;
    font-size: 12px;
}
.articleText{
  text-align: center;
  font-size: 30px;
}
</style>
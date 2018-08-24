<template>
  <div class="index">
    <Card>
      <div class="index-card-header" slot=header>
        <img src="http://placeholder.qiniudn.com/640x300" style="width:100%;display:block;">
      </div>
      <div class="index-card-content" slot="content">
        <p>哈哈哈哈哈</p>
      </div>
      <div class="index-card-footer" slot="footer">
        <p>哈大大</p>
      </div>
    </Card>
  </div>
</template>

<script>
import { Card } from "vux";
import axios from 'axios'
export default {
    data() {
        return {};
    },
    methods:{
      getLists() {
        axios.get('https://op.juhe.cn/onebox/basketball/nba?key=3e99d92bd400bd5db7d67d732e434b2c')
        .then(res => {
          console.log(res)
        })
      }
    },
    components: { Card },
    created(){
      this.getLists();
    }
};
</script>

<style scoped lang="scss">
  .index{
    &-card-header{
      width: 100%;
      max-height: 200px;
      overflow: hidden;
      img{
        width: 100%;
        height: 100%;
        object-fit: 100%;
      }
    }
    &-card-content{
      padding: 20px 10px;
      p{
        text-align: left;
      }
    }
  }
</style>

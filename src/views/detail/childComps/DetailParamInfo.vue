<template>
  <div v-if="Object.keys(paramInfo).length !== 0" class="param-info">
    <table v-for="(table, index) in paramInfo.rule" class="info-size" :key="index">
      <tr v-for="(tr, indey) in table" :key="indey">
        <td v-for="(td, indez) in tr" :key="indez">{{td}}</td>
      </tr>
    </table>
    <table class="info-param">
      <tr v-for="(info, index) in paramInfo.info">
        <td class="info-param-key">{{info.key}}</td>
        <td class="params-value">{{info.value}}</td>
      </tr>
    </table>
    <div class="info-img" v-if="paramInfo.image.length !== 0">
      <img :src="paramInfo.image" alt="">
    </div>
  </div>
</template>
<script>
  export default {
    name:'DetailParamInfo',
    props: {
      paramInfo: {
        type: Object,
        default() {
          return {}
        }
      }
    }
  }
</script>
<style scoped>
  .param-info{
    padding: 20px 15px;
    font-size: 14px;
    border-bottom: 5px solid #f2f5f8;
  }
  .param-info table{
    width: 100%;
    border-collapse: collapse;
  }
  .param-info table tr {
    height: 42px;
  }
  .param-info table tr td {
    border-bottom: 1px solid rgba(100, 100, 100, .1);
  }
  .info-param-key{
    width: 95px;
  }
  .info-param {
    border-top: 1px solid rgba(0,0,0,.1);
  }
  .params-value {
    color: #eb4868;
  }
  .info-img img {
    width: 100%;
  }
</style>
<!--<template>-->
<!--  <div v-if="Object.keys(paramInfo).length !== 0" class="params-wrap">-->
<!--    <div v-for="(item, index) in paramInfo.rule">-->
<!--      <div v-for="(list, index) in item" class="flex">-->
<!--        <div v-for="(listitem, index) in list" class="rule-list-item">-->
<!--          {{listitem}}-->
<!--        </div>-->
<!--      </div>-->
<!--    </div>-->
<!--    <div v-for="(info, index) in paramInfo.info" :key="index" class="flex info-list-wrap">-->
<!--      <div class="info-list-tit">{{info.key}}</div>-->
<!--      <div>{{info.value}}</div>-->
<!--    </div>-->
<!--  </div>-->
<!--</template>-->

<!--<script>-->
<!--export default {-->
<!--  name:'DetailParamInfo',-->
<!--  props: {-->
<!--    paramInfo: {-->
<!--      type: Object,-->
<!--      default() {-->
<!--        return {}-->
<!--      }-->
<!--    }-->
<!--  }-->
<!--}-->
<!--</script>-->

<!--<style  scoped>-->
<!--.params-wrap {-->
<!--  border-top: 4px solid #ececec;-->
<!--  border-bottom: 4px solid #ececec;-->
<!--}-->
<!--.rule-list-item {-->
<!--  font-size: 12px;-->
<!--  width: 20%;-->
<!--  border-bottom: 1px solid #ececec;-->
<!--  padding: 10px 4px;-->
<!--}-->
<!--.info-list-wrap {-->
<!--  font-size: 14px;;-->
<!--  border-bottom: 1px solid #ececec;-->
<!--  padding: 10px 4px;-->
<!--  line-height: 20px;-->
<!--}-->
<!--.info-list-tit {-->
<!--    width: 18%;-->
<!--  }-->

<!--</style>-->

<template>
<div class="commentbox" v-if="getcommentbox">
  <div class="commenttitle">
    <h3>评一下吧</h3>  
    <span class="pull-right" id="add" @click="closeCommentbox">取消</span>
    <span class="pull-right" id="sure" @click="commentSure">确定</span>
  </div>
  <div class="commentcontent">
  <table>
    <tr>
      <td>添加tag：</td>
      <td>
         <input type="text" class="tag_input" v-model="tag">
      </td>
    </tr>     
    <tr>
      <td>评个分：</td>
      <td>
        <div class="scorebox">
          <span class="reduction pull-left" v-on:click="changeNum(score,-1)">-</span>
          <span type="text"  class="score_value">{{score}}</span>
          <span class="plus pull-right" v-on:click="changeNum(score,1)">+</span>
        </div>
      </td>
    </tr> 
    <tr>   
      <td>我的状态：</td>
      <td>
        <input type="radio" name="state" v-model="state" class="state"  id="state-1"><label for="state-1" type="button" class="btn btn-sm btn-default">想看</label>
        <input type="radio" name="state"  v-model="state" class="state"  id="state-2"><label for="state-2" type="button" class="btn btn-sm btn-default">再看</label>
        <input type="radio" name="state"  v-model="state" class="state"  id="state-3" checked><label for="state-3" type="button" class="btn btn-sm btn-default" >看过</label>
        <input type="radio" name="state"  v-model="state" class="state"  id="state-4"><label for="state-4" type="button" class="btn btn-sm btn-default">搁置</label>
        <input type="radio" name="state"  v-model="state" class="state"  id="state-5"><label for="state-5" type="button" class="btn btn-sm btn-default">抛弃</label>     
      </td>
    </tr>
  </table>
  <textarea v-model="text"></textarea>
  </div>
</div>
</template>
<script>
import {mapGetters} from 'vuex';
export default {
  data () {
    return {
      tag: '',
      score: 9,
      state: '',
      text: ''
    }
  },
  props: {
    collects: {
      type: Array
    }
  },
  methods: {
    closeCommentbox ( ) {
      this.$store.commit('changecommentbox')
    },
    commentSure(){
      var tag = this.tag;
      var score = this.score;
      var state = this.state;
      var text = this.text;
      console.log(tag+'---------'+score+'------'+state+'----'+text);
      if (tag !== '' && score !== '' && state !== '' && text !== '') {
        var data = {
          tag: tag,
          score: score,
          state: state,
          text: text
        }
        console.log(data);
        this.$store.commit('changecommentbox')
        this.$store.commit('changedialog')
        this.$store.commit('changedialoginfo', '评论成功！！！')
      } else {
        this.$store.commit('changedialog')
        this.$store.commit('changedialoginfo', '一定是忘记输入什么啦！！！')
      }
    },
    changeNum: function(score, way){
      if (way>0 && score<10 ){
        score++;
      } else if ( way<0 && score>1 ){
        score--;
      }
    }
  },
  computed: {
    ...mapGetters([
      'getcommentbox'
    ])
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.commentbox {
  padding-top: 50px;
  width: 100%;
  height:100%;
  overflow:hidden;
  position: fixed;
  border-top:1px solid #AAA9A7;
  color:#000;
  left: 0;
  bottom:-50px;
  background-color:#fff;}
.commenttitle{
  line-height:40px;
  height:40px;
  border-bottom: 1px dotted #aaa;
  font-weight: bold;}
.commenttitle h3{
  font-weight:bold;
  padding-left: 5%;
  display: inline-block;}
.commenttitle span{
  padding-right: 5%;
  line-height:40px;
  height:40px;
  font-size: 14px;
  font-weight: normal;}
.commentcontent{
  width: 100%;
  padding:5%;
  font-size: 14px;}
.commentcontent table{
  width: 100%}
.commentcontent table tr{
  padding: 10px 0;
  line-height: 40px;
  height:40px;
}
.tag_input{
  border:1px solid #ccc;
  height: 28px;
  line-height: 28px;
}
/********实现切换效果**********/
.state{
  display:none;}
.state:checked+label{
  color:#FFF;
  font-weight:bold;
  background-color:#C00;}
.score { 
  color:#F66;
  margin-left:5px;
  font-size:20px;}
textarea{
  width:100%;
  height:auto;
  min-height:100px;
  margin-top:20px;
border:#ccc solid 1px;}
.scorebox{
  height:100%;
  width: 80px;
  border:#ccc solid 1px;
  line-height: 28px;
text-align:center;}
.reduction{border-right:#ccc solid 1px;}
.plus{border-left:#ccc solid 1px;}
.reduction,.plus{
  height:100%;
  font-size: 18px;
  width:20px;
  line-height: 28px;
  background-color:#f5f5f5;
  display: inline-block;
}
.score_value{
  height:100%;
}
.commentbtn{margin-right:1rem;}
</style>

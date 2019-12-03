<template>
  <el-row>
    <el-col>
      <el-col :span="6">样品编号</el-col>
      <el-col :span="12">样品名称</el-col>
      <el-col :span="6">操作</el-col>
    </el-col>
    <el-divider></el-divider>
    <el-col v-for="(item, index) in list.items" :key="index" style="padding-bottom: 10px;">
      <el-col :span="6">{{item.number}}</el-col>
      <el-col :span="12">{{item.name}}</el-col>
      <el-col :span="6"><span style="cursor: pointer;" @click="getDetail(item.id)">查看详细</span></el-col>
      <el-divider></el-divider>
    </el-col>
    <el-dialog
      title="详细"
      :visible.sync="detaildBox"
      width="50%">
      <el-row>
        <el-col>
          <el-col :span="8">样品编号</el-col>
          <el-col :span="6">样品名称</el-col>
          <el-col :span="6">样品检测项目</el-col>
          <el-col :span="4">样品检测结果</el-col>
        </el-col>
        <el-divider></el-divider>
        <el-col>
          <el-col :span="8">{{detail.number}}</el-col>
          <el-col :span="6">{{detail.name}}</el-col>
          <el-col :span="6">{{detail.model}}</el-col>
          <el-col :span="4">{{detail.result}}</el-col>
          <el-divider></el-divider>
        </el-col>
      </el-row>
      <span slot="footer" class="dialog-footer">
        <el-button @click="detaildBox = false">取 消</el-button>
        <el-button type="primary" @click="detaildBox = false">确 定</el-button>
      </span>
    </el-dialog>
    <div>
    <el-button type="primary" @click="mockData">mock</el-button>
  </div>
  </el-row>
</template>

<script>
import axios from 'Axios'
import Axios from 'Axios';
export default {
  data () {
    return {
      list: '',
      detail:'',
      activeName:'',
      detaildBox: false,
    }
  },
  methods:{
    mockData: function(){
      axios.post('/user', {
        firstName: 'Fred',
        lastName: 'Flintstone'
      })
      .then(function (response) {
        console.log(response);
      })
      .catch(function (error) {
        console.log(error);
      });
    },
    getdata:function(){
      let self = this;
      axios.get('https://lims-dev.im-cc.com/interview/api/restful/query/__::__/model.runtime.sample.Sample/all?equal_filter=model.runtime.sample.Sample.application_id__1')
      .then(function (response) {
        console.log(response.data,'res')
        self.list = response.data;
      })
      .catch(function (error) {
        console.log(error);
      });
    },
    getDetail:function(id){
      let self = this;
      axios.get('https://lims-dev.im-cc.com/interview/api/restful/entity/__::__sample_items.item/model.runtime.sample.Sample/'+id)
      .then(function (response) {
        console.log(response.data,'detail')
        self.detail = response.data;
        self.detaildBox = true;
      })
      .catch(function (error) {
        console.log(error);
      });
    },
    seeDetail(){
      this.detaildBox = true;
    },
  },
  created(){
    this.getdata();
    // this.getDetail()
  }
  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>

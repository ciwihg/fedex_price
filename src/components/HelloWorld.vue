<template>
  <div class="hello">
    <div class="m-title">目的地</div>
    <el-autocomplete
    class="m-destination"
      v-model="country"
      :fetch-suggestions="querySearch"
      placeholder="请输入目的地"
      :trigger-on-focus="false"
      @select="handleSelect"
    ></el-autocomplete>
    <div class="m-title">包装服务</div>
    <el-select class="m-input" v-model="paktype" placeholder="请选择包装">
    <el-option
      v-for="item in pakoptions"
      :key="item.value"
      :label="item.label"
      :value="item.value">
    </el-option>
    </el-select>
    <div class="m-title">重量</div>
    <el-input  class="m-input m-inputw" placeholder="请输入重量" v-model="weight"></el-input>

    <div><el-button class="m-input" type="primary" @click="getPrice">查询</el-button></div>
    <div class="m-price">运费:CNY{{price}}</div>
  </div>
</template>

<script>
import axios from 'axios';

axios.defaults.baseURL = `https://easyhome.applinzi.com/public/index.php/fedex`;
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data:function(){
    return {
      country: '',
      weight:"",
      price:"",
      ccode:"",
      paktype:"",
      pakoptions:[
        {value:'evo',label:"信封袋"},
        {value:'pak',label:"包裹袋"},
        {value:'package',label:"纸箱"},
      ]
    };
  },
  methods:{
    querySearch(queryString,cb) {
    //  var results = queryString;

      axios.post('/querypage/querycountry',{country:this.country},{
        headers: {'Content-Type': 'application/x-www-form-urlencoded'},
        transformRequest:[(data)=>`data=`+JSON.stringify(data)]
       })
      .then(function (response) {
         console.log(response);

           cb(response.data);
    })
    .catch(function (error) {
      console.log(error);
    })

    },
    handleSelect(item){
    this.ccode=item.code;
    },
    getweightgrade(w){
      var wl;
        var result=w.match(/(.+)(\..+)/);
      if(result){
        (0<result[2]&&result[2]<=0.5)?(wl=parseInt(result[1])+0.5):(result[2]==0)?(wl=result[1]):(wl=parseInt(result[1])+1);
        return wl;
      }else{
        wl=w;
        return wl;
      }


    },
    getPrice(){
      var vm=this;
      var w=this.getweightgrade(this.weight);
      if(this.paktype=="evo"&&w>0.5){
        this.$alert('信封袋包装,重量不能超过0.5KG', '提示', {
       confirmButtonText: '确定',
       callback:()=>{
         this.price="";
       }
     });
        return;
      }
      if(this.paktype=="pak"&&w>2.5){
        this.$alert('包裹袋包装,重量不能超过2.5KG', '提示', {
       confirmButtonText: '确定',
       callback:()=>{
         this.price="";
       }
     });
        return;
      }
      axios.post('/querypage/queryprice',{code:this.ccode,weight:w,type:this.paktype},{
        headers: {'Content-Type': 'application/x-www-form-urlencoded'},
        transformRequest:[(data)=>`data=`+JSON.stringify(data)]
       })
      .then(function (response) {
        console.log(response.data);
         vm.price=response.data[0].price;

    })
    .catch(function (error) {
      console.log(error);
    })

    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.m-inputw{
  width:30%;
}
.m-destination{
  width:40%;
}
.m-input{
  margin: 20px 0;
}
.m-title{
  margin: 20px 0;
}
h3 {
  margin: 40px 0 0;
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
.hello{
  margin-top: 60px;
  width: 100%;
  position: fixed;
  left: 0;
  top:0;
}
</style>

<script>
export default{
  components:{

  },

  data() {
    return {
      city:null,
      temp:null,
      describe:null,
      img:null
    }
  },

  methods: {
    getInfo(){
      fetch("https://opendata.cwb.gov.tw/api/v1/rest/datastore/F-C0032-001?Authorization=CWB-CEF7D024-39BA-4493-8D04-1A3875E8971A")
      .then(res => res.json())
      .then(data => {
      console.log(data)
      this.city = data.records.location[6].locationName;
      this.temp = data.records.location[6].weatherElement[4].time[0].parameter.parameterName
      this.describe = data.records.location[6].weatherElement[0].time[0].parameter.parameterName
      
      
    })
    },

    loadImg(e){
      const sourceImg = e.target.files[0]
      
      const reader = new FileReader();
      // 非同步
      reader.readAsDataURL(sourceImg);
      reader.onload = (e) => {
        // console.log(e.target.result)
        this.img = e.target.result
        console.log(this.img)
        
      }

    }


  },

  mounted(){
  
  }

}

</script>

<template>
  <div>
    <button type="button" @click="getInfo">getInfo</button>
    <h1 v-if="city !== null" >城市:{{ city }}</h1>
    <h1 v-if="temp !== null" :class="{hot: temp > 25}">氣溫:{{ temp }}度</h1>
    <h1 v-if="describe !== null" >描述:{{ describe }}</h1>


    <!-- 單向數據流 -->
    <input type="file" @change="loadImg">
    <img :src="img">
  </div>
</template>

<style scoped>
.hot{
  color:red;
}

</style>

<template>
  <div class='app-inner'>
    <button @click='show_one'> 点击显示一个点 </button> 
    <div id="container"></div>
  </div>
</template>

<script>
import AMapLoader from '@amap/amap-jsapi-loader';
// import Vue from 'vue/dist/vue.esm.js';

export default {
  name: 'Map',
  data() {
    return {
      map: null,
      marker: null,
      geocoder: null,
      autoComplete: null,
      address: '',
      infoWindow: null,
      arrivalRange: null,
      polygons: [],
      companyPosition: [],
      travelTime: 30,
      travelMethod: 0,
    };
  },
  computed: {

  },
  created() {},
  mounted () {
    AMapLoader.load({
        "key": "2b2fa2dca30400a380445eb87ce96229",   // 申请好的Web端开发者Key，首次调用 load 时必填
        "version": "2.0",   // 指定要加载的 JSAPI 的版本，缺省时默认为 1.4.15
        "plugins": [
          'AMap.ToolBar',
          'AMap.PlaceSearch',
          'AMap.Scale',
          'AMap.Geocoder',
          'AMap.AutoComplete',
          'AMap.Marker',
          'AMap.InfoWindow',
          'AMap.Event',
          'AMap.Icon',
          'AMap.Polygon',
          'AMap.ArrivalRange',
          //'AMap.MarkerCluster',

        ],  // 需要使用的的插件列表，如比例尺'AMap.Scale'等
    }).then((AMap)=>{
        var city="北京"
        this.map = new AMap.Map('container', {'zoom': 9});
        const toolbar = new AMap.ToolBar();
        const scale = new AMap.Scale();
        this.map.addControl(toolbar);
        this.map.addControl(scale);
        this.geocoder = new AMap.Geocoder({});
        this.marker = new AMap.Marker({});
        this.infoWindow = new AMap.InfoWindow({});
        this.arrivalRange = new AMap.ArrivalRange();
        this.AMap = AMap;
        const autoOptions = {
          //city 限定城市，默认全国
          city: city,
          input: 'input',
        }
        this.autoComplete = new AMap.AutoComplete(autoOptions);
        this.placeSearch = new AMap.PlaceSearch({
          city: city,
          //map: this.map // 因为要自定义窗口，所以自己来标记点以及写
        });
        this.autoComplete.on('select', this.select);
    }).catch(e => {
        console.log(e);
    })

    //this.showAllHouses();
  },
  methods: {
    show_one()
    {
      new this.AMap.Marker({
        map: this.map,
        position: ['116.42', '39.9'],
        icon: '//vdata.amap.com/icons/b18/1/2.png',
        offset: new this.AMap.Pixel(-10, -10),
        anchor: 'center',
      })
      console.log('show point');
      this.map.setFitView();

    },


  },
};
</script>

<style lang='less' scoped>
.app-inner {
  text-align: center;
}

#container {
  margin: 30px auto;
  width: 750px;
  height: 300px;;
}

.input {
  &-wrap {
    margin: 20px 0;
  }

  &-box {
    position: relative;
    display: inline-block;
  }

  &-text {
    width: 300px;
    height: 30px;
  }

  &-btn {
    padding: 0 10px;
    height: 36px;
  }

  &-result {

    &__list {
      padding-bottom: 10px;
      position: absolute;
      top: 30px;
      left: 0;
      z-index: 99;
      width: 100%;
      background-color: #fff;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    }

    &__item {
      padding: 0 10px;
      width: 100%;
      line-height: 30px;
      text-align: left;
      border-bottom: 1px solid #eee;

      &:last-child {
        border-bottom: 0;
      }
    }
  }
}

.map-img {
  margin: 0 0 30px;
}


</style>
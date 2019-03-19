<template>
  <div id="menuApp">
    <div class="row">
      <div class="col-sm-12 col-md-7">
        <table  class="table">
          <thead>
            <tr>
              <th>尺寸</th>
              <th>价格</th>
              <th>加入</th>
            </tr>
          </thead>
          <tbody v-for="list in lists" :key="list.name">
            <tr>
              <td>
                <img :src="list.url" alt="" class="img-responsive imgBox">
                <strong>{{list.name}}</strong>
              </td>
            </tr>
            <tr v-for="option in list.options" :key="option.size">
              <td>{{option.size}}</td>
              <td>{{option.price}}</td>
              <td>
                <button class="btn btn-default" v-on:click.prevent="add(list,option)">+</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
      <!-- 购物车 -->
      <div class="col-md-5 col-sm-12">
        <div v-if="baskets.length>0">
           <table class="table">
          <thead>
            <tr>
              <th>数量</th>
              <th>品种</th>
              <th>价格</th>
            </tr>
          </thead>
          <tbody >
            <tr v-for="item in baskets">
              <td>
                <img :src="item.url" alt="" class="imgBox2">
                <button class="btn btn-sm" v-on:click='reduce(item)'>-</button>
                <span>{{item.num}}</span>
                <button class="btn btn-sm" v-on:click='increase(item)'>+</button>
              </td>
              <td>{{item.name}} {{item.size}}</td>
              <td>{{item.price*item.num}}</td>
            </tr>
        <tr>总计：{{sum+'元'}}</tr>
          </tbody>
        </table>
        </div>
        <div v-else='baskets.length<=0' class="empty">{{basketText}}</div>
      
       
      </div>
    </div>

    <!-- {{baskets}} -->
  </div>
</template>

<script>
export default {
  name: "menuApp",
  data() {
    return {
      basketText:'购物车空空如也',
      baskets: [],
      lists: [
        {
          name: "榴莲Pizaa",
          decription: "这是喜欢吃榴莲的朋友最佳的选择",
          url:'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1553009409682&di=2aea2df701a5da590349f6298b778b86&imgtype=0&src=http%3A%2F%2Fimg06.tooopen.com%2Fimages%2F20160729%2Ftooopen_sy_173149363327.jpg',
          options: [{ size: 9, price: 38 }, { size: 12, price: 48 }]
        },
        {
          name: "苹果Pizaa",
          decription: "苹果味真的不错哦",
           url:'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1553009409683&di=afb36948013df0ddf247bb91db9789d3&imgtype=0&src=http%3A%2F%2Fdocs.ebdoor.com%2FImage%2FInfoContentImage%2FProductDesc%2F2015%2F11%2F13%2F74%2F74102d06-b130-46a9-bb06-f47b87e057e7.jpg',
          options: [{ size: 9, price: 38 }, { size: 12, price: 48 }]
        },
        {
          name: "芝士Pizaa",
          decription: "芝士杀手，浓浓的芝士味",
           url:'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1553009409683&di=1ca9d5e715d631d9055d1fc1d50871e0&imgtype=0&src=http%3A%2F%2Fimg004.hc360.cn%2Fm8%2FM08%2F66%2FC3%2FwKhQpVWU0w2ECOPCAAAAALfd1X0738.jpg',
          options: [{ size: 9, price: 38 }, { size: 12, price: 48 }]
        }
      ]
    };
  },
  methods: {
    add(list,option) {
      let basket={
        name: list.name,
        url:list.url,
        price:option.price,
        size: option.size,
        num:1
      }
      //判断购物车  购物车大于0 判断是否添加重复的，过滤
      // 购物车小于0，直接push进去
      if(this.baskets.length>0){
        let result=this.baskets.filter((basket)=>{
          return basket.name===list.name&&basket.price===option.price
        })
        if(result!=null&&result.length>0){
          result[0].num++;
        }else{
          this.baskets.push(basket)
        }
        }else{
        this.baskets.push(basket)

      }
    }, 
    reduce(item){
      item.num--;
      if(item.num<=0){
        this.removeBasket(item)
      }
    },
    increase(item){
      item.num++;
    },
    removeBasket(item){
      this.baskets.splice(this.baskets.indexOf(item),1)
    }
  },
      computed: {
        sum(){
        let total=0;
        for(let index in this.baskets){
          let totalItem=this.baskets[index]
          console.log(totalItem)
          total+=totalItem.num*totalItem.price
  
        }
        return total
      }
        },
     
};
</script>
<style>
.imgBox{
  width:100px;
}
.imgBox2{
  width:50px;
}
.table td{
  vertical-align: middle;
}
.empty{
  line-height: 300px;
  font-size: 24px;
  width:400px;
  height:300px;
 text-align: center;
  /* border:1px solid red; */
}
</style>
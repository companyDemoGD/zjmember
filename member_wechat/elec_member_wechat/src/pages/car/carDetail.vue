<template>
<div>
  <div style="background-color:white;padding:0.5rem;padding-left:0.2rem;display;flex">
    <div style="font-size:0.5rem;">
      你的车牌<label style="color:#A8A8A8;padding-left:0.2rem"><span v-for='item of licenceNum'>{{item}}</span></label>
    </div>
  </div>
  <div style="width:100%">
    <table style="margin-top:0.5rem;margin-left:0.2rem;width:90%">
      <tr>
        <td style="align:center;font-size:0.8rem;color:#00C4AB;padding-right:0.1rem">
          ¥20.00
        </td>
        <td style="color:#B7B7B7">
          入场时间:09月09日 12:30<br/>
          <br /> 已停时长：2小时
          <br />
          <br /> 停车位置：B区108
        </td>
      </tr>
    </table>
  </div>
  <div style="background-color:white;height:0.5rem;margin-top:0.3rem;font-size:0.2rem">
    <div style="padding-left:0.04rem;padding-right:0.1rem">
      <group>
        <popup-radio title="优惠券" :options="options1" v-model="option1" placeholder="请选择优惠券"></popup-radio>
      </group>
    </div>
  </div>
  <div style="background-color:white;height:0.5rem;font-size:0.2rem;">
    <div style="padding-left:0.2rem;padding-right:0.1rem">积分
      <input type="text" style="float:right;height:100%;border: medium;text-align:right;height:0.42rem;width:73%;padding-right:0.15rem" placeholder="请输入可用积分">
    </div>


  </div>
  <div style="margin-bottom:0.8rem;padding-right:10%">
    <div style="color:#F79E07;padding-top:0.3rem;padding-left:0.2rem;">温馨提醒</div>
    <div style="color:#979797;padding-top:0.3rem;padding-left:0.2rem;">1 积分抵现金比例： 100积分=1元人民币；</div>
    <div style="color:#979797;padding-top:0.3rem;padding-left:0.2rem;">2 积分抵现条件： 100积分起抵，100积分一档。即：只能100，200,300...依此类推</div>
  </div>
  <router-link to="/carDetail">
    <div style="position:absolute;right:0;left:0;bottom:0;padding-top:0.2rem;display:flex;position:fixed;right:0;left:0;bottom:0;">

      <div style="width:70%;height:100%;font-size:0.5rem;text-align:center;background-color:#E6E6E6">
        <div style="padding:0.3rem;">
          还需支付
          <label style="color:#00C4AB;">¥20.00</label>
        </div>
      </div>
      <div style="color:white;font-size:0.5rem;background-color:#46D0C3;width:30%;height:100%;">
        <div style="padding:0.3rem;" @click="wxpayClik">立即支付</div>
      </div>
    </div>
  </router-link>
</div>
</template>
<script>
import {
  Group,
  PopupRadio
} from 'vux'



import pay from '@/components/common/wxpay.vue'
const {
  WxPay
} = pay
export default {
  components: {
    Group,
    PopupRadio
  },
  data() {
    return {
      options1: ['A', 'B', 'C'],
      licenceNum: []
    }
  },
  mounted() {
    let arr = this.$route.query.car_number
    this.licenceNum = arr
    let bbb = WxPay
  },

  methods: {
     async wxpayClik() {

      let { wx_openid } = this.$route.query
  debugger
      let wx_app_id
      try{
        wx_app_id = 'wx3041b222eaad5c8a'
        // wx_app_id = (await this.$http.get('http://jiayuanmember.dorm9tech.com/wx/appid')).data.wx_app_id
      }catch(e){
      }

      if(!wx_openid){
        const { redirect } = this.$route.query
        if(redirect){
          localStorage.setItem('redirect', redirect)
        }
        const redirectUri = `http://${location.hostname}/wx/code2openid`
        location.href = `http://open.weixin.qq.com/connect/oauth2/authorize?appid=${wx_app_id}&redirect_uri=${redirectUri}&response_type=code&scope=snsapi_userinfo&state=STATE#wechat_redirect`
        return;
      }else{
        WxPay.getAccessToken({
          notify_url: 'http://demo.com/', //微信支付完成后的回调
          out_trade_no: new Date().getTime(), //订单号
          attach: '名称',
          body: '购买信息',
          total_fee: '1', // 此处的额度为分
          spbill_create_ip: '192',
          wx_code: wx_openid,
        }, function(error, responseData) {
          res.render('payment', {
            title: '微信支付',
            wxPayParams: JSON.stringify('123'),
            //userInfo : userInfo
          });
        })
      }

    }
  }
}
</script>

<style scoped>
.demo3-slot {
  text-align: center;
  padding: 8px 0;
  color: #888;
}
</style>

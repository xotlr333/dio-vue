<template>
  <section>
  <div class="title-container">
    <h3 class="title">모바일</h3><p class="phone">{{ userData.phoneNumber}}</p>
  </div>

  <div id="MobileUsePriceHtml" class="product-container">
    <div class="mycont-box">
      <div class="mycalling-plan">
        <h4 class="name">이용중인 요금제</h4>
        <p class="plan">{{ userData.mobileProduct}}</p>
      </div>
      <div class="calling-planbox">
        <div class="calling-head">
          <div class="flat-rate-pricing">
            <div>
              <strong class="name2">월 실납부금액 <span class="intext">(부가세포함)</span></strong>
            </div>
            <div>
              월정액
              <strong class="price">{{ userData.payment}}원</strong>
            </div>
          </div>
        </div>
        <div class="calling-plan-box">
          <table class="calling-plan-data">
            <colgroup>
              <col style="width:9%;">
              <col style="width:61%;">
              <col style="width:8%;">
              <col style="width:22%;">
            </colgroup>
            <tbody>
            <tr>
              <th scope="row">데이터</th>
              <td colspan="3">
                {{ userData.mobileData}}</td>
            </tr>
            <tr>
              <th scope="row">음성</th>
              <td>{{ userData.voice}}</td>
              <th scope="row">문자</th>
              <td>{{ userData.sms}}</td>
            </tr>
            </tbody>
          </table>
        </div>
      </div>
      <ul class="list-dot">
        <li>실납부금액은 <strong class="pointer">결합/요금/복지할인 등이 반영된 금액</strong>이며, 실제 청구되는 금액과는 차이가 있을 수 있습니다.</li>
        <li>요금제 변경/예약 혹은 옵션 설정은 ‘마이 &gt; 요금/서비스 &gt; <a href="/product/MobilePriceView.do" onclick="try{KT_trackClicks('KT-개인_마이', '^KT-개인_마이_요금제/부가서비스^내모바일요금제^요금제변경')}catch(e){}" class="decoration pointer">요금제 변경</a>’ 에서 가능합니다. </li>
      </ul>
    </div>
  </div>

  <div id="MyMobileSaleInfoHtml" class="statement-container" style="">

    <div class="bround-content">
      <h4 class="ctitle">할인 유형 및 약정기간</h4>
      <table class="data-list-row">
        <colgroup>
          <col style="width:20%;">
          <col style="width:20%;">
          <col style="width:25%;">
          <col style="width:30%;">
        </colgroup>
        <tbody>
        <tr >
          <th scope="row">할인 유형</th>
          <td>{{ userData.discountType}}</td>
          <th scope="row">
            약정기간<br>
            (가입일~종료예정일)
          </th>
          <td>{{ userData.contractDate}}</td>
        </tr>
        </tbody>
      </table>
      <h4 class="ctitle">요금할인 및 반환금</h4>
      <table class="pg-data-list pg-charge-list">
        <colgroup>
          <col style="width:70%;">
          <col style="width:30%;">
        </colgroup>
        <tbody>
        <tr>
          <th scope="row">요금할인(선택약정할인)</th>
          <td><strong class="money">{{ userData.discountAmount}}</strong>원</td>
        </tr>
        <tr>
          <th scope="row">요금할인(선택약정) 반환금</th>
          <td><strong class="money">{{ userData.discountRefund}}</strong>원</td>
        </tr>
        <tr>
          <th scope="row">현재까지 실제 할인받은 금액(금일 기준)</th>
          <td><strong class="money">{{ userData.totalDiscountAmount}}</strong>원</td>
        </tr>
        </tbody>
      </table>
    </div>
  </div>
</section>

</template>
<script setup>
import router from "@/router";
import {onBeforeMount, ref} from "vue";
import axiosInstance from "@/utils/axiosInstance";
import {AxiosError} from "axios";
import {useUserStore} from "@/store/useUserStore";

const userStore = useUserStore();
const userData = ref({});

onBeforeMount(async () => {
  console.log("내 모바일 요금제 데이터 호출");
  console.log("user id : " + userStore.user.id);
  try {
    const { data } = await axiosInstance({
      url: `/info/myMobile`,
      method: "post",
      data: JSON.stringify({
        id: userStore.user.id
      }),
    });
    console.log(data);
    if(data.code === 0){
      await router.push("login");
    }
    userData.value = { ...data.information };

  } catch (error) {
    if (error instanceof AxiosError) {
      alert(error.response.data.message);
    }
  }
});

</script>

<style scoped>
section {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: flex-start;
  flex-direction: column;
  margin-top: 50px;

}

.title-container {
  display: flex;
  align-items: center;
  border: 1px solid #666;
  padding-left: 30px;
  width: 770px;
  height: 90px;
  border-radius: 10px;
}

.title {
  font-size: 25pt;
}

.phone {
  vertical-align: middle;
  margin-left: 10px;
  font-size: 25pt;
  margin-left: 40px;
}

.product-container {
  display: flex;
  border: 1px solid #666;
  width: 770px;
  //height: 90px;
  border-radius: 10px;
  padding: 20px 30px;
}

.name {
  font-weight: 700;
  font-size: 15px;
  line-height: 18px;
  color: #000;
  letter-spacing: -0.6px;
}

.plan{
  margin: 12px 0 0 0;
  font-weight: 700;
  font-size: 30px;
  line-height: 34px;
  color: #000;
  letter-spacing: -0.75px;
}

.calling-planbox{
  margin: 13px 0 0 0;
  padding: 25px 20px 20px 20px;
  border-radius: 5px;
  background: #f3f3f3;
  color: #000;
}

.flat-rate-pricing{
  display: flex;
  min-height: 26px;
  justify-content: space-between;
  padding-right: 10px;
  padding-left: 10px;
}

.calling-head{
  position: relative;
  min-height: 26px;
}

.price{
  margin-left: 10px;
  color: red;
}

.name2{
  color: #000;
  font-size: 18px;
  line-height: 21px;
  letter-spacing: -0.06px;
}

.calling-plan-box{
  margin: 7px 0 0 0;
  padding: 10px 20px;
  border-radius: 5px;
  background: #fff;
}
.list-dot{
  margin-top: 10px;
}

div.bround-content {
  padding: 29px 29px 41px 29px;
  border: 1px solid #ccc;
  border-bottom-right-radius: 5px;
  border-bottom-left-radius: 5px;
  background: #f0f0f0;
  border-radius: 10px;
}

.statement-container{
  display: flex;
  //border: 1px solid #666;
  //padding: 10px;
  width: 770px;
//height: 90px;
  border-radius: 10px;
}

.ctitle {
  margin: 14px 0 0 0;
  font-size: 21px;
  font-weight: normal;
  line-height: 26px;
}

table.data-list-row {
  margin: 20px 0 0 0;
  width: 100%;
  border-collapse: separate;
  border-top: 1px solid #666;
  background: #fff;
  table-layout: fixed;
}

table.data-list-row tbody th {
  height: 26px;
  padding: 17px 20px;
  font-size: 15px;
  line-height: 18px;
  text-align: left;
  border-left: 1px solid #eaeaea;
  border-bottom: 1px solid #ddd;
  border-right: 1px solid #eaeaea;
  background: #f7f7f7;
}

table.data-list-row tbody td {
  height: 30px;
  padding: 20px;
  font-size: 15px;
  color: #333;
  border-bottom: 1px solid #ddd;
  line-height: 23px;
}

table.pg-data-list {
  margin: 16px 0 0 0;
  width: 100%;
  border-collapse: separate;
  border-left: 1px solid #ddd;
  border-right: 1px solid #ddd;
  background: #fff;
  table-layout: fixed;
}

table.pg-data-list tbody th {
  padding: 17px 0 17px 19px;
  text-align: left;
  border-left: 1px solid #eaeaea;
  border-bottom: 1px solid #ddd;
  font-size: 15px;
}

table.pg-data-list tbody td {
  padding: 13px 19px 12px 0;
  border-bottom: 1px solid #ddd;
  text-align: right;
  font-size: 15px;
}
</style>

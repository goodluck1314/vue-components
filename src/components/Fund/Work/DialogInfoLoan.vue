<template>
<section id="print">
  <el-dialog custom-class="dia-class" :visible.sync="visibleP" :before-close="handleClose" center="">
    <header slot="title">
      <span class="title">
        {{getTitle}}
      </span>
    </header>
    <section>
      <ul>
        <li>
          <span>付款单位:{{this.detailsP.companyName}}</span>
        </li>
        <li>
          <span>贴现客户:{{this.detailsP.custFromName}}</span>
        </li>
      </ul>
      <ul>
        <li>
          <span>付款银行名称:{{this.detailsP.payerBankName}}</span>
        </li>
        <li>
          <span>付款银款账号:{{this.detailsP.payerBankAccount}}</span>
        </li>
      </ul>
      <ul>
        <li>
          <span>贴现客户收款银行:{{this.detailsP.bankName}}</span>
        </li>
        <li>
          <span>贴现客户收款银行:{{this.detailsP.bankAccount}}</span>
        </li>
      </ul>
      <ul>
        <li>
          <span>贴现金额:{{this.detailsP.billBookAmt}}</span>
        </li>
        <li>
          <span>币别:{{this.detailsP.currencyDesc}}</span>
        </li>
      </ul>
      <ul>
        <li>
         <span>贴现利率:{{this.detailsP.interestRate}}</span>
        </li>
        <li>
         <span>逾期利率:{{this.detailsP.overdueRate}}</span>
        </li>
      </ul>
      <ul>
        <li>
          <span>实际放款金额:{{this.detailsP.actualDiscountAmt}}</span>
        </li>
      </ul>
      <ul>
        <li>
          <span>对应发票号:<label v-for="(item,index) in detailsP.invoiceCustomList" :key="index">{{item.invoiceNo}}</label></span>
        </li>
      </ul>
      <ul>
          <span>合同:
          <div class="a-link-group inline-block">
            <a v-for="item in detailsP.contractList" :key="item.contractId" href="http://" @click.prevent="constractHandle(item.contractNo)">{{item.contractName}}</a>
          </div>
        </span>
      </ul>
      <!-- <p>
            <span>最终付款单位:{{this.detailsP.company}}</span>
            <span>最终付款账户：{{this.detailsP.bankCompaney}}</span>
            <el-tooltip :content="'最终付款账号:'+this.detailsP.bankAccount" placement="bottom" effect="light">
                <span>最终付款账号:{{this.detailsP.bankAccount}}</span>
            </el-tooltip>
        </p>
        <p>
            <span>AR来源:{{this.detailsP.come}}</span>
            <span>状态:{{this.detailsP.status}}</span>
            <span>币别:{{this.detailsP.moneyType}}</span>
        </p>
        <p>
            <span>票面金额:{{this.detailsP.company}}</span>
            <span>可用余额:{{this.detailsP.money_can}}</span>
            <span>预计回款日期:{{this.detailsP.arriveDate}}</span>
        </p> -->
    </section>
    <footer class="no-print" slot="footer" :style="'clear:both'">
      <el-button type="primary" @click="handleClose">确认</el-button>
      <el-button type="primary" @click="print('print')">打印</el-button>
    </footer>
  </el-dialog>
</section>
</template>
<style scoped lang="scss">
@import "@/assets/css/_dialog.scss";
</style>

<script>
import DialogClose from '@/mixins/suplier/Ar/DialogClose'

export default {
  props: ['visibleP', 'detailsP'],
  mixins: [DialogClose],
  data () {
    return {
      radio2: 3
    }
  },
  computed: {
    getTitle () {
      console.log(this.detailsP.masterChainId)
      return this.detailsP.masterChainId + '详情'
    }
  },
  methods: {

  }
}

</script>

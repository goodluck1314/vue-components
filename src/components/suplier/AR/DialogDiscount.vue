<template>

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
          <span>对手单位:{{this.detailsP.custToName}}</span>
        </li>
      </ul>
      <ul>
        <li>
          <span>AR来源:{{this.detailsP.arSourceDesc}}</span>
        </li>
        <li>
          <span>状态:{{this.detailsP.arStatusTypeName}}</span>
        </li>
      </ul>
      <ul>
        <li>
          <span>币别:{{this.detailsP.currencyDesc}}</span>
        </li>
        <li>
          <span>预计回款日期:{{this.detailsP.billPayDate | dateFormat}}</span>
        </li>
      </ul>
      <ul>
        <li>
          <span>票面金额:{{this.detailsP.company}}</span>
        </li>
        <li>
          <span>可用金额:{{this.detailsP.arAvailableAmt}}</span>
        </li>
      </ul>
      <ul>
        <li>
            <span>贴现保理公司：山西英和</span>
        </li>
      </ul>
      <ul>
          <span>已勾选发票:
            <div class="el-check-group inline-block">
              <el-checkbox v-for="item in detailsP.invoiceListSelected" :key="item.invoiceNo" v-model="item.invoiceIsSelected" disabled>{{item.invoiceNo}}</el-checkbox>
            </div>
          </span>
      </ul>
      <ul>
          <span>未勾选发票:
            <el-checkbox-group v-model="checkList" class="inline-blox">
              <el-checkbox v-for="item in detailsP.invoiceList" :key="item.invoiceNo" :label="item.invoiceNo">{{item.invoiceNo}}(￥{{item.invoiceAfterTaxAmt}})</el-checkbox>
            </el-checkbox-group>
            <!-- <el-checkbox v-for="item in detailsP.invoiceList" :key="item.invoiceNo" v-model="item.invoiceIsSelected">{{item.invoiceNo}}</el-checkbox> -->
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
    <section class="layout form">
      <el-row>
        <el-col :span="8" class="flex"><label>贴现金额：</label><el-input v-model.number="transAmt" placeholder="请输入转让金额："></el-input></el-col>
      </el-row>
    </section>
    <footer slot="footer" :style="'clear:both'">
      <el-button type="primary" @click="handleSubmit" :loading="isLoading">确认</el-button>
    </footer>
  </el-dialog>
</template>
<style scoped lang="scss">
@import "@/assets/css/_dialog.scss";
.layout.form{
  margin-top: 10px;
}
.layout.form .flex{
  display: flex;
  >label{
    width:120px;
    height: 40px;
    line-height: 40px;
  }
}
</style>

<script>
import DialogClose from '@/mixins/suplier/Ar/DialogClose'
import Common from '@/mixins/common'

export default {
  name: 'ardiscount', // 贴现弹窗
  props: ['visibleP', 'detailsP'],
  mixins: [DialogClose, Common],
  data () {
    return {
      transAmt: 0,
      checkList: [],
      isLoading: false
    }
  },
  computed: {
    getTitle () {
      return this.detailsP.masterChainId + '贴现'
    }
  },
  methods: {
    handleSubmit () {
      this.isLoading = true
      const data = {
        masterChainId: this.detailsP.masterChainId,
        discountAmt: this.transAmt,
        discountSelectedInvoice: this.checkList.join(',')
      }
      // 已勾选发票
      const arr = []
      this.detailsP.invoiceList.forEach(item => {
        for (const iterator of this.checkList) {
          if (iterator === item.invoiceNo) {
            arr.push(item)
          }
        }
      })
      arr.concat(...this.detailsP.invoiceListSelected)
      console.log(arr)
      console.log(this.detailsP.invoiceListSelected)
      if (arr.length <= 0) {
        this.$message({
          type: 'error',
          message: '未勾选发票'
        })
        this.isLoading = false
        return
      }
      let sum = arr.reduce((sum, currVal) => {
        let num = Number(currVal.invoiceAfterTaxAmt)
        if (isNaN(num)) {
          return
        }
        return sum + num
      }, 0)
      if (Number(this.transAmt) > sum) {
        this.$message({
          type: 'error',
          message: '贴现金额不得大于勾选发票总额'
        })
        this.isLoading = false
        return
      }
      this.checkList = [] // 重置
      console.log(data)
      this.axios.post('/myAr/initiateDiscount.do', data).then(res => {
        let type = res.data.status ? 'success' : 'error'
        this.$message({
          message: res.data.data.message ? res.data.data.message : '返回结果错误，请联系管理员',
          type: type
        })
        this.isLoading = false
        this.handleClose() // 关闭弹窗
        this.$parent.fresh() // 刷新数据
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '操作失败'
        })
        this.isLoading = false
      })
    }
  }
}

</script>

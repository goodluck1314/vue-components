<template>
  <div>
    <el-card class="box-card">
      <div slot="header" class="clearfix">
        <span>企业基础信息</span>
      </div>
      <ul>
        <li :span="8"><span>企业状态：</span>{{infos.isUsableDesc}}</li>
        <li :span="8"><span>所属客户经理：</span>{{infos.sourceName}}</li>
        <li :span="8">
          <el-tooltip :content="'公司ID:'+infos.custId" placement="bottom" effect="light">
            <span>公司ID: {{infos.custId}}</span>
          </el-tooltip>
        </li>
      </ul>
      <ul>
        <li :span="8"><span>企业电话：</span>{{infos.companyPhone}}</li>
        <li :span="8">
          <el-tooltip :content="'企业地址:'+infos.companyAddress" placement="bottom" effect="light">
            <span>企业地址: {{infos.companyAddress}}</span>
          </el-tooltip>
        </li>
        <li :span="8"><span>统一社会信用代码: </span>{{infos.creditCode}}</li>
      </ul>
      <ul>
        <li :span="8"><span>纳税人识別号：</span>{{infos.payTaxesNumber}}</li>
        <li :span="8"><span>供应商代码：</span>{{infos.vendorCodes}}</li>
        <li :span="8"><span>注册资本: </span>{{infos.registeredCapital}}</li>
      </ul>
      <ul>
        <li :span="8"><span>实收资本：</span>{{infos.paidinCapital}}</li>
        <li :span="8"><span>公司成立日期：</span>{{infos.establishDate | dateFormat}}</li>
        <li :span="8"><span>营业执照开始日期：</span>{{infos.businessStartDate | dateFormat}}</li>
      </ul>
      <ul>
        <li :span="8"><span>营业执照结束日期：</span>{{infos.businessEndDate | dateFormat}}</li>
        <li :span="8"><span>公司登记日期：</span>{{infos.companyRegisterDate | dateFormat}}</li>
        <li :span="8"><span>经营范围：</span>{{infos.mainProducts}}</li>
      </ul>
    </el-card>
    <!-- <table>
      <tr>企业基础信息</tr>
      <tr>
        <td>企业状态：{{infos.isUsableDesc}}
</td>
        <td>所属客户经理：{{infos.sourceName}}</td>
        <td>公司ID：{{infos.custId}}</td>
      </tr>
      <tr>
        <td>企业电话：{{infos.companyPhone}}
        </td>
        <td>企业地址：{{infos.companyAddress}}</td>
      </tr>
      <tr>
        <td>统一社会信用代码:{{infos.creditCode}}</td>
        <td>纳税人识別号：{{infos.payTaxesNumber}}</td>
      </tr>
      <tr>
        <td>供应商代码：{{infos.vendorCodes}}</td>
        <td>注册资本:{{infos.registeredCapital}}</td>
        <td>实收资本：{{infos.paidinCapital}}</td>
      </tr>
      <tr>
        <td>公司成立日期：{{infos.establishDate}}</td>
        <td>营业执照开始日期：{{infos.businessStartDate}}</td>
        <td>营业执照结束日期：{{infos.businessEndDate}}</td>
        <td>公司登记日期：{{infos.companyRegisterDate}}</td>
      </tr>
      <tr>
        <td>经营范围 ：{{infos.mainProducts}}</td>
      </tr>
    </table> -->
  </div>
</template>
<script>
/* 用户信息列表 */
import Common from '@/mixins/common'
export default {
  props: ['infos'],
  mixins: [Common]
}
</script>
<style lang="scss" scoped>
 li{
   list-style: none;
    width: 31%;
    display: inline-block;
    text-overflow: ellipsis;
    overflow: hidden;
    white-space: nowrap;
    line-height: 32px;
    text-align: left;
    padding-left: 5px;
 }
</style>

<template>
  <a-card :bordered="false">

    <!-- 查询区域 -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline" @keyup.enter.native="searchQuery">
        <a-row :gutter="24">
          <a-col :md="6" :sm="12">
            <a-form-item label="打款时间">
              <a-range-picker v-model="dateRange" @change = "dateRangeChange"/>
            </a-form-item>
          </a-col>
          
          <a-col :md="6" :sm="12">
            <a-form-item label="联系人">
              <a-input placeholder="输入联系人" v-model="queryParam.customerName"></a-input>
            </a-form-item>
          </a-col>
          
          <a-col :md="6" :sm="8">
            <a-form-item label="打款确认">
              <a-select v-model="queryParam.status" placeholder="请选择打款确认">
                <a-select-option value="">请选择</a-select-option>
                <a-select-option value="1">待确认</a-select-option>
                <a-select-option value="2">已确认</a-select-option>
                <a-select-option value="3">已驳回</a-select-option>
              </a-select>
            </a-form-item>
          </a-col>

          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <span style="float: left;overflow: hidden;" class="table-page-search-submitButtons">
              <a-button type="primary" @click="searchQuery" icon="search">查询</a-button>
            </span>
          </a-col>

        </a-row>
      </a-form>
    </div>

    <!-- 操作按钮区域 -->
    <div class="table-operator">
      <a-button type="primary" icon="download" @click="handleExportXls('提现申请表')">导出</a-button>
      <a-upload 
        name="file" 
        :showUploadList="false" 
        :multiple="false" 
        :headers="tokenHeader" 
        :action="importExcelUrl" 
        @change="handleImportExcel" >
        <a-button type="primary" icon="import">导入</a-button>
      </a-upload>
    </div>

    <!-- table区域-begin -->
    <div>
      <a-table ref="table" size="middle" bordered rowKey="id" :columns="columns" :dataSource="dataSource"
        :pagination="ipagination" :loading="loading" class="j-table-force-nowrap" @change="handleTableChange">

        <span slot="action" slot-scope="text, record">
          <a @click="handleEdit(record)" v-has="'groupSpec:edit'">编辑</a>
        </span>

      </a-table>
    </div>
    <!-- table区域-end -->

    <!-- 表单区域 -->
    <withdrawalApply-modal ref="modalForm" @ok="modalFormOk"></withdrawalApply-modal>
  </a-card>
</template>

<script>
import '@/assets/less/TableExpand.less'
import WithdrawalApplyModal from './modules/WithdrawalApplyModal'
import { JeecgListMixin } from '@/mixins/JeecgListMixin'

export default {
  name: "WithdrawalApplyList",
  mixins: [JeecgListMixin],
  components: {
    WithdrawalApplyModal
  },
  data() {
    return {
      description: '提现申请页面',
      // 表头
      columns: [
        {
          title: '#',
          dataIndex: '',
          key: 'rowIndex',
          width: 60,
          align: "center",
          customRender: function (t, r, index) {
            return parseInt(index) + 1;
          }
        },
        {
          title: '银行名称',
          align: "center",
          dataIndex: 'bankName',
        },
        {
          title: '开户行名称',
          align: "center",
          dataIndex: 'bankOpenName',
        },
        {
          title: '银行卡号',
          align: "center",
          dataIndex: 'bankCardNumber',
        },
        {
          title: '真实姓名',
          align: "center",
          dataIndex: 'customerName',
        },
        {
          title: '联系手机号',
          align: "center",
          dataIndex: 'customerPhone',
        },
        {
          title: '提现时间',
          align: "center",
          dataIndex: 'createTime',
        },
        {
          title: '打款时间',
          align: "center",
          dataIndex: 'payTime',
        },
        {
          title: '提现金额',
          align: "center",
          dataIndex: 'applyAmountStr',
        },
        {
          title: '打款金额',
          align: "center",
          dataIndex: 'payAmountStr',
        },
        {
          title: '打款确认',
          align: "center",
          dataIndex: 'status_dictText',
        },
        {
          title: '操作',
          dataIndex: 'action',
          align: "center",
          scopedSlots: { customRender: 'action' },
        }
      ],
      url: {
        list: "/withdrawalApply/list",
        exportXlsUrl: "/withdrawalApply/exportXls",
        importExcelUrl: "withdrawalApply/importExcel",
      },
      dateRange:null,
    }
  },
  computed: {
    importExcelUrl: function () {
      return `${window._CONFIG['domianURL']}/${this.url.importExcelUrl}`;
    }
  },
  methods: {
    dateRangeChange(d1, d2) {
      if (d2[0]) {
        this.queryParam.payTimeBegin = d2[0] + " 00:00:00"
      } else {
        this.queryParam.payTimeBegin = d2[0]
      }
      
      if (d2[1]) {
        this.queryParam.payTimeEnd = d2[1] + " 23:59:59"
      } else {
        this.queryParam.payTimeEnd = d2[1]
      }
    }
  }
}
</script>
<style scoped>
@import '~@assets/less/common.less';
</style>
<template>
  <a-card :bordered="false">

    <!-- 查询区域 -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline" @keyup.enter.native="searchQuery">
        <a-row :gutter="24">

          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <span style="float: left;overflow: hidden;" class="table-page-search-submitButtons">
              <a-button type="primary" @click="searchQuery" icon="search">查询</a-button>
            </span>
          </a-col>

        </a-row>
      </a-form>
    </div>

    <!-- table区域-begin -->
    <div>
      <a-table ref="table" size="middle" bordered rowKey="id" :columns="columns" :dataSource="dataSource"
        :pagination="ipagination" :loading="loading" class="j-table-force-nowrap" @change="handleTableChange">

        <span slot="action" slot-scope="text, record">
          <a @click="handleEdit(record)" v-has="'sysConfig:edit'">编辑</a>
        </span>

      </a-table>
    </div>
    <!-- table区域-end -->

    <!-- 表单区域 -->
    <sysConfig-modal ref="modalForm" @ok="modalFormOk"></sysConfig-modal>
  </a-card>
</template>

<script>
import '@/assets/less/TableExpand.less'
import SysConfigModal from './modules/SysConfigModal'
import { JeecgListMixin } from '@/mixins/JeecgListMixin'

export default {
  name: "SysConfigList",
  mixins: [JeecgListMixin],
  components: {
    SysConfigModal
  },
  data() {
    return {
      description: '系统配置表管理页面',
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
          title: '餐馆配送范围',
          align: "center",
          dataIndex: 'restaurantCoverage',
          customRender: (text) => {
            return text + "米";
          }
        },
        {
          title: '开团点允许的最近距离',
          align: "center",
          dataIndex: 'groupOrderMinDistance',
          customRender: (text) => {
            return text + "米";
          }
        },
        {
          title: '操作',
          dataIndex: 'action',
          align: "center",
          scopedSlots: { customRender: 'action' },
        }
      ],
      url: {
        list: "/sysConfig/list",
      },
    }
  },
  computed: {
    importExcelUrl: function () {
      return `${window._CONFIG['domianURL']}/${this.url.importExcelUrl}`;
    }
  },
  methods: {

  }
}
</script>
<style scoped>
@import '~@assets/less/common.less';
</style>
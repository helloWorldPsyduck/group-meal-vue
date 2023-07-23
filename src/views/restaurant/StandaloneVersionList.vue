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

    <!-- 操作按钮区域 -->
    <div class="table-operator">
      <a-button @click="handleAdd" type="primary" icon="plus" v-has="'groupSpec:add'">新增</a-button>
    </div>

    <!-- table区域-begin -->
    <div>
      <a-table ref="table" size="middle" bordered rowKey="id" :columns="columns" :dataSource="dataSource"
        :pagination="ipagination" :loading="loading" class="j-table-force-nowrap" @change="handleTableChange">

        <span slot="action" slot-scope="text, record">
          <a @click="handleEdit(record)" v-has="'groupSpec:edit'">编辑</a>
          <a-divider type="vertical" v-has="'groupSpec:delete'"/>
          <a-popconfirm title="确定删除吗?" @confirm="() => handleDelete(record.id)" v-has="'groupSpec:delete'">
            <a>删除</a>
          </a-popconfirm>
        </span>

      </a-table>
    </div>
    <!-- table区域-end -->

    <!-- 表单区域 -->
    <standaloneVersion-modal ref="modalForm" @ok="modalFormOk"></standaloneVersion-modal>
  </a-card>
</template>

<script>
import '@/assets/less/TableExpand.less'
import StandaloneVersionModal from './modules/StandaloneVersionModal'
import { JeecgListMixin } from '@/mixins/JeecgListMixin'

export default {
  name: "GroupSpecList",
  mixins: [JeecgListMixin],
  components: {
    StandaloneVersionModal
  },
  data() {
    return {
      description: '独立版客户端',
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
          title: '饭店',
          align: "center",
          dataIndex: 'restaurantName',
        },
        {
          title: 'APPID',
          align: "center",
          dataIndex: 'appId',
        },
        {
          title: '操作',
          dataIndex: 'action',
          align: "center",
          scopedSlots: { customRender: 'action' },
        }
      ],
      url: {
        list: "/standaloneVersion/list",
        delete: "/standaloneVersion/delete",
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
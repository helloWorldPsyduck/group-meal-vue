<template>
  <a-card :bordered="false">

    <!-- 查询区域 -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline" @keyup.enter.native="searchQuery">
        <a-row :gutter="24">

          <a-col :md="6" :sm="12">
            <a-form-item label="菜名">
              <j-input placeholder="输入菜名" v-model="queryParam.name"></j-input>
            </a-form-item>
          </a-col>

          <a-col :md="6" :sm="8">
            <a-form-item label="状态">
              <a-select v-model="queryParam.status" placeholder="请选择状态">
                <a-select-option value="">请选择</a-select-option>
                <a-select-option value="1">正常</a-select-option>
                <a-select-option value="2">回收站</a-select-option>
              </a-select>
            </a-form-item>
          </a-col>


          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <span style="float: left;overflow: hidden;" class="table-page-search-submitButtons">
              <a-button type="primary" @click="searchQuery" icon="search">查询</a-button>
              <a-button type="primary" @click="searchReset" icon="reload" style="margin-left: 8px">重置</a-button>
            </span>
          </a-col>

        </a-row>
      </a-form>
    </div>

    <!-- 操作按钮区域 -->
    <div class="table-operator">
      <a-button @click="handleAdd" type="primary" icon="plus">新增</a-button>
      <a-button type="primary" icon="download" @click="handleExportXls('菜品库')">导出</a-button>
      <a-dropdown v-if="selectedRowKeys.length > 0">
        <a-menu slot="overlay">
          <a-menu-item key="1" @click="batchDel"><a-icon type="delete" />删除</a-menu-item>
        </a-menu>
        <a-button style="margin-left: 8px"> 批量操作 <a-icon type="down" /></a-button>
      </a-dropdown>
    </div>

    <!-- table区域-begin -->
    <div>
      <a-table ref="table" size="middle" bordered rowKey="id" :columns="columns" :dataSource="dataSource"
        :pagination="ipagination" :loading="loading" class="j-table-force-nowrap" @change="handleTableChange">
        <span slot="action" slot-scope="text, record">
          <a @click="handleEdit(record)">编辑</a>
        </span>

      </a-table>
    </div>
    <!-- table区域-end -->

    <!-- 表单区域 -->
    <dishlibrary-modal ref="modalForm" @ok="modalFormOk"></dishlibrary-modal>
  </a-card>
</template>

<script>
import '@/assets/less/TableExpand.less'
import DishlibraryModal from './modules/DishlibraryModal'
import { JeecgListMixin } from '@/mixins/JeecgListMixin'

export default {
  name: "DishlibraryList",
  mixins: [JeecgListMixin],
  components: {
    DishlibraryModal
  },
  data() {
    return {
      description: '菜品库管理页面',
      ossHost: "https://oss.vbuyy.com/",
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
          title: '菜名',
          align: "center",
          dataIndex: 'name'
        },
        {
          title: '推荐价格',
          align: "center",
          dataIndex: 'price',
          customRender: (text) => {
            return text + "元";
          }
        },
        {
          title: '预估分量',
          align: "center",
          dataIndex: 'weight',
          customRender: (text) => {
            return text + "g";
          }
        },
        {
          title: '状态',
          align: "center",
          dataIndex: 'status',
          customRender: (text) => {
            if (text == 1) {
              return '正常';
            } else if (text == 2) {
              return "回收站";
            }
            return text;
          }
        },
        {
          title: '图片',
          align: "center",
          width: 120,
          dataIndex: 'picture',
          customRender: (text) => {
            if (!text) {
              return '无图片'
            } else {
              return this.ossHost + text;
            }
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
        list: "/dishLibrary/page",
        exportXlsUrl: "/dishLibrary/exportXls",
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
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

        <span slot="manor" slot-scope="text, record">
          <div v-for="(value) in text">{{ value }}</div>
        </span>

      </a-table>
    </div>
    <!-- table区域-end -->
  </a-card>
</template>

<script>
import '@/assets/less/TableExpand.less'
import { JeecgListMixin } from '@/mixins/JeecgListMixin'

export default {
  name: "SysConfigList",
  mixins: [JeecgListMixin],
  components: {
  },
  data() {
    return {
      description: '人员管理页面',
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
          title: '姓名',
          align: "center",
          dataIndex: 'name',
        },
        {
          title: '手机号',
          align: "center",
          dataIndex: 'phone',
        },
        {
          title: '角色',
          align: "center",
          dataIndex: 'type',
          customRender: (text) => {
            var roles = text.split(",");
            let roleStr = "";
            if (roles.indexOf("1") >= 0){
              roleStr += "超级管理员，"
            }
            if (roles.indexOf("2") >= 0){
              roleStr += "区域经理，"
            }
            if (roles.indexOf("3") >= 0){
              roleStr += "客户端销售，"
            }
            if (roles.indexOf("4") >= 0){
              roleStr += "餐馆销售，"
            }
            if (roleStr) {
              roleStr = roleStr.substring(0, roleStr.length-1);
            }
            return roleStr;
          }
        },
        {
          title: '辖区',
          align: "center",
          dataIndex: 'manor',
          scopedSlots: { customRender: 'manor' },
        },
        {
          title: '创建时间',
          align: "center",
          dataIndex: 'createTime',
        },
      ],
      url: {
        list: "/manager/list",
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
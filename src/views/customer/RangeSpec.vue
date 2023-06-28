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
      <a-button @click="add" type="primary" icon="plus">新增</a-button>
    </div>

    <!-- table区域-begin -->
    <div>
      <a-table ref="table" size="middle" bordered rowKey="id" :columns="columns" :dataSource="dataSource"
        :pagination="ipagination" :loading="loading" class="j-table-force-nowrap" @change="handleTableChange">

        <span slot="action" slot-scope="text, record">
          <a @click="edit(record)" v-has="'dishLibrary:edit'">编辑</a>
          <a-divider type="vertical" v-has="'dishLibrary:edit'"/>
          <a-popconfirm title="确定删除吗?" @confirm="() => handleDelete(record.id)">
            <a>删除</a>
          </a-popconfirm>
        </span>

      </a-table>
    </div>
    <!-- table区域-end -->

    <j-modal :title="title" :width="800" :visible="visible" @ok="ok" @cancel="cancel" :confirmLoading="confirmLoading" cancelText="关闭">
      <a-form-model ref="form" :model="model">
        <a-row>
          <a-col :span="24">
            <a-form-model-item label="规格" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="spec">
              <a-input v-model="model.spec" placeholder="请输入规格"></a-input>
            </a-form-model-item>
          </a-col>
        </a-row>
      </a-form-model>
    </j-modal>

  </a-card>
</template>

<script>
import '@/assets/less/TableExpand.less'
import { JeecgListMixin } from '@/mixins/JeecgListMixin'
import { httpAction } from '@/api/manage'

export default {
  name: "RangeSpec",
  mixins: [JeecgListMixin],
  components: {
  },
  data() {
    return {
      description: '写字楼范围配置页面',
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
          title: '规格',
          align: "center",
          dataIndex: 'spec',
        },
        {
          title: '操作',
          dataIndex: 'action',
          align: "center",
          scopedSlots: { customRender: 'action' },
        },
      ],
      url: {
        list: "/officeBuildingRangeSpec/list",
        add: "/officeBuildingRangeSpec/add",
        edit: "/officeBuildingRangeSpec/edit",
        delete: "/officeBuildingRangeSpec/delete",
      },
      labelCol: {
        xs: { span: 24 },
        sm: { span: 5 },
      },
      wrapperCol: {
        xs: { span: 24 },
        sm: { span: 16 },
      },
      visible: false,
      confirmLoading: false,
      title: "",
      model: {
        id: "",
        spec: "",
      }
    }
  },
  methods: {
    add() {
      this.title = "添加人数配置";
      this.model = {
        id: "",
        spec: "",
      };
      this.visible = true;
    },
    edit(model) {
      this.title = "编辑人数配置";
      this.model = model;
      this.visible = true;
    },
    ok() {
      const that = this;
      that.confirmLoading = true;
      let httpurl = '';
      let method = '';
      if (!this.model.id) {
        httpurl += this.url.add;
        method = 'post';
      } else {
        httpurl += this.url.edit;
        method = 'put';
      }
      httpAction(httpurl, this.model, method).then((res) => {
        if (res.success) {
          that.$message.success(res.message);
        } else {
          that.$message.warning(res.message);
        }
      }).finally(() => {
        that.confirmLoading = false;
        that.visible = false;
        that.searchQuery();
      })
    },
    cancel() {
      this.visible = false;
    },
  }
}
</script>
<style scoped>
@import '~@assets/less/common.less';
</style>
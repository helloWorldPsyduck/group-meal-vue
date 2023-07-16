<template>
  <j-modal :title="title" :width="800" :visible="visible" :confirmLoading="confirmLoading" switchFullscreen @ok="handleOk"
    @cancel="handleCancel" cancelText="关闭">

    <a-spin :spinning="confirmLoading">
      <a-form-model ref="form" :model="model" :rules="validatorRules">
        <a-row>
          <a-col :span="24">
            <a-form-model-item label="打款金额" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="begin">
              <a-input :min="0" v-model="model.payAmountStr" placeholder="请输入打款金额"></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="打款时间" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="end">
              <a-date-picker v-model="model.payTime" show-time placeholder="选择打款时间" @change="dateChange"/>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="打款确认" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="content">
              <a-select v-model="model.status" placeholder="请选择打款确认">
                <a-select-option :value="1">待确认</a-select-option>
                <a-select-option :value="2">已确认</a-select-option>
                <a-select-option :value="3">已驳回</a-select-option>
              </a-select>
            </a-form-model-item>
          </a-col>
        </a-row>
      </a-form-model>
    </a-spin>
  </j-modal>
</template>

<script>
import { httpAction } from '@/api/manage'
import moment from "moment"

export default {
  name: "OpeningHoursModal",
  data() {
    return {
      title: "操作",
      visible: false,
      model: {},
      labelCol: {
        xs: { span: 24 },
        sm: { span: 5 },
      },
      wrapperCol: {
        xs: { span: 24 },
        sm: { span: 16 },
      },

      confirmLoading: false,
      validatorRules: {
      },
      url: {
        edit: "/withdrawalApply/edit",
      },
    }
  },
  created() {
  },
  methods: {
    edit(record) {
      this.model = Object.assign({}, record);
      this.visible = true;
    },
    close() {
      this.$emit('close');
      this.visible = false;
      this.$refs.form.clearValidate();
    },
    handleOk() {
      const that = this;
      // 触发表单验证
      this.$refs.form.validate(valid => {
        if (valid) {
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
              that.$emit('ok');
            } else {
              that.$message.warning(res.message);
            }
          }).finally(() => {
            that.confirmLoading = false;
            that.close();
          })
        } else {
          return false;
        }
      })
    },
    dateChange(d1, d2) {
      this.model.payTime = d2;
    },
    handleCancel() {
      this.close()
    },


  }
}
</script>

<style lang="less" scoped></style>
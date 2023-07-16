<template>
  <j-modal :title="title" :width="800" :visible="visible" :confirmLoading="confirmLoading" switchFullscreen @ok="handleOk"
    @cancel="handleCancel" cancelText="关闭">

    <a-spin :spinning="confirmLoading">
      <a-form-model ref="form" :model="model" :rules="validatorRules">
        <a-row>
          <a-col :span="24">
            <a-form-model-item label="起" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="begin">
              <a-input-number :min="0" :max="23" v-model="model.begin" placeholder="请输入营业开始时间"></a-input-number>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="止" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="end">
              <a-input-number :min="model.begin" :max="24" v-model="model.end" placeholder="请输入营业结束时间" width="300px"></a-input-number>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="文本" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="content">
              <a-input v-model="model.content" placeholder="请输入文本"></a-input>
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
        edit: "/restaurantOpeningHours/edit",
        add: "/restaurantOpeningHours/add",
      },
    }
  },
  created() {
  },
  methods: {
    add() {
      //初始化默认值
      this.edit({});
    },
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
    handleCancel() {
      this.close()
    },


  }
}
</script>

<style lang="less" scoped></style>
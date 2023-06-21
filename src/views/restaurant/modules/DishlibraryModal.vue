<template>
  <j-modal :title="title" :width="800" :visible="visible" :confirmLoading="confirmLoading" switchFullscreen @ok="handleOk"
    @cancel="handleCancel" cancelText="关闭">

    <a-spin :spinning="confirmLoading">
      <a-form-model ref="form" :model="model">
        <a-row>
          <a-col :span="24">
            <a-form-model-item label="菜名" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="name">
              <a-input v-model="model.name" placeholder="请输入菜名"></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="推荐价格" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="price">
              <a-input v-model="model.price" placeholder="请输入推荐价格"></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="预估分量" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="weight">
              <a-input v-model="model.weight" placeholder="请输入预估分量"></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="图片" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="picture">
              <a-input v-model="model.picture" placeholder="请上传图片"></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="状态" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="status">
              <a-select v-model="model.status" placeholder="请选择状态">
                <a-select-option value="">请选择</a-select-option>
                <a-select-option :value=1>正常</a-select-option>
                <a-select-option :value=2>回收站</a-select-option>
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
  name: "DishlibraryModal",
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
        add: "/org.jeecg.modules/dishlibrary/add",
        edit: "/org.jeecg.modules/dishlibrary/edit",
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
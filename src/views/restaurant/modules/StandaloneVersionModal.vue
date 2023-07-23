<template>
  <j-modal :title="title" :width="800" :visible="visible" :confirmLoading="confirmLoading" switchFullscreen @ok="handleOk"
    @cancel="handleCancel" cancelText="关闭">

    <a-spin :spinning="confirmLoading">
      <a-form-model ref="form" :model="model" :rules="validatorRules">
        <a-row>
          <a-col :span="24">
            <a-form-model-item label="APPID" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="name">
              <a-input v-model="model.appId" placeholder="请输入APPID"></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="餐馆" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="name">
              <a-select v-model="model.restaurantId" placeholder="请选择餐馆">
                <a-select-option value="">请选择</a-select-option>
                <a-select-option :value="item.id" v-for="(item, index) in restaurants">{{item.name}}</a-select-option>
              </a-select>
            </a-form-model-item>
          </a-col>
        </a-row>
      </a-form-model>
    </a-spin>
  </j-modal>
</template>

<script>
import { httpAction,getAction } from '@/api/manage'
import moment from "moment"

export default {
  name: "GroupSpecModal",
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
        allRestaurant: "/restaurant/all",
        edit: "/standaloneVersion/edit",
        add: "/standaloneVersion/add",
      },
      restaurants:[],
    }
  },
  created() {
    this.initAllRestaurants();
  },
  methods: {
    initAllRestaurants(){
      getAction(this.url.allRestaurant).then((res) => {
        console.log(res);
        if (res.success) {
          this.restaurants = res.result;
        }else{
          this.$message.warning(res.message)
        }
      })
    },
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
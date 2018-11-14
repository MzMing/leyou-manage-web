<template>
  <v-form v-model="valid" ref="myBrandForm">
    <v-text-field v-model="brand.name" label="请输入品牌名称" required
                  :rules="[v => !!v || '品牌名称不能为空']"
    />
    <v-text-field v-model="brand.letter" label="请输入品牌首字母" required
                  mask="A" :rules="[v => !!v || '品牌首字母不能为空']"
    />

    <v-cascader
      url="/item/category/list"
      multiple
      required
      v-model="brand.categories"
      label="请选择商品分类"/>

    <v-layout row>
      <v-flex xs3>
        <span style="font-size: 16px; color: #444">品牌LOGO：</span>
      </v-flex>
      <v-flex>
        <v-upload
          v-model="brand.image"
          url="/upload"
          :multiple="false"
          :pic-width="250"
          :pic-height="90"
        />
      </v-flex>
    </v-layout>
    <v-layout row class="py-5" >
      <v-btn color="success" @click="submit">提交</v-btn>
      <v-btn color="warning">重置</v-btn>
    </v-layout>
  </v-form>
</template>

<script>
  export default {
    name: "MyBrandForm",
    data() {
      return {
        valid: false, // 表单校验结果标记
        brand: {
          name: '', // 品牌名称
          letter: '', // 品牌首字母
          image: '',// 品牌logo
          categories: [], // 品牌所属的商品分类数组
        }
      }
    },
    methods:{
      submit() {
        // 1、表单校验
        if (this.$refs.myBrandForm.validate()) {
          // 2、定义一个请求参数对象，通过解构表达式来获取brand中的属性
          const {categories ,letter ,...params} = this.brand;
          // 3、数据库中只要保存分类的id即可，因此我们对categories的值进行处理,只保留id，并转为字符串
          params.cids = categories.map(c => c.id).join(",");
          // 4、将字母都处理为大写
          params.letter = letter.toUpperCase();
          // 5、将数据提交到后台
          this.$http.post('/item/brand', this.$qs.stringify(params))
            .then(() => {
              this.$emit("close");
              // 6、弹出提示
              this.$message.success("保存成功！");
            })
            .catch(() => {
              this.$message.error("保存失败！");
            });
        }
      }
    }
  }
</script>

<style scoped>

</style>

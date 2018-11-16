<template>
  <div>
    <v-card>
      <v-card-title class="layout row">
        <v-btn color="primary" @click="show=true">
          新增品牌
        </v-btn>
        <v-spacer/>
        <v-text-field
          label="请输入搜索条件"
          append-icon="search"
          class="flex sm3"
          hide-details
          v-model="search"
        />
      </v-card-title>
      <v-divider/>
      <v-data-table
        :headers="headers"
        :items="brands"
        :pagination.sync="pagination"
        :total-items="totalBrands"
        :loading="loading"
        class="elevation-1"
      >
        <template slot="items" slot-scope="props">
          <td class="text-xs-center">{{ props.item.id }}</td>
          <td class="text-xs-center">{{ props.item.name}}</td>
          <td class="text-xs-center"><img v-if="!!props.item.image" :src="props.item.image"/></td>
          <td class="text-xs-center">{{ props.item.letter }}</td>
          <td class="justify-center layout px-0">
            <v-btn small icon color="info" @click="editBrand(props.item)">
              <i class="el-icon-edit"/>
            </v-btn>
            <v-btn small icon color="error" @click="deleteBrand(props.item)">
              <i class="el-icon-delete"/>
            </v-btn>
          </td>
        </template>
      </v-data-table>
      <!--新增-->
      <v-dialog v-model="show" max-width="500" scrollable persistent>
        <v-card>
          <v-toolbar dense color="primary" dark class="title px-2">
            <span>新增品牌</span>
            <v-spacer/>
            <v-btn icon @click="show = false">
              <v-icon>close</v-icon>
            </v-btn>
          </v-toolbar>
          <v-card-text style="height: 500px" class="px-5">
            <my-brand-form @close="closeWindow"/>
          </v-card-text>
        </v-card>
      </v-dialog>
    </v-card>
  </div>
</template>

<script>
  import MyBrandForm from "./MyBrandForm";

  export default {
    name: "my-brande",
    data() {
      return {
        headers: [
          {text: "id", value: "id", align: 'center', sortable: true},
          {text: "名称", value: "name", align: 'center', sortable: false},
          {text: "LOGO", value: "image", align: 'center', sortable: false},
          {text: "首字母", value: "letter", align: 'center', sortable: false},
          {text: "操作", align: 'center', sortable: false}
        ],
        brands: {},
        pagination: {},
        totalBrands: 0,
        loading: false,
        search: '',
        show: false
      }
    }, // data

    watch: {
      pagination: {
        deep: true,
        handler() {
          this.getDataFromServer();
        }
      },
      search() {
        this.getDataFromServer();
      },
      created() {
        this.getDataFromServer();
      },
    }, // watch

    methods: {
      closeWindow(){
        //关闭窗口
        this.show=false;
        //重新加载数据
        this.getDataFromServer();
      },

      getDataFromServer() {
        //开启进度条
        this.loading = true;
        //发起ajax请求，传参：
        //page 当前页,rows 每页大小,key 搜索条件,sortBy 排序条件,desc 是否是降序
        //返回值：总页数，总条数，当前页的数据
        this.$http.get("/item/brand/page", {
          params: {
            //分页
            page: this.pagination.page,
            rows: this.pagination.rowsPerPage,
            sortBy: this.pagination.sortBy,
            desc: this.pagination.descending,
            key: this.search,
          }
        }).then(resp => {
          //赋值
          this.brands = resp.data.items;
          this.totalBrands = resp.data.total;
          //关闭进度条
          this.loading = false;
        })
      },

      deleteBrand(item) {
        this.$message.confirm('此操作将永久删除该品牌, 是否继续?').then(() => {
          // 发起删除请求
          this.$http.delete("/item/brand" , {  params:{id:item.id}   })
            .then(() => {
              // 删除成功，重新加载数据
              this.$message.success("删除成功！");
              this.getDataFromServer();
            })
        }).catch(() => {
          this.$message.info("删除已取消！");
        });


      },

    }, //methods

    components: {
      MyBrandForm
    }
  }
</script>

<style scoped>

</style>

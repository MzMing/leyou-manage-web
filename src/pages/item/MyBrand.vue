<template>
  <v-card>
    <v-card-title>
      <v-btn small color="primary">
        新增品牌
      </v-btn>
      <v-spacer />
      <v-text-field label="输入关键字搜索" v-model="search" append-icon="search" hide-details/>
    </v-card-title>
    <v-divider/>
    <v-data-table
      :headers="headers"
      :items="brands"
      :pagination.sync="pagination"
      :total-items="totalBrands"
      :loading="loading"
      :search="search"
      class="elevation-1"
    >
      <template slot="items" slot-scope="props">
        <td class="text-xs-center">{{ props.item.id }}</td>
        <td class="text-xs-center">{{ props.item.name }}</td>
        <td class="text-xs-center"><img :src="props.item.image"/></td>
        <td class="text-xs-center">{{ props.item.letter }}</td>
        <td class="justify-center layout">
          <v-btn small icon @click="editBrand(props.item)">
            <v-icon color="success">edit</v-icon>
          </v-btn>
          <v-btn small icon @click="deleteBrand(props.item)">
            <v-icon color="primary">delete</v-icon>
          </v-btn>
        </td>
      </template>
    </v-data-table>
  </v-card>
</template>

<script>
  export default {
    name: "MyBrand",
    data() {
      return {
        headers: [
          {
            text: "id", // 表头的显示文本
            value: "id", // 表头对应的每行数据的key
            align: 'center', // 位置
            sortable: true, // 是否可排序
          },
          {text: "名称", value: "name", align: 'center', sortable: false},
          {text: "LOGO", value: "image", align: 'center', sortable: false},
          {text: "首字母", value: "letter", align: 'center', sortable: false},
          {text: '操作', align: 'center', value: 'id', sortable: false},
        ],
        brands: [],
        pagination: {},
        totalBrands: 0,
        loading: false,
        search:'',

      }
    },
    watch: {
      pagination: {
        deep: true,
        handler() {
          this.getDataFromServer();
        }
      },
      search(){
        this.getDataFromServer();
      }
    },
    created() {
      this.getDataFromServer();
    },
    methods: {
      getDataFromServer() {
        this.loading = true;

        //ajax请求
        this.$http.get("/item/brand/page",{
          params:{
            page:1,
            rows:5,
            sortBy:"id",
            desc:true,
            search:this.search,
          }
        }).then(resp =>{
          this.brands = resp.data.items;
          this.totalBrands = resp.data.total;
          this.loading = false;
        })
      }
    }
  }
</script>

<style scoped>

</style>

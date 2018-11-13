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


        const brands = [
          {
            "id": 2032,
            "name": "OPPO",
            "image": "http://img10.360buyimg.com/popshop/jfs/t2119/133/2264148064/4303/b8ab3755/56b2f385N8e4eb051.jpg",
            "letter": "O"
          },
          {
            "id": 2033,
            "name": "飞利浦（PHILIPS）",
            "image": "http://img12.360buyimg.com/popshop/jfs/t18361/122/1318410299/1870/36fe70c9/5ac43a4dNa44a0ce0.jpg",
            "letter": "F"
          },
          {
            "id": 2034,
            "name": "华为（HUAWEI）",
            "image": "http://img10.360buyimg.com/popshop/jfs/t5662/36/8888655583/7806/1c629c01/598033b4Nd6055897.jpg",
            "letter": "H"
          },
          {
            "id": 2036,
            "name": "酷派（Coolpad）",
            "image": "http://img10.360buyimg.com/popshop/jfs/t2521/347/883897149/3732/91c917ec/5670cf96Ncffa2ae6.jpg",
            "letter": "K"
          },
          {
            "id": 2037,
            "name": "魅族（MEIZU）",
            "image": "http://img13.360buyimg.com/popshop/jfs/t3511/131/31887105/4943/48f83fa9/57fdf4b8N6e95624d.jpg",
            "letter": "M"
          }
        ];
        setTimeout(() => {
          this.brands = brands;
          this.totalBrands = 10;
          this.loading = false;
        }, 300);
      }
    }
  }
</script>

<style scoped>

</style>

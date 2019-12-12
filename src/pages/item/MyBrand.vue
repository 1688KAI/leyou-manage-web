<template>
  <div>
    <template>
      <div>
        <v-layout class="px-2 pb-2">
          <v-flex xs2>
            <v-btn small color="info">新增品牌</v-btn>
          </v-flex>
          <v-spacer/>
          <v-flex xs4>
            <v-text-field label="搜索" hide-details append-icon="search" v-model="key"></v-text-field>
          </v-flex>
        </v-layout>
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
            <td class="text-xs-center">{{ props.item.name }}</td>
            <td class="text-xs-center"><img :src="props.item.image" alt=""></td>
            <td class="text-xs-center">{{ props.item.letter }}</td>
            <td class="text-xs-center">
              <v-btn flat icon color="info">
                <v-icon>edit</v-icon>
              </v-btn>
              <v-btn flat icon color="error">
                <v-icon>delete</v-icon>
              </v-btn>
            </td>
          </template>
        </v-data-table>
      </div>
    </template>
  </div>
</template>
<script>
    export default {
        name: "MyBrand",
        data() {
            return {
                headers: [
                    {text: '品牌id', align: 'center', value: 'id', sortable: true},
                    {text: '品牌名称', align: 'center', value: 'name', sortable: false},
                    {text: '品牌LOGO', align: 'center', value: 'image', sortable: false},
                    {text: '品牌首字母', align: 'center', value: 'letter', sortable: true},
                    {text: '操作', align: 'center', value: 'operating', sortable: false}
                ],
                brands: [],
                pagination: {},
                totalBrands: 0,
                loading: true,
                key: null,
            }
        },
        methods:{
            loadBrands(){
                this.loading = true;
                this.$http.get("/item/brand/page",{
                    params:{
                        page: this.pagination.page, //当前页
                        rows: this.pagination.rowsPerPage, //每页大小
                        sortBy: this.pagination.sortBy, //排序字段
                        dec: this.pagination.descending, //是否降序
                        key: this.key, //搜索条件
                    }
                }).then(resp => { // 这里使用箭头函数
                    this.brands = resp.data.items;
                    this.totalBrands = resp.data.total;
                    // 完成赋值后，把加载状态赋值为false
                    this.loading = false;
                })
            }
        },
       watch:{
           key() {
               this.pagination.page = 1;
               // this.loadBrands();
           },
           pagination:{
               deep: true,
               handler(){
                   console.log("重复请求");
                   this.loadBrands();
               }
           }
       },
    }
</script>
<style>

</style>

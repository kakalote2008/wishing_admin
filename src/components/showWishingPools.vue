<template>
  <el-main>
    <el-table :data="tableData" border highlight-current-row>
      <el-table-column prop="id" label="id">
      </el-table-column>
      <el-table-column prop="name" label="name">
      </el-table-column>
      <el-table-column prop="status" label="status(0收费1免费)">
      </el-table-column>
      <el-table-column prop="description" label="描述">
      </el-table-column>
      <el-table-column prop="wishing_count" label="许愿人数">
      </el-table-column>
      <el-table-column prop="wishing_card_ids" label="所关联许愿牌">
      </el-table-column>
      <el-table-column label="img_bg">
        <template slot-scope="scope">
          <img :src="scope.row.img_bg" height="100" width="100" />
        </template>
      </el-table-column>
      <el-table-column label="img_cover">
        <template slot-scope="scope">
          <img :src="scope.row.img_cover" height="100" width="100" />
        </template>
      </el-table-column>
      <el-table-column prop="create_time" label="create_time">
      </el-table-column>
      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button @click.native.prevent="deleteRow(scope.$index, tableData,scope.row)" type="text" size="small">
            删除
          </el-button>
          <el-button @click="editRow(scope.row)" type="text" size="small">
            编辑
          </el-button>
        </template>
      </el-table-column>
    </el-table>
  </el-main>
</template>

  <script>
export default {
  data() {
    return {
      tableData: []
    };
  },
  created() {
    // 组件创建完后获取数据，
    // 此时 data 已经被 observed 了
    this.fetchData();
  },
  methods: {
    fetchData() {
      this.$http.get(global.host + "/getWishingPools").then(
        successCallback => {
          console.log(successCallback.body);
          this.$data.tableData = successCallback.body.data;
        },
        errorCallback => {
          console.log(errorCallback.body);
        }
      );
    },
    deleteRow(index, tableData, row) {
      tableData.splice(index, 1);
      this.$http
        .post(global.host + "/deleteWishingPoolById", {
          id: row.id
        })
        .then(
          successCallback => {},
          errorCallback => {
            console.log(errorCallback.body);
          }
        );
    },
    editRow(row){
      this.$router.push({ path: "/createPool/"+row.id });
    }
  }
};
</script>

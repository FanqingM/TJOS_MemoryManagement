<template>
  <el-table
    :data="tableData"
    style="width: 100%"
    max-height="500">
    <el-table-column
      fixed
      prop="num"
      label="Sequence"
      width="150">
    </el-table-column>
    <el-table-column
      prop="instruct"
      label="Instruction"
      width="120">
    </el-table-column>
    <el-table-column
      prop="province"
      label="Frame1"
      width="120">
    </el-table-column>
    <el-table-column
      prop="city"
      label="Frame2"
      width="120">
    </el-table-column>
    <el-table-column
      prop="address"
      label="Frame3"
      width="300">
    </el-table-column>
    <el-table-column
      prop="zip"
      label="Frame4"
      width="120">
    </el-table-column>
    <el-table-column
    prop="discription"
    label="Page-Fault"
    width="120">
  </el-table-column>
  </el-table>
</template>

<script>
  export default {
    methods: {
      deleteRow(index, rows) {
        rows.splice(index, 1);
      }
    },
    data() {
      return {
        tableData: [{
          num: '2016-05-03',
          instructor: '王小虎',
          province: '上海',
          city: '普陀区',
          address: '上海市普陀区金沙江路 1518 弄',
          zip: 200333,
          discription:'null'
        }, {
          num: '2016-05-02',
          instructor: '王小虎',
          province: '上海',
          city: '普陀区',
          address: '上海市普陀区金沙江路 1518 弄',
          zip: 200333,
          discription:'null'
        }, {
          num: '2016-05-04',
          instructor: '王小虎',
          province: '上海',
          city: '普陀区',
          address: '上海市普陀区金沙江路 1518 弄',
          zip: 200333,
          discription:'null'
        }, {
          num: '2016-05-01',
          instructor: '王小虎',
          province: '上海',
          city: '普陀区',
          address: '上海市普陀区金沙江路 1518 弄',
          zip: 200333,
          discription:'null'
        }, {
          num: '2016-05-08',
          instructor: '王小虎',
          province: '上海',
          city: '普陀区',
          address: '上海市普陀区金沙江路 1518 弄',
          zip: 200333,
          discription:'null'
        }, {
          num: '2016-05-06',
          instructor: '王小虎',
          province: '上海',
          city: '普陀区',
          address: '上海市普陀区金沙江路 1518 弄',
          zip: 200333,
          discription:'null'
        }, {
          num: '2016-05-07',
          instructor: '王小虎',
          province: '上海',
          city: '普陀区',
          address: '上海市普陀区金沙江路 1518 弄',
          zip: 200333,
          discription:'null'
        }]
      }
    }
  }
</script>
<template>
  <!--基本表格-->
  <!-- <el-table
    :data="tableData"
    border
    style="width: 100%"
    height='260'>
    
    <el-table-column
      fixed
      prop="date"
      label="日期"
      width="150">
    </el-table-column>
    <el-table-column
      prop="name"
      label="姓名"
      width="120">
    </el-table-column>
    <el-table-column
      prop="province"
      label="省份"
      width="120">
    </el-table-column>
    <el-table-column
      prop="city"
      label="市区"
      width="120">
    </el-table-column>
    <el-table-column
      prop="address"
      label="地址"
      width="300">
    </el-table-column>
    <el-table-column
      prop="zip"
      label="邮编"
      width="120">
    </el-table-column>
    <el-table-column
      fixed="right"
      label="操作"
      width="100">
      <template slot-scope="scope">
        <el-button size="small" @click="handleClick(scope.row, scope.$index)" type="text" >查看</el-button>
        <el-button type="text" size="small">编辑</el-button>
      </template>
    </el-table-column>
  </el-table> -->
  <div>
    <el-table :data="tableData" border style="widht:100%">
      <el-table-column v-for="(item,index) in headerTitle" :key="index" width="150" :prop="item.names" :label="item.label" :fixed="index===0?true:false" :show-overflow-tooltip="true" align="center"></el-table-column>
      <el-table-column
        fixed="right"
        label="操作"
        width="100">
        <template slot-scope="scope">
          <el-button size="small" @click="handleClick(scope.row, scope.$index)" type="text" >查看</el-button>
          <el-button type="text" size="small" @click="edit(scope.row,scope.$index)">编辑</el-button>
        </template>
      </el-table-column>
    </el-table>
    <!-- 表单弹框 -->
    <el-dialog :title="tits" :visible.sync="tableVisible">
      <el-form ref="headerTitle"  label-width="80px">
        <el-form-item :label="item.label" v-for="(item,index) in headerTitle" :key="index">
          <el-input :disabled="tits=='查看'?true:false" v-if="item.type=='input'" v-model="item.value"></el-input>
          <el-select :disabled="tits=='查看'?true:false"  placeholder="请选择item.label" v-if="item.type=='select'" v-model="item.value">
            <el-option label="上海" value="上海"></el-option>
            <el-option label="北京" value="北京"></el-option>
          </el-select>
          <el-col :span="11" v-if="item.type=='date'">
            <el-date-picker :disabled="tits=='查看'?true:false" format="yyyy 年 MM 月 dd 日" value-format="yyyy-MM-dd" type="date" placeholder="选择日期"  style="width: 100%;" v-model="item.value"></el-date-picker>
          </el-col>
        </el-form-item>
          
       
        <el-form-item>
          <el-button @click="cancle">取消</el-button>
          <el-button type="primary" @click="onSubmit" v-if="tits !='查看'">确定</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>
  </div>
  
  
</template>

<script>
export default {

  data() {
    return {
      tits:'查看',
      i:'',
      tableVisible:false,
      headerTitle:[
        {
          type:'date',label:'日期',names:'date',value:''
        },{
          type:'input',label:'姓名',names:'name',value:''
        },{
          type:'select',label:'省份',names:'province',value:''
        },{
          type:'select',label:'市区',names:'city',value:''
        },{
          type:'input',label:'地址',names:'address',value:''
        },{
          type:'input',label:'邮编',names:'zip',value:''
        }
      ],
      tableData: [{
        date: '2016-05-03',
        name: '王小虎',
        province: '上海',
        city: '普陀区',
        address: '上海市普陀区金沙江路 1518 弄',
        zip: 200333
      }, {
        date: '2016-05-02',
        name: '王小虎',
        province: '上海',
        city: '普陀区',
        address: '上海市普陀区金沙江路 1518 弄',
        zip: 200333
      }, {
        date: '2016-05-04',
        name: '王小虎',
        province: '上海',
        city: '普陀区',
        address: '上海市普陀区金沙江路 1518 弄',
        zip: 200333
      }, {
        date: '2016-05-01',
        name: '王小虎',
        province: '上海',
        city: '普陀区',
        address: '上海市普陀区金沙江路 1518 弄',
        zip: 200333
      }, {
        date: '2016-04-28',
        name: '王小虎',
        province: '上海',
        city: '青浦区',
        address: '上海市青浦璞玉路 1518 弄',
        zip: 200333
      },{
        date: '2016-05-03',
        name: '王小虎',
        province: '上海',
        city: '普陀区',
        address: '上海市普陀区金沙江路 1518 弄',
        zip: 200333
      }, {
        date: '2016-05-02',
        name: '王小虎',
        province: '上海',
        city: '普陀区',
        address: '上海市普陀区金沙江路 1518 弄',
        zip: 200333
      }, {
        date: '2016-05-04',
        name: '王小虎',
        province: '上海',
        city: '普陀区',
        address: '上海市普陀区金沙江路 1518 弄',
        zip: 200333
      }, {
        date: '2016-05-01',
        name: '王小虎',
        province: '上海',
        city: '普陀区',
        address: '上海市普陀区金沙江路 1518 弄',
        zip: 200333
      }, {
        date: '2016-04-28',
        name: '王小虎',
        province: '上海',
        city: '青浦区',
        address: '上海市青浦璞玉路 1518 弄',
        zip: 200333
      }]
    }
  },

  methods: {
    handleClick(row, i) {
      console.log(row, i)
      this.headerTitle.forEach((it)=>{
        it.value=row[it.names]
      })
      this.tits='查看'
      this.tableVisible=true
      console.log(this.headerTitle)
    },
    edit(row,i){
      console.log(row,i)
      this.headerTitle.forEach((it)=>{
        it.value=row[it.names]
      })
      this.tableVisible=true
      this.tits='编辑'
      this.i=i;
    },
    cancle(){
      this.tableVisible=false
    },
    onSubmit(e){
      this.tableData.forEach((it,idx)=>{
        if(idx==this.i){
          this.headerTitle.map((t)=>{
            it[t.names]=t.value
          })
          
        }
      })
      this.cancle()
      console.log(e,'198',this.headerTitle)
    }
  }
}
</script>

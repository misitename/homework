<template>
  <div class="product_class">
    <div class="user-title">
      <span class="bars">
        当前位置：
        <span>商品分类</span>
      </span>
    </div>
    <el-card class="box-card">
      <div slot="header" class="clearfix">
        <el-button type="primary" class="el-icon-plus" size="mini" @click="addRow">添加</el-button>
      </div>
      <el-table :data="tableData" style="width: 100%">
        <el-table-column type="index" label="排序"></el-table-column>
        <el-table-column prop="date" label="分类名称"></el-table-column>
        <el-table-column prop="name" label="分类图片"></el-table-column>
        <el-table-column prop="address" label="上级分类"></el-table-column>
        <el-table-column prop="date" label="状态"></el-table-column>
        <el-table-column label="操作">
          <template slot-scope="scope">
            <el-button
              type="primary" icon="el-icon-edit" size="mini" circle
              @click.native.prevent="editRow(scope.$index, tableData)"
            ></el-button>
            <el-button
              type="danger" icon="el-icon-delete" size="mini" circle
              @click.native.prevent="deleteRow(scope.$index, tableData)"
            ></el-button>
          </template>
        </el-table-column>
      </el-table>
      <el-pagination
        background
        style="float:right;margin:20px 0 50px 0"
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="currentPage"
        :page-sizes="[5, 10, 20, 50, 100]"
        :page-size="pagesize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="tableData.length"
      ></el-pagination>
    </el-card>
    <!-- 编辑弹窗 -->
    <div class="edit_box">
      <el-dialog :title="title" :visible.sync="dialogFormVisible" width="35%">
        <el-form :model="ruleForm">
          <el-form-item label="排序" :label-width="formLabelWidth">
            <el-input v-model="ruleForm.address" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="分类名称" :label-width="formLabelWidth">
            <el-input v-model="ruleForm.date" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="上级分类" :label-width="formLabelWidth">
            <el-select v-model="ruleForm.name" placeholder="请选择状态">
              <el-option label="王小虎" value="王小虎"></el-option>
              <el-option label="王虎" value="王虎"></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="分类图标" :label-width="formLabelWidth">
            <el-upload
              class="upload-demo"
              style="width:60%"
              action="https://jsonplaceholder.typicode.com/posts/"
              :on-preview="handlePreview"
              :on-remove="handleRemove"
              :on-exceed="handle"
              :limit="1"
              list-type="picture"
            >
              <el-button size="small" type="primary">选择图片</el-button>
              <div slot="tip" class="el-upload__tip">（建议图片尺寸 1:1）图片最大张数为 1。</div>
            </el-upload>
          </el-form-item>
          <el-form-item label="是否显示" :label-width="formLabelWidth">
            <el-radio-group v-model="ruleForm.resource">
              <el-radio label="是"></el-radio>
              <el-radio label="否"></el-radio>
            </el-radio-group>
          </el-form-item>
          <el-form-item :label-width="formLabelWidth">
            <el-button size="small">取 消</el-button>
            <el-button type="primary" size="small" @click="cancel">确 定</el-button>
          </el-form-item>
        </el-form>
      </el-dialog>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      title:"",
      index: "",
      ruleForm: {
        date: "",
        name: "",
        address: "",
        resource: ""
      },
      dialogFormVisible: false,
      formLabelWidth: "120px",
      search: "",
      currentPage: 1,
      pagesize: 5,
      tableData: [
        {
          date: "2016-05-02",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1518 弄"
        },
        {
          date: "2016-05-02",
          name: "王虎",
          address: "上海市普陀区金沙江路 1518 弄"
        }
      ]
    };
  },
  methods: {
    // 分页
    handleSizeChange: function(size) {
      this.pagesize = size;
    },
    handleCurrentChange: function(currentPage) {
      this.currentPage = currentPage;
    },
    //编辑按钮
    editRow(index, rows) {
      this.dialogFormVisible = true;
      this.ruleForm.date = rows[index].date;
      this.ruleForm.name = rows[index].name;
      this.ruleForm.address = rows[index].address;
      this.title = "编辑信息";
      this.index = index;
    },
    //弹框取消
    cancel() {
      this.dialogFormVisible = false;
      this.ruleForm.date = "";
      this.ruleForm.name = "";
      this.ruleForm.address = "";
      this.ruleForm.resource = "";
    },
    //移除
    deleteRow(index, rows) {
      rows.splice(index, 1);
    },
    //添加
    addRow() {
      this.dialogFormVisible = true;
      this.ruleForm.date = "";
      this.ruleForm.name = "";
      this.ruleForm.address = "";
      this.ruleForm.resource = "";
      this.title = "添加分类"
    },
    //上传图片
    handleRemove(file, fileList) {
      // console.log(file, fileList);
    },
    handlePreview(file) {
      // console.log(file);
    },
    handle(files, fileList) {
      // console.log(files,fileList);
      alert("图片数量已上限！");
    }
  }
};
</script>
<style scoped>
.product_class {
  padding: 0 20px;
}
.user-title {
  width: 100%;
  height: 30px;
  box-sizing: border-box;
  margin: 10px 10px;
}
.user-title span {
  display: inline-block;
}
.user-title .bars {
  border-left: 5px solid #409eff;
  padding-left: 15px;
  line-height: 20px;
  font-size: 16px;
}
.user-title .bars span {
  color: #409eff;
}
</style>

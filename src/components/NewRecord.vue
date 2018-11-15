<template>
    <div class="signTwo">
              <!--新建建档-->
              <el-button style="float:right;margin:20px;" type="text" @click="createTableVisible = true">导入</el-button>
              <div class="newType">

              
              <el-row class="demo-autocomplete">
                <el-input class="inline-input" placeholder="姓名" v-model="input10" clearable>
                </el-input>
              </el-row>
              <el-row class="demo-autocomplete">
                <el-input class="inline-input" placeholder="学号" v-model="input9" clearable>
                </el-input>
              </el-row>
              <el-row class="demo-autocomplete">
                <el-input class="inline-input" placeholder="记录时间" v-model="input8" clearable>
                </el-input>
              </el-row>
              <el-row class="demo-autocomplete">
                <el-input class="inline-input" placeholder="地点" v-model="input7" clearable>
                </el-input>
              </el-row>
              
              <el-row class="demo-autocomplete">
                <el-input class="inline-input" placeholder="记录者" clearable>
                </el-input>
              </el-row>
              <el-row class="demo-autocomplete">
                <el-input class="inline-input" placeholder="记录者" clearable>
                </el-input>
              </el-row>
              
                <el-input class="inline-textarea" type="textarea" :rows="3" placeholder="请输入">
                </el-input>
              
              
              
                <el-input class="inline-textarea" type="textarea" :rows="3" placeholder="请输入">
                </el-input>
            </div>
            </div>
</template>

<script>
export default {
  methods:{
  }
}
</script>


<style scoped>
.signTwo {
  background-color: #fff;
}
.newType{
  height: 500px;
}
.demo-autocomplete {
      display: inline-block;
    }
    .inline-input {
      width: 80%;
      padding: 20px;
    }
</style>

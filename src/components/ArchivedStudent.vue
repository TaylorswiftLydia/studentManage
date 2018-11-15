<template>
    <div>
              <!--查询块-->
              <el-popover placement="bottom" style="position:relative" width="auto" trigger="click">
                <div>
                  <el-form :inline="true" :model="formInline" class="demo-form-inline">
                    <el-form-item label="学院">
                      <el-select v-model="formInline.college">
                        <el-option label="信息与软件工程学院" value="信息与软件工程学院"></el-option>
                      </el-select>
                    </el-form-item>
                    <el-form-item label="年级">
                      <el-select v-model="formInline.grade">
                        <el-option label="2018" value="2018"></el-option>
                        <el-option label="2017" value="2017"></el-option>
                        <el-option label="2016" value="2016"></el-option>
                        <el-option label="2015" value="2015"></el-option>
                      </el-select>
                    </el-form-item>
                    <el-form-item label="班级">
                      <el-input v-model="formInline.class" placeholder="如:2016220301"></el-input>
                    </el-form-item>
                    <el-form-item label="专业">
                      <el-select v-model="formInline.major">
                        <el-option v-for="major in allMajor" :label='major' :value="major" :key="major"></el-option>
                      </el-select>
                    </el-form-item>
                    <el-form-item label="面貌">
                      <el-select v-model="formInline.identity">
                        <el-option v-for="(identity,index) in allIdentities" :label='identity' :value="identity" :key="index"></el-option>
                      </el-select>
                    </el-form-item>
                    <el-form-item label="姓名">
                      <el-input v-model="formInline.name" placeholder="姓名"></el-input>
                    </el-form-item>
                    <el-form-item label="学号">
                      <el-input v-model="formInline.studenID" placeholder="如:2016220301001"></el-input>
                    </el-form-item>
                    <el-form-item label="籍贯">
                      <el-select v-model="formInline.province">
                        <el-option v-for="province in allProvinces" :label='province' :value="province" :key="province"></el-option>
                      </el-select>
                    </el-form-item>
                    <el-form-item label="民族">
                      <el-select v-model="formInline.nation">
                        <el-option v-for="nation in allNations" :label='nation' :value="nation" :key="nation"></el-option>
                      </el-select>
                    </el-form-item>
                    <el-form-item label="性别">
                      <el-select v-model="formInline.sex">
                        <el-option label="男" value="男"></el-option>
                        <el-option label="女" value="女"></el-option>
                      </el-select>
                    </el-form-item>
                    <el-form-item>
                      <el-button type="primary" item-width="100%" @click="findHistoryTable()">查询</el-button>
                    </el-form-item>
                  </el-form>
                </div>

                <div slot="reference" class="search-btn" type="primary">查询</div>
              </el-popover>
              <!--查看总体的记录-->
              <el-table :data="beforStuInfTable">
                <el-table-column v-for="(data,index) in straitened_stu_table" :prop="data.prop" :label="data.label" :key="index">
                </el-table-column>
                <el-table-column fixed="right" label="操作" width="100">
                  <div slot-scope="scope">
                    <el-button @click="lookClick(scope.row)" type="text" size="small">查看</el-button>
                    <el-button @click="disClick(scope.row)" type="text" size="small">讨论</el-button>
                  </div>
                </el-table-column>
              </el-table>
            </div>
</template>

<script>
export default {
    data(){
        return {
            formInline: {
            college: "",
            grade: "",
            major: "",
            class: "",
            identity: '',
            name: "",
            studentID: "",
            province: "",
            nation: "",
            sex: ""
        },
        allMajor: [
            "信息工程", "软件技术", "嵌入式系统", "网络安全工程", "大型主机", "信息与获取", "数字动漫", "留学生", "工业产品计算机", "数字信息处理", "互联网+", "互联网安全", "国际班", "英文授课留学生"
        ],
        allIdentities: [
            "中共党员", "共青团员", "预备党员", "积极分子", "群众"
        ],
        allProvinces: ["四川省", "河北省", "山西省", "吉林省", "辽宁省", "黑龙江省", "陕西省", "甘肃省", "青海省", "山东省",
            "福建省", "浙江省", "河南省", "湖北省", "湖南省", "江西省", "江苏省", "安徽省", "广东省", "海南省", "贵州省", "云南省",
            "北京", "上海", "重庆", "天津", "内蒙古", "新疆", "宁夏", "广西", "西藏", "台湾", "香港", "澳门"
        ],
        allNations: ["汉族", "壮族", "满族", "回族", "苗族", "维吾尔族", "土家族", "彝族", "蒙古族", "藏族", "布依族", "侗族",
            "瑶族", "朝鲜族", "白族", "哈尼族", "哈萨克族", "黎族", "傣族", "畲族", "傈僳族", "仡佬族", "东乡族", "高山族", "拉祜族",
            "水族", "佤族", "纳西族", "羌族", "土族", "仫佬族", "锡伯族", "柯尔克孜族", "达斡尔族", "景颇族", "毛南族", "撒拉族",
            "布朗族", "塔吉克族", "阿昌族", "普米族", "鄂温克族", "怒族", "京族", "基诺族", "德昂族", "保安族", "俄罗斯族", "裕固族",
            "乌孜别克族", "门巴族", "鄂伦春族", "独龙族", "塔塔尔族", "赫哲族", "珞巴族"
        ],
        //帮扶学生列表
        beforStuInfTable: [{
            historyArchiveId: "",
            studentId: "2016",
            sex: "男",
            name: "陈",
            major: "",
            grade: "大三",
            studentClass: "0302",
            ethnicGroup: "",
            contactWay: "电话",
            familyTelNumber: "123456",
            bulidingTime: "2018-01-01",
            helpType: "经济困难",
            attentionType: "一般关注",
            lastRecordTime: "2017-12-01"
        }],
        //困难学生横轴属性
        straitened_stu_table: [
            { prop: "name", label: "姓名" },
            { prop: "studentId", label: "学号" }, { prop: "grade", label: "年级" }, { prop: "studentClass", label: "班级" },
            { prop: "sex", label: "性别" }, { prop: "nation", label: "民族" }, { prop: "contactWay", label: "联系方式" },
            { prop: "familyTelNumber", label: "家庭电话" }, { prop: "bulidingBasis", label: "建档类型" }, { prop: "bulidingTime", label: "建档日期" }, { prop: "attentionType", label: "关注状态" }, { prop: "destoryingTime", label: "除档时间" }
        ],
        }
    },
    methods:{
      //8、搜索历史帮扶学生列表
        findHistoryTable: function () {
            this.postData('http://180.76.249.233:8080/newhelp/api/historyArchives', this.beforStuInfTable, "GET")
                .then(data => {
                    console.log(data.data);
                    this.data.beforStuInfTable = data.data;
                }) // JSON from `response.json()` call
                .catch(error => console.error(error));
        },
    }
}
</script>


<style scoped>
.search-btn {
      background-color: white;
      font-size: 20px;
      width: 100%;
      height: 50px;
      line-height: 50px;
      margin-bottom: 10px;
    }
.el-form-item {
      width: 50%;
      position: relative;
      left: 25%;
      display: flex;
      justify-content: flex-start;
    }
</style>

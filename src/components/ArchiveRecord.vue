<template>
     <div>
              <!--建档历史列表-->
              <el-table :data="record_table">
                <el-table-column v-for="(record,index) in record_table_attitude" :key="index" :prop="record.prop" :label="record.label">
                </el-table-column>
                <el-table-column fixed="right" label="操作" width="190">
                  <div slot-scope="scope">
                    <el-tooltip class="item" effect="dark" content="更改记录人" placement="top-end">
                      <el-button @click="change_recorder(scope.row)" size="small" icon="el-icon-document
                    "></el-button>
                    </el-tooltip>

                    <el-tooltip class="item" effect="dark" content="修改记录" placement="top-start">
                      <el-button @click="change_record_inf(scope.row)" icon="el-icon-edit
                    " size="small"></el-button>
                    </el-tooltip>

                    <el-tooltip class="item" effect="dark" content="删除" placement="top-start">
                      <el-button @click="delRecordDialog = true " icon="el-icon-delete
                    " size="small"></el-button>
                    </el-tooltip>
                  </div>
                </el-table-column>
              </el-table>
            </div>
</template>

<script>
export default {
    data(){
        return {
          //记录列表
          record_table: [
            {
                recordId: "2016",
                recordName: "陈",
                recordTime: "2018-01-01",
                location: "",
                witness: "",
                recorder: "",
                participant: "",
                way: "",
                content: "",
                comment: "",
            },
        ],
        //记录列表属性
        record_table_attitude: [
            { prop: "recordId", label: "记录号" },
            { prop: "recordName", label: "姓名" },
            { prop: "recordTime", label: "记录时间" },
            { prop: "location", label: "地点" },
            { prop: "witness", label: "证人" },
            { prop: "recorder", label: "记录者" },
            { prop: "participant", label: "参与者" },
            { prop: "way", label: "方式" },
            { prop: "content", label: "内容" },
            { prop: "comment", label: "评论" }
        ],
        }
    },
    methods:{
      change_recorder: function (row) {
            this.changeRecorderDialog = true;
            console.log(row);
            //this.tempRecordInf = row;
        },
         change_record_inf: function (row) {
            this.changeRecordDialog = true;
            console.log(row);
        },
    },
    created() {
        let token = sessionStorage.getItem('token')
        let teacherId = sessionStorage.getItem('userName');
        //7、查看历史帮扶学生列表
         $.ajax({
        type:"GET",
        beforeSend:function(request){
          request.setRequestHeader("Authorization",token);
        },
        url:"http://180.76.249.233:8080/newhelp/api/historyArchives/"+teacherId,
        contentType: "application/json; charset=utf-8",
        dataType:"json",
        success:function(data){
          if(data.success){
            var allDifficultStu = data.data;
            for(var k = 0; k < allDifficultStu.length; k++){
              var thisDifficultStu = allDifficultStu[k];

              $(".historyArchives").children().eq(1).append(
                "<tr>"+
                  "<input type='hidden' value = '" + thisDifficultStu.historyArchiveId + "'/" + ">" +
                  "<td onclick='showRecord(this)'><a>"  + thisDifficultStu.name + "</a></td>"+
                  "<td name='stuId'>" + thisDifficultStu.studentId + "</td>"+
                  "<td>" + thisDifficultStu.grade + "</td>"+
                  "<td>" + thisDifficultStu.studentClass + "</td>"+
                  "<td>" + thisDifficultStu.sex + "</td>"+
                  "<td>" + thisDifficultStu.ethnicGroup + "</td>"+
                  "<td>" + thisDifficultStu.contactWay + "</td>"+
                  "<td>" + thisDifficultStu.familyTelNumber + "</td>"+
                  "<td>" + thisDifficultStu.helpType + "</td>"+
                  "<td>" + thisDifficultStu.bulidingTime + "</td>"+
                  "<td>" + thisDifficultStu.attentionType + "</td>"+
                  "<td>" + thisDifficultStu.destoryingTime + "</td>"+
                "</tr>"
              );
            }
            $('tbody').children().on("mouseenter",function(event){
                $(this).addClass("info");
            });
            $('tr').on("mouseout",function(event){
                $(this).removeClass("info");
            });

            for(var j = 0; j < $(".tableRecord").length ;j++)
           {
            $(".tableRecord").eq(j).children().eq(0).children().children().eq(0).css("display","none");
            var thisTr = $(".tableRecord").eq(j).children().eq(1).children();
            for(var i = 0 ; i < thisTr.length ;i++)
            {
              thisTr.eq(i).children().eq(0).css("display","none");
              thisTr.eq(i).children().eq(6).children().eq(1).css("display","none");
              thisTr.eq(i).children().eq(6).children().eq(2).css("display","none");

            }
          }

          var creat = $('button[name = "creat"]');
          for(var j = 0; j < creat.length ;j++)
          {
             creat.eq(j).css("display","none");
             creat.eq(j).parents().children().eq(3).css("display","none");
             creat.eq(j).parents().children().eq(2).css("display","none");

          }


          }else{
            alert(data.message);
          }
        },
        error:function(data){
         alert("无法获取历史信息");
        },
      });
        //2、查看记录列表
        this.postData('http://180.76.249.233:8080/newhelp/api/records/{recordName}/{studentId}', {}, "GET")
            .then(data => {
                console.log(data.data);
                this.data.record_table = data.data;
            }) // JSON from `response.json()` call
            .catch(error => console.error(error));
            this.nickname = localStorage.teacherName;
    },
    mounted() {
        this.students = this.loadAll();
    },
}
</script>

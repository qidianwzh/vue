<template>
      <div class="classmain">
          <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm" style="width:500px">
          <el-form-item label="标题" prop="title">
            <el-input v-model="ruleForm.title"></el-input>
          </el-form-item>
          <el-form-item label="接口地址" prop="url">
            <el-input v-model="ruleForm.url"></el-input>
          </el-form-item>
          <el-form-item label="类型" prop="backorfont">
            <el-select v-model="ruleForm.backorfont" placeholder="请选择活动区域">
              <el-option label="前台接口" value="font"></el-option>
              <el-option label="后台接口" value="back"></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="请求类型" prop="type">
            <el-select v-model="ruleForm.type" placeholder="请选择活动区域">
              <el-option label="post" value="post"></el-option>
              <el-option label="get" value="get"></el-option>
              <el-option label="formdata" value="formdata"></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="请求参数" prop="sendparams">
            <el-input type="textarea" v-model="ruleForm.sendparams" :autosize='{ minRows: 2, maxRows: 10 }'></el-input>
          </el-form-item>
          <el-form-item label="响应数据" prop="getparams">
            <el-input type="textarea" v-model="ruleForm.getparams" :autosize='{ minRows: 2, maxRows: 10 }'></el-input>
          </el-form-item>
         <el-form-item >
            <el-button type="primary" @click="submitForm('ruleForm')">提交</el-button>
            <el-button @click="resetForm('ruleForm')">重置</el-button>
          </el-form-item>
        </el-form>
      </div>
</template>
<script>
export default {
  name: 'index',
 data() {
      return {
        ruleForm: {
          title: '',
          url: '',
          type: '',
          backorfont: '',
          sendparams: "",
          getparams: "",
          
        },
        state:false,
        rules: {
          title: [
            { required: true, message: '不能为空', trigger: 'blur' },
          ],
           url: [
            { required: true, message: '不能为空', trigger: 'blur' },
          ],
           type: [
            { required: true, message: '不能为空', trigger: 'blur' },
          ],
           backorfont: [
            { required: true, message: '不能为空', trigger: 'blur' },
          ],
           sendparams: [
            { required: true, message: '不能为空', trigger: 'blur' },
          ],
           getparams: [
            { required: true, message: '不能为空', trigger: 'blur' },
          ],
          
        }
      };
    },
    created(){
      let sesstion=JSON.parse(sessionStorage.getItem("apiamend"))
      console.log(sesstion)
      if(this.$route.query.type=="amend"){
        this.state=true
        this.ruleForm=sesstion
      }
    },
    methods: {
      submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            let url="",code=""
            if(this.state){
              code="1031"
                url="/api/back/api/update"
            }else{
              code="1011"
                url="/api/back/api/insert"
            }
            console.log(this.ruleForm)
            this.axios.post(url,this.ruleForm).then((data)=>{
             
             if(data.data.code==code){
                 this.$message({
                    showClose: true,
                    message: '恭喜你，提交成功',
                    type: 'success'
                  });
             }else{
                this.$message({
                    showClose: true,
                    message: '提交失败',
                    type: 'error'
                  });
             }
           })


          } else {
            console.log('error submit!!');
            return false;
          }
        });
      },
      resetForm(formName) {
        this.$refs[formName].resetFields();
      }
    }
  
  
}
</script>

<style scoped>
.classmain{
  padding:30px 20px;
}
p{
  font-size: 20px;
  font-weight: bold;
  padding:10px;
}
</style>

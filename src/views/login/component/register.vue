<template>
    <el-form :model="registerForm" :rules = rules ref='regiForm' status-icon label-width="100px" class="login">
        <el-form-item label="用户名" prop="username">
            <el-input v-model="registerForm.username" placeholder="请输入姓名"></el-input>
            <!-- <p>{{ registerForm.username }}</p>
            <p>Stay</p> -->
        </el-form-item>
        <el-form-item label="邮箱" prop="email">
            <el-input v-model="registerForm.email"></el-input>
        </el-form-item>
        <el-form-item label="密码" prop="password">
            <el-input type="password" v-model="registerForm.password" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="确认密码" prop="checkPassword">
            <el-input type="password" v-model="registerForm.checkPassword" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item>
            <el-button type="primary" @click="submitForm('regiForm')">提交</el-button>
            <!-- <el-button @click="resetForm('ruleForm')">重置</el-button> -->
        </el-form-item>
    </el-form>
</template>
<script setup lang = 'ts'>
import { reactive } from 'vue';
import { ref } from 'vue';

type regForm = {
    username: string;
    email: string;
    password: string;
    checkPassword: string;
};


const registerForm = reactive<regForm>({
    username: "",
    email: "",
    password: "",
    checkPassword: ""
});

const regiForm = ref();

const checkEmail = (rule, email: string, callback) => {
    if (email === "") {
        return callback(new Error('邮箱不能为空'));
    }
    setTimeout(() => {
        if (!email.includes('@') || !email.includes(".com")) {
            callback(new Error('请输入有效邮箱'));
        } else {
            callback();

        }
    }, 1000);
};
function validatePass (rule, value, callback) {
    if (value === '') {
        callback(new Error('请输入密码'));
    } else {
        if (registerForm.checkPassword !== '') {
            this.regiForm.validateField('checkPass');
        }
        callback();
    }
};
const validatePass2 = (rule, value, callback) => {
    console.log(value);
    console.log(registerForm.password)
    if (value === '') {
        callback(new Error('请再次输入密码'));
    } else if (value !== registerForm.password) {
        callback(new Error('两次输入密码不一致!'));
    } else {
        callback();
    }
};

const rules = {
    password: [
        { validator: validatePass, trigger: 'blur' }
    ],
    checkPassword: [
        { validator: validatePass2, trigger: 'blur' }
    ],
    email: [
        { validator: checkEmail, trigger: 'blur' }
    ]
};

function submitForm (formName: string) {
    // console.log(this.$refs[formName]);
    // console.log(registerForm);
    // console.log(this);
    this.regiForm.validate((valid) => {
        if (valid) {
            alert('submit!');
        } else {
            console.log('error submit!!');
            return false;
        }
    });
}


// export default {
//       data() {
//         var checkAge = (rule, value, callback) => {
//           if (!value) {
//             return callback(new Error('年龄不能为空'));
//           }
//           setTimeout(() => {
//             if (!Number.isInteger(value)) {
//               callback(new Error('请输入数字值'));
//             } else {
//               if (value < 18) {
//                 callback(new Error('必须年满18岁'));
//               } else {
//                 callback();
//               }
//             }
//           }, 1000);
//         };
//         var validatePass = (rule, value, callback) => {
//           if (value === '') {
//             callback(new Error('请输入密码'));
//           } else {
//             if (this.ruleForm.checkPass !== '') {
//               this.$refs.ruleForm.validateField('checkPass');
//             }
//             callback();
//           }
//         };
//         var validatePass2 = (rule, value, callback) => {
//           if (value === '') {
//             callback(new Error('请再次输入密码'));
//           } else if (value !== this.ruleForm.pass) {
//             callback(new Error('两次输入密码不一致!'));
//           } else {
//             callback();
//           }
//         };
//         return {
//           ruleForm: {
//             pass: '',
//             checkPass: '',
//             age: ''
//           },
//           rules: {
//             pass: [
//               { validator: validatePass, trigger: 'blur' }
//             ],
//             checkPass: [
//               { validator: validatePass2, trigger: 'blur' }
//             ],
//             age: [
//               { validator: checkAge, trigger: 'blur' }
//             ]
//           }
//         };
//       },
//       methods: {
//         submitForm(formName) {
//           this.$refs[formName].validate((valid) => {
//             if (valid) {
//               alert('submit!');
//             } else {
//               console.log('error submit!!');
//               return false;
//             }
//           });
//         },
//         resetForm(formName) {
//           this.$refs[formName].resetFields();
//         }
//       }
//     }
</script>

<style scoped lang="scss">
.login-content-form {
	margin-top: 20px;
	@for $i from 1 through 4 {
		.login-animation#{$i} {
			opacity: 0;
			animation-name: error-num;
			animation-duration: 0.5s;
			animation-fill-mode: forwards;
			animation-delay: calc($i/10) + s;
		}
	}
}
</style>
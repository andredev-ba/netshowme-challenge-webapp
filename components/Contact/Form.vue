<template>
  <div class="form-contact">
    <el-card class="box-card">
      <div slot="header" class="clearfix box-card-header">
        <div class="logo" />
        <span>Formulário de Contato</span>
      </div>
      <el-form :rules="rules" size='small' label-position='top' ref="form" :model="form">
        <el-form-item label="Nome Completo" prop="name">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
        <el-form-item label="E-mail" prop="email">
          <el-input v-model="form.email"></el-input>
        </el-form-item>
        <el-form-item label="Telefone" prop="phone">
          <el-input v-mask.native="['(##) #####-####', '(##) ####-####']" v-model="form.phone"></el-input>
        </el-form-item>
        <el-form-item label="Mensagem" prop="message">
          <el-input rows="10" type="textarea" v-model="form.message"></el-input>
        </el-form-item>
        <el-form-item label="Anexar arquivo" prop="file">
          <input :type="'file'" id="file" ref="file" @change="onChangeFileUpload()" />
        </el-form-item>
        <el-form-item style="margin-top: 40px;">
          <el-button style="float:right" type="primary" @click="submitForm('form')">Enviar</el-button>
          <NuxtLink tag="el-button" style="float:right; margin-right: 10px;" to="/">Cancelar</NuxtLink>
        </el-form-item>
      </el-form>
    </el-card>
  </div>
</template>

<script>
import { mask } from 'vue-the-mask'

export default {
  directives: { mask },
  data () {
    var file = (rule, value, callback) => {
      if (!value) {
        return callback(new Error());
      }
      const extensionsAllowed = ['pdf','doc','docx','odt','txt']
      const ext = value.name.split('.').pop();

      // if not valid extension
      if (extensionsAllowed.indexOf(ext) == -1) {
        return callback(new Error());
      }
      
      callback();
    };
    return {
      form: {
        name: '',
        email: '',
        phone: '',
        message: '',
        file: null
      },
      rules: {
        name: [
          { required: true, message: 'Por favor, digite seu nome completo.', trigger: 'change' },
          { min: 6, message: 'O nome completo precisa conter o mínimo de 6 caracteres', trigger: 'change' }
        ],
        email: [
          { required: true, type: 'email', message: 'Por favor, digite um e-mail válido', trigger: 'change' }
        ],
        phone: [
          { required: true, min: 14, max: 15, message: 'Por favor, digite um telefone válido', trigger: 'change' }
        ],
        message: [
          { required: true, message: 'Por favor, digite uma mensagem', trigger: 'change' }
        ],
        file: [
          { validator: file, message: 'Por favor, anexo um arquivo válido. Tipos permitidos: PDF, DOC, DOCX, ODT, TXT', trigger: 'change' }
        ]
      }
    }
  },
  methods: {
    submitForm (formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.storeContact()
        } else {
          this.$notify({
            title: 'Atenção',
            message: 'Preencha todos os campos obrigátios.',
            type: 'warning'
          });
          return false
        }
      })
    },
    async storeContact() {
      try {
        let formData = new FormData();
        formData.append('name', this.form.name)
        formData.append('email', this.form.email)
        formData.append('phone', this.form.phone)
        formData.append('message', this.form.message)
        formData.append('file', this.form.file)
    
        const data = await this.$axios.$post('contact', formData, { headers: { "Content-Type": "multipart/form-data" } })
        
        this.$notify({
          title: 'Tudo certo',
          message: `Dados enviados com sucesso`,
          type: 'success'
        });

        this.$router.push('/')

      } catch({ response }) {
        this.$notify({
          title: response.data.message,
          message: `Ocorreu um erro ao enviar o formulário: ${JSON.stringify(response.data.details)}`,
          type: 'erro'
        });
      }
    },
    onChangeFileUpload(){
      this.form.file = this.$refs.file.files[0];
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
 .form-contact {
   margin: 0 auto;
   min-height: 100vh;
   display: flex;
   justify-content: center;
   align-items: center;
 }

 .form-contact .box-card {
   width: 50%;
 }

 .form-contact .box-card-header {
   display: flex;
   justify-content: space-between;
 }

 .form-contact .logo {
   content: url(https://netshow.me/wp-content/uploads/2019/11/logo-nsm-red-2.svg);
   width: 20%;
 }
</style>

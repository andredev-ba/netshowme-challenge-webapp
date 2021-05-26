<template>
  <div class="list-contact">
    <el-card class="box-card">
      <div slot="header" class="clearfix box-card-header">
        <div class="logo" />
        <span>Lista de Contatos</span>
      </div>
      <div class="list-contact-body">
        <div>
          <el-table
            :data="contactList"
            stripe
            border
            style="width: 100%">
            <el-table-column
              prop="name"
              label="Nome Completo"
            >
            </el-table-column>
            <el-table-column
              prop="email"
              label="Email"
            >
            </el-table-column>
            <el-table-column
              prop="phone"
              label="Telefone">
            </el-table-column>
            <el-table-column
              prop="message"
              label="Mensagem">
            </el-table-column>
            <el-table-column
              prop="file_path"
              label="Anexo">
            </el-table-column>
            <el-table-column
              prop="client_ip"
              label="IP">
            </el-table-column>
          </el-table>
        </div>
        <div>
          <NuxtLink tag="el-button" style="margin-top: 50px; float:right" to="/">Voltar</NuxtLink>
        </div>
      </div>
    </el-card>
  </div>
</template>

<script>
import { mask } from 'vue-the-mask'

export default {
  directives: { mask },
  data () {
    return {
      contactList: []
    }
  },
  created () {
    this.getContactList()
  },
  methods: {
    async getContactList () {
      const response = await this.$axios.$get('contact')
      this.contactList = response
    },
  }
}
</script>

<style scoped>
 .list-contact {
   margin: 0 auto;
   display: flex;
   justify-content: center;
   align-items: center;
   min-height: 100vh;
 }

 .list-contact .box-card {
   width: 50%;
   min-height: 80vh;
 }

 .list-contact-body {
   height: 100% !important;
   display: flex;
   flex-direction: column;
   justify-content: space-between;
 }

 .list-contact .box-card-header {
   display: flex;
   justify-content: space-between;
 }

 .list-contact .logo {
   content: url(https://netshow.me/wp-content/uploads/2019/11/logo-nsm-red-2.svg);
   width: 20%;
 }
</style>

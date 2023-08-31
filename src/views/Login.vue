<template>
  <v-form ref="form" @submit.prevent="handleSubmit">
    <v-text-field 
      variant="outlined"
     v-model="usuario.email" 
     type="email" 
     label="Email" 
     placeholder="Digite o email" 
     :rules="[value => !!value || 'O email é obrigatório!']"
     />

    <v-text-field 
      variant="outlined"
      v-model="usuario.password" 
      type="password" 
      label="Senha" 
      placeholder="Digite a senha" 
     :rules="[value => !!value || 'O senha é obrigatória!']"
     />
    
    <v-btn type="submit">Fazer login</v-btn>
  </v-form>
</template>

<script>
import axios from "axios"

export default {
  data() {
    return {
      usuario: {
        email: "",
        password: ""
      },
    }
  },
  methods: {
    async handleSubmit(){
      const {valid} = await this.$refs.form.validate()

      if(!valid){
        alert("Preencha todos os dados!")
        return
      }

      try {
        const result = await axios.post('http://localhost:3000/sessions', this.usuario)

        if(result.status == 200){
          debugger
          localStorage.setItem("user-info", JSON.stringify(result.data))
          this.$router.push('/dashboard')
        }
        
        console.log(result)

      } catch (error) {
        console.log(error.response.data.error)
        alert("Usuário não cadastrado!")

      }

      
    }
  }
}
</script>

<style></style>

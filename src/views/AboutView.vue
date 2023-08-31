<template>
  <v-form ref="form" @submit.prevent="handleSubmit">
    <v-text-field 
      variant="outlined" 
      v-model="aluno.nome" 
      :rules="[value => !!value || 'O nome é obrigatório']"
      type="text" 
      label="Nome" 
      placeholder="Digite o nome" 
    />
    
    <v-text-field 
      variant="outlined" 
      v-model="aluno.idade" 
      :rules="[value => value > 10 || 'A idade mínima é 10 anos',
      value => value < 100 || 'A idade máxima é 100 anos']"
      type="number" 
      label="Idade" 
      placeholder="Digite a idade"
    />

    <v-text-field 
      variant="outlined"
     v-model="aluno.email" 
     type="email" 
     label="Email" 
     placeholder="Digite o email" 
     />

    <v-select 
      :items="items" 
      v-model="aluno.curso" 
      variant="outlined" 
      label="Curso"
      placeholder="Selecione o curso">
    </v-select>
    <v-btn type="submit">Cadastrar</v-btn>
  </v-form>
</template>

<script>
export default {
  data() {
    return {
      aluno: {
        nome: "",
        idade: null,
        email: "",
        curso: ""
      },
      items: ['HTML', 'CSS', 'JAVASCRIPT', 'VUEJS', 'PHP']
    }
  },
  methods: {
    async handleSubmit(){
      const {valid} = await this.$refs.form.validate()

      if(!valid){
        alert("Preencha todos os dados!")
        return
      }

      const result = confirm("Aluno cadastrado com sucesso! Deseja ir para o dashboard?")
      this.$refs.form.reset()
    }
  }
}
</script>

<style></style>

<template>
  {{ aluno }}
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
      :items="diasLista" 
      v-model="aluno.day" 
      variant="outlined" 
      label="Selecione o dia"
      placeholder="Selecione o dia">
    </v-select>
    <v-select 
      :items="exercises" 
      v-model="aluno.exercise_id" 
      variant="outlined" 
      item-title="description"
      item-value="id"
      label="Selecione o exercício"
      placeholder="Selecione o exercício">
    </v-select>
    <v-btn type="submit">Cadastrar</v-btn>
  </v-form>
</template>

<script>
import axios from "axios"
export default {
  data() {
    return {
      aluno: {
        student_id: 0,
        exercise_id: 0,
        repetitions: 0,
        weight: 0,
        break_time: 0,
        observations: "",
        day: "segunda"
      },
      diasLista: [
        {
          title: 'Segunda-feira',
          value: 'segunda'
        },
        {
          title: 'Terça-feira',
          value: 'terca'
        },
        {
          title: 'Quarta-feira',
          value: 'quarta'
        },
        {
          title: 'Quinta-feira',
          value: 'quinta'
        },
        {
          title: 'Sexta-feira',
          value: 'sexta'
        },
        {
          title: 'Sábado',
          value: 'sabado'
        },
        {
          title: 'Domingo',
          value: 'domingo'
        }
      ],
      exercises: []
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
  },
  mounted(){
    axios.get('http://localhost:3000/exercises')
    .then(res => this.exercises = res.data)
  }
}
</script>

<style></style>

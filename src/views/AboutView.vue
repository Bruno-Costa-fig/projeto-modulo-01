<template>
  {{ date }}
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

     <img src="./sala-de-aula.png" alt="Imagem representando uma sala de aula">
    
     <label>Selecione a data de nascimento</label>
    <VueDatePicker 
      v-model="date" 
      :max-date="new Date()" 
      locale="pt-BR" 
      cancelText="cancelar" 
      selectText="Selecionar"
      label="Selecione a data"
      :format="format"
      :enable-time-picker="false"
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
import moment from "moment"
export default {
  data() {
    return {
      date: "",
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
  watch: {
    date: (newValue) => {
      let dataFormatada = moment(newValue).format("DD/MM/YYYY")
      return dataFormatada
    }
  },
  methods: {
    format(date) {
      const day = date.getDate();
      const month = date.getMonth() + 1;
      const year = date.getFullYear();

      return `${day}/${month}/${year}`;
    },
    async handleSubmit(){
      const {valid} = await this.$refs.form.validate()

      if(!valid){
        alert("Preencha todos os dados!")
        return
      }

      let dataFormatada = moment(this.data).format("DD/MM/YYYY")

      const result = confirm("Aluno cadastrado com sucesso! Deseja ir para o dashboard?")
      this.$refs.form.reset()
    }
  },
  mounted(){
    axios.get(`${import.meta.env.VITE_URL_API_BASE}/exercises`)
    .then(res => this.exercises = res.data)
  }
}
</script>

<style></style>

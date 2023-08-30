<template>
  <main>
    <h1>Formulário de cadastro</h1>
    <Form :validation-schema="alunoSchema" @submit="handleSubmit" v-slot="{ errors }">
      <Field type="text" name="nome" v-model="aluno.nome" placeholder="Digite o nome" />
      {{ errors.nome }}
      <Field type="number" name="idade" v-model="aluno.idade" placeholder="Digite a idade" />
      {{ errors.idade }}
      <Field type="email" name="email" v-model="aluno.email" placeholder="Digite o email" />
      {{ errors.email }}
      <button type="submit">Cadastrar</button>
    </Form>
  </main>
</template>

<script>
import { Form, Field } from "vee-validate"
import * as Yup from "yup"
export default {
  components: {
    Form,
    Field
  },
  data() {
    return {
      alunoSchema: Yup.object().shape({
        nome: Yup.string().required("O nome é obrigatório!"),
        idade: Yup.number().required("A idade é obrigatória!").min(10, "A idade mínima é 10 anos"),
        email: Yup.string().email()
      }),
      aluno: {
        nome: "",
        idade: 0,
        email: ""
      }
    }
  },
  methods: {
    handleSubmit() {
      alert("Cadastro realizado!")
    }
  },
}
</script>
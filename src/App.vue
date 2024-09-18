<script setup>
import { reactive } from "vue";

const estado = reactive({
  filtro: "todos",
  tarefas: [
    {
      titulo: "Fazer a comida",
      finalizada: false,
    },
    {
      titulo: "Ir para academia",
      finalizada: true,
    },
    {
      titulo: "Comer pela manha",
      finalizada: false,
    },
  ],
  newValue: "",
});

const getTarefasPendente = () => {
  return estado.tarefas.filter((item) => !item.finalizada);
};

const getTarefasFinalizadas = () => {
  return estado.tarefas.filter((item) => item.finalizada);
};

const filtrarSelect = () => {
  const { filtro } = estado;

  switch (filtro) {
    case "pendentes":
      return getTarefasPendente();
    case "finalizadas":
      return getTarefasFinalizadas();
    default:
      return estado.tarefas;
  }
};

function adicionarNovaTerefa() {
  if (estado.newValue.trim() !== "") {
    estado.tarefas.push({
      titulo: estado.newValue,
      finalizada: false,
    });
  }
  estado.newValue = ""
}
</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <h1>Minhas Tarefas</h1>
      <p>Voçe possui {{ getTarefasPendente().length }} tarefas pendentes</p>
    </header>
    <form @submit.prevent="adicionarNovaTerefa()">
      <div class="row">
        <div class="col">
          <input
            type="text"
            v-model="estado.newValue"
            placeholder="digite aqui a descrição da tarefa"
            class="form-control"
          />
        </div>
        <div class="col-md-2">
          <button
            class="btn btn-primary"
            type="submit"
          >
            Cadastrar
          </button>
        </div>
        <div class="col-md-3">
          <select
            class="form-control"
            @change="(event) => (estado.filtro = event.target.value)"
          >
            <option value="todas">Todas as Tarefas</option>
            <option value="pendentes">Pendentes</option>
            <option value="finalizadas">Finalizadas</option>
          </select>
        </div>
      </div>
    </form>
    <div class="list-group mt-4">
      <li class="list-group-item" v-for="tarefa in filtrarSelect()">
        <input
          v-model="tarefa.finalizada"
          :checked="tarefa.finalizada"
          :id="tarefa.titulo"
          type="checkbox"
        />
        <label :class="{ done: tarefa.finalizada }" class="ms-3" for="">
          {{ tarefa.titulo }}
        </label>
      </li>
    </div>
  </div>
</template>

<style scoped>
.done {
  text-decoration: line-through;
}
</style>

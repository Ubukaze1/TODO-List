<script setup lang="ts">
import { ref, Ref, onMounted, watch, computed } from "vue";

interface list {
  title: string
  select: string
  hecho: boolean
}

const color = computed((text: string) => {
  switch (selec.value) {
    case "Negocio":
      return "in-neg"
    case "Personal":
      return "in-per"
  }
})

const colorr = (col: string) => {
  switch (col) {
    case "Negocio":
      return "in-neg"
    case "Personal":
      return "in-per"
  }

}

const dele = (title: string)  => {
  lista.value = lista.value.filter(t => t.title != title)
}

const name: Ref<string> = ref("");
const title: Ref<string> = ref("")
const selec: Ref<string> = ref("")
const lista: Ref<Array<list>> = ref([])

watch(name, (newval) => {
  localStorage.setItem("name", newval);
});

watch(lista, (newval) => {
  localStorage.setItem("Lista", JSON.stringify(newval))
}, { deep: true })

const añadir = () => {

  if (title.value.trim() === "" || selec.value === "") {
    return
  }

  lista.value.push({
    title: title.value,
    select: selec.value,
    hecho: false
  })
  title.value = ""
  selec.value = ""
}

onMounted(() => {
  name.value = localStorage.getItem("name") || "";
  lista.value = JSON.parse(localStorage.getItem("Lista") || "") || []
});
</script>

<template>
  <main>
    <div class="container">
      <section class="first">
        <h2>
          Hola
          <input v-model="name" autofocus type="text" placeholder="Tu nombre" />
        </h2>
      </section>
      <section class="secont">
        <form @submit.prevent="añadir">
          <h3>Crear Todo</h3>
          <h4>¿Que tienes que hacer?</h4>
          <input placeholder="Ej. Estudiar" type="text" class="input-todo" v-model="title" />
          <h4>Catergoria</h4>
          <div class="selec">
            <div class="selec-all">
              <input class="in-neg" type="radio" name="in-in" value="Negocio" v-model="selec" />
              <label>Negocio</label>
            </div>
            <div class="selec-all">
              <input class="in-per" type="radio" name="in-in" value="Personal" v-model="selec" />
              <label>Personal</label>
            </div>
          </div>
          <button class="bt-aña">Añadir</button>
          <h3>Lista</h3>
        </form>
      </section>
      <section class="third">
        <div class="lista">
          <div class="list" v-for="lis in lista" :key="lis.title">
            <input type="checkbox" :class="colorr(lis.select)" v-model="lis.hecho" />
            <input type="text" v-model="lis.title" />
            <button @click="dele(lis.title)">Delete</button>
          </div>

        </div>
      </section>
    </div>
  </main>
</template>

<style scoped>
main {
  width: 30vw;
  height: 100vh;
  margin: 0 auto;
}

.container {
  width: 1000px;
  padding: 20px;
  margin: 100px auto;
}

.first input {
  border: none;
  font-size: 24px;
  font-weight: bold;
  background-color: whitesmoke;
}

.first input:focus {
  outline: none;
}

.secont {
  display: flex;
  flex-direction: column;
}

h3 {
  font-weight: lighter;
  margin-bottom: 2px;
  margin-top: 8px;
}

h4 {
  margin-top: 4px;
  margin-bottom: 10px;
  font-size: 12px;
  color: gray;
}

h2 {
  margin: 0;
}

.input-todo {
  width: 350px;
  padding: 10px;
  border: none;
  font-size: 16px;
  font-weight: 200;
}

.input-todo:focus {
  outline: none;
}

.selec {
  display: flex;
}

.selec-all {
  margin-right: 10px;
  display: flex;
  flex-direction: column;
  text-align: center;
  width: 170px;
  height: 85px;
  border-radius: 10px;
  background-color: white;
}

.in-neg,
.in-per {
  margin-top: 20px;
  cursor: pointer;
}

.in-neg:focus,
.in-per:focus {
  outline: none;
}

.in-neg {
  accent-color: blue;
}

.in-per {
  accent-color: red;
}

.selec-all label {
  margin-top: 10px;
  font-weight: lighter;
}

.bt-aña {
  border: none;
  margin-top: 15px;
  margin-bottom: 10px;
  width: 350px;
  padding: 10px;
  border-radius: 10px;
  background-color: #d9534f;
  color: white;
  cursor: pointer;
}

.third {
  display: flex;
  flex-direction: column;
  width: 350px;
  margin-top: 10px;
  background-color: white;
  border-radius: 5px;
}

.list {
  display: flex;
  padding: 10px;
  height: 50px;
}

.list input[type="checkbox"] {
  margin: 0;
}

.list input[type="text"] {
  border: none;
  margin-left: 10px;
  padding-left: 10px;
  margin-right: 10px;
  font-size: 16px;
}

.list input[type="text"]:focus {
  outline: none;
}

.list button {
  border: none;
  background-color: #d9534f;
  color: white;
  padding: 5px;
  font-weight: bold;
  font-size: 12px;
  cursor: pointer;
}

.lista {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
</style>

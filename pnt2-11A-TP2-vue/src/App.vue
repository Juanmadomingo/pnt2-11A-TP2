<script setup>
import { ref, computed } from 'vue';

const people = [
  { name: "Daniel Sanchez", dni: 20442873 },
  { name: "Juan Perez", dni: 12345678 },
  { name: "Ana Suarez", dni: 87654321 },
  { name: "Juanita Lopez", dni: 11223344 },
  { name: "Carlos Gomez", dni: 44556677 }
];

const nameFilter = ref('');
const dniFilter = ref('');

const filteredPeople = computed(() => {
  console.log("Filtered People:", filteredPeople.value); 
  return people.filter((person) => {
    const matchesName = person.name.toLowerCase().includes(nameFilter.value.toLowerCase());
    const matchesDni = person.dni.toString().includes(dniFilter.value);
    return matchesName && matchesDni;
  });
});

const showAlert = computed(() => {
  console.log("Show Alert:", showAlert.value); 
  return (nameFilter.value.length < 3 || dniFilter.value.length < 3);
});
</script>

<template>
  <div>
    <input
      v-model="nameFilter"
      type="text"
      placeholder="Buscar por nombre"
      :disabled="dniFilter.length > 0 && nameFilter.length < 3"
    />
    <input
      v-model="dniFilter"
      type="text"
      placeholder="Buscar por DNI"
      :disabled="nameFilter.length > 0 && dniFilter.length < 3"
    />

    <div v-if="showAlert" class="alert alert-warning">
      Por favor, ingresa al menos 3 caracteres en uno de los filtros.
    </div>


    <ul v-if="filteredPeople.length > 0">
      <li v-for="(person, index) in filteredPeople" :key="index">
        {{ person.name }} - DNI: {{ person.dni }}
      </li>
    </ul>

    <div v-else>
      <p>No se encontraron resultados que coincidan.</p>
    </div>
  </div>
</template>

<style scoped>
.alert {
  margin-top: 20px;
}
</style>

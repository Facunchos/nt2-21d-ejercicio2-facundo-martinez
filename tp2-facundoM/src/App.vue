<script setup>
import { ref, computed } from 'vue'

const criterioDeBusqueda = ref('')

// Dos filtros separados
const filtroNombre = ref('')
const filtroDoc = ref('')

// Las personas del index.html
const personas = ref([
  {
    nombre: "Daniel",
    apellido: "Sanchez", 
    correo: "danielsanchez68@hotmail.com",
    dni: "20442873"
  },
  {
    nombre: "Juan",
    apellido: "Perez",
    correo: "j@p.gmail.com", 
    dni: "12345678"
  },
  {
    nombre: "Ana",
    apellido: "Suarez",
    correo: "a@s.gmail.com",
    dni: "87654321"
  },
  {
    nombre: "Facundo",
    apellido: "Martinez",
    correo: "facundoezequielmartinez@gmail.com",
    dni: "40900390"
  }
])


// Filtrado excluyente en tiempo real
const personasFiltradas = computed(() => {
  return personas.value.filter((persona) => {
    let pasaFiltroNombre = true
    let pasafiltroDoc = true
    
    // Filtro por nombre y o apellido
    if (filtroNombre.value.trim() !== '') {
      const nombreCompleto = `${persona.nombre} ${persona.apellido}`.toLowerCase()
      pasaFiltroNombre = nombreCompleto.includes(filtroNombre.value.toLowerCase())
    }
    
    // Filtro por documento
    if (filtroDoc.value.trim() !== '') {
      pasafiltroDoc = persona.dni.includes(filtroDoc.value)
    }
    
    // Resultado excluyente (ambos filtros deben pasar)
    return pasaFiltroNombre && pasafiltroDoc
  })
})

// Función para nombre completo
const getNombre = (persona) => {
  return `${persona.nombre} ${persona.apellido}`
}

// Advertencia que dispara la alerta por debajo de 3 caracteres
const mostrarAdvertencia = computed(() => {
  const nombreTieneMenosDe3 = filtroNombre.value.length > 0 && filtroNombre.value.length < 3
  const docTieneMenosDe3 = filtroDoc.value.length > 0 && filtroDoc.value.length < 3
  
  return nombreTieneMenosDe3 || docTieneMenosDe3
})

</script>

<template>
  <div class="container-fluid mt-3">

    <!-- //filtros -->
    <div class="row mb-3">
      <div class="col-md-6">
        <input 
          type="text" 
          class="form-control" 
          v-model="filtroNombre"
          placeholder="Buscar por nombre o apellido"
        />
      </div>

      <div class="col-md-6">
        <input 
          type="text" 
          class="form-control" 
          v-model="filtroDoc"
          placeholder="Buscar por documento"
        />
      </div>
    </div>

    <!-- la alerta -->
    <div v-if="mostrarAdvertencia" class="alert alert-warning" role="alert">
      No se cumplen los criterios minimos para la busqueda
    </div>
    
    <div class="row">
      <div class="col-md-4 mb-3" v-for="persona in personasFiltradas" :key="persona.dni">
        <div class="card">
          <div class="card-body">
            <h5 class="card-title">{{ getNombre(persona) }}</h5>
            <p class="card-text">DNI: {{ persona.dni }}</p>
            <a :href="`mailto:${persona.correo}`" class="card-link">{{ persona.correo }}</a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
</style>
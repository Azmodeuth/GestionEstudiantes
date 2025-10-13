<template>
  <div class="estudiantes-container">
    <h1>Gestión de Estudiantes</h1>
    
    <!-- Sección para agregar estudiante -->
    <div class="seccion-agregar">
      <h2>Agregar Estudiante</h2>
      <form @submit.prevent="agregarEstudiante" class="formulario">
        <div class="campo">
          <label for="nombre">Nombre:</label>
          <input 
            type="text" 
            id="nombre" 
            v-model="nuevoEstudiante.nombre" 
            required
            placeholder="Ingrese el nombre"
          />
        </div>
        
        <div class="campo">
          <label for="apellidos">Apellidos:</label>
          <input 
            type="text" 
            id="apellidos" 
            v-model="nuevoEstudiante.apellidos" 
            required
            placeholder="Ingrese los apellidos"
          />
        </div>
        
        <div class="campo">
          <label for="ci">CI:</label>
          <input 
            type="text" 
            id="ci" 
            v-model="nuevoEstudiante.ci" 
            required
            placeholder="Ingrese el carnet de identidad"
          />
        </div>
        
        <div class="campo-checkbox">
          <input 
            type="checkbox" 
            id="nuevoIngreso" 
            v-model="nuevoEstudiante.nuevoIngreso"
          />
          <label for="nuevoIngreso">Nuevo Ingreso</label>
        </div>
        
        <button type="submit" class="btn-agregar">Agregar Estudiante</button>
      </form>
    </div>
    
    <!-- Sección para visualizar lista de estudiantes -->
    <div class="seccion-lista">
      <h2>Lista de Estudiantes</h2>
      <div v-if="estudiantes.length === 0" class="mensaje-vacio">
        No hay estudiantes registrados
      </div>
      <table v-else class="tabla-estudiantes">
        <thead>
          <tr>
            <th>Nombre</th>
            <th>Apellidos</th>
            <th>CI</th>
            <th>Nuevo Ingreso</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(estudiante, index) in estudiantes" :key="index">
            <td>{{ estudiante.nombre }}</td>
            <td>{{ estudiante.apellidos }}</td>
            <td>{{ estudiante.ci }}</td>
            <td>{{ estudiante.nuevoIngreso ? 'Sí' : 'No' }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: 'EstudiantesComponent',
  data() {
    return {
      // Objeto para almacenar los datos del nuevo estudiante
      nuevoEstudiante: {
        nombre: '',
        apellidos: '',
        ci: '',
        nuevoIngreso: false
      },
      // Array para almacenar la lista de estudiantes
      estudiantes: []
    };
  },
  methods: {
    agregarEstudiante() {
      // Crear una copia del nuevo estudiante
      const estudiante = {
        nombre: this.nuevoEstudiante.nombre,
        apellidos: this.nuevoEstudiante.apellidos,
        ci: this.nuevoEstudiante.ci,
        nuevoIngreso: this.nuevoEstudiante.nuevoIngreso
      };
      
      // Agregar el estudiante a la lista
      this.estudiantes.push(estudiante);
      
      // Limpiar el formulario
      this.nuevoEstudiante = {
        nombre: '',
        apellidos: '',
        ci: '',
        nuevoIngreso: false
      };
    }
  }
};
</script>

<style scoped>
.estudiantes-container {
  max-width: 900px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}

h1 {
  text-align: center;
  color: #333;
  margin-bottom: 30px;
}

h2 {
  color: #555;
  margin-bottom: 15px;
  border-bottom: 2px solid #4CAF50;
  padding-bottom: 5px;
}

.seccion-agregar {
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 8px;
  margin-bottom: 30px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.formulario {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.campo {
  display: flex;
  flex-direction: column;
}

.campo label {
  font-weight: bold;
  margin-bottom: 5px;
  color: #555;
}

.campo input {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
}

.campo input:focus {
  outline: none;
  border-color: #4CAF50;
  box-shadow: 0 0 5px rgba(76, 175, 80, 0.3);
}

.campo-checkbox {
  display: flex;
  align-items: center;
  gap: 10px;
}

.campo-checkbox input[type="checkbox"] {
  width: 18px;
  height: 18px;
  cursor: pointer;
}

.campo-checkbox label {
  font-weight: bold;
  color: #555;
  cursor: pointer;
}

.btn-agregar {
  background-color: #4CAF50;
  color: white;
  padding: 12px 20px;
  border: none;
  border-radius: 4px;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.3s;
}

.btn-agregar:hover {
  background-color: #45a049;
}

.seccion-lista {
  background-color: #fff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.mensaje-vacio {
  text-align: center;
  color: #999;
  padding: 20px;
  font-style: italic;
}

.tabla-estudiantes {
  width: 100%;
  border-collapse: collapse;
  margin-top: 10px;
}

.tabla-estudiantes thead {
  background-color: #4CAF50;
  color: white;
}

.tabla-estudiantes th,
.tabla-estudiantes td {
  padding: 12px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

.tabla-estudiantes th {
  font-weight: bold;
  text-transform: uppercase;
  font-size: 14px;
}

.tabla-estudiantes tbody tr:hover {
  background-color: #f5f5f5;
}

.tabla-estudiantes tbody tr:last-child td {
  border-bottom: none;
}
</style>


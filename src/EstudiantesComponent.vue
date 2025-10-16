<template>
  <div class="estudiantes-container">
    <h1>Gestión de Estudiantes</h1>

    <div class="seccion-agregar">
      <h2>Agregar Estudiante</h2>
      
      <div v-if="mensajeExito" class="mensaje exito">{{ mensajeExito }}</div>

      <form @submit.prevent="agregarEstudiante" class="formulario" novalidate>
        <div class="campo">
          <label for="nombre">Nombre:</label>
          <input 
            type="text" 
            id="nombre"
            placeholder="Ingrese el nombre"
            :value="nuevoEstudiante.nombre"
            @input="filtrarLetras($event, 'nombre')"
            :class="{ 'input-error': errors.nombre }"
            :aria-invalid="errors.nombre ? 'true' : 'false'"
          />
          <p v-if="errors.nombre" class="error-text">{{ errors.nombre }}</p>
        </div>
        
        <div class="campo">
          <label for="apellidos">Apellidos:</label>
          <input 
            type="text" 
            id="apellidos" 
            placeholder="Ingrese los apellidos"
            :value="nuevoEstudiante.apellidos"
            @input="filtrarLetras($event, 'apellidos')"
            :class="{ 'input-error': errors.apellidos }"
            :aria-invalid="errors.apellidos ? 'true' : 'false'"
          />
          <p v-if="errors.apellidos" class="error-text">{{ errors.apellidos }}</p>
        </div>
        
        <div class="campo">
          <label for="ci">CI:</label>
          <input 
            type="text" 
            id="ci"
            placeholder="Ingrese el carnet de identidad"
            :value="nuevoEstudiante.ci"
            @input="filtrarNumerosCI"
            :class="{ 'input-error': errors.ci }"
            :aria-invalid="errors.ci ? 'true' : 'false'"
          />
          <p v-if="errors.ci" class="error-text">{{ errors.ci }}</p>
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
    
    <div class="seccion-lista">
      <h2>Lista de Estudiantes</h2>
      <div v-if="estudiantes.length === 0" class="mensaje-vacio">
        Aún no hay estudiantes registrados.
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
          <tr v-for="estudiante in estudiantes" :key="estudiante.ci">
            <td data-label="Nombre">{{ estudiante.nombre }}</td>
            <td data-label="Apellidos">{{ estudiante.apellidos }}</td>
            <td data-label="CI">{{ estudiante.ci }}</td>
            <td data-label="Nuevo Ingreso">{{ estudiante.nuevoIngreso ? 'Sí' : 'No' }}</td>
          </tr>
        </tbody>
      </table>
    </div>

    <footer class="pie-pagina">
      <p>Aplicación hecha por Robert Dorantes García.</p>
    </footer>
    
  </div>
</template>

<script>
export default {
  name: 'EstudiantesComponent',
  data() {
    return {
      nuevoEstudiante: {
        nombre: '',
        apellidos: '',
        ci: '',
        nuevoIngreso: false
      },
      estudiantes: [],
      errors: {
        nombre: '',
        apellidos: '',
        ci: ''
      },
      mensajeExito: ''
    };
  },
  methods: {
    filtrarLetras(event, campo) {
      const valor = event.target.value;
      const valorFiltrado = valor.replace(/[^a-zA-Z\s]/g, '');
      this.nuevoEstudiante[campo] = valorFiltrado;
      event.target.value = valorFiltrado;
    },

    filtrarNumerosCI(event) {
      const valor = event.target.value;
      const valorFiltrado = valor.replace(/[^\d]/g, '').slice(0, 11);
      this.nuevoEstudiante.ci = valorFiltrado;
      event.target.value = valorFiltrado;
    },

    validarFormulario() {
      this.errors = { nombre: '', apellidos: '', ci: '' }; 
      let esValido = true;

      const nombreTrimmed = this.nuevoEstudiante.nombre.trim();
      if (!nombreTrimmed) {
        this.errors.nombre = 'El nombre es obligatorio.';
        esValido = false;
      } else if (!/^[a-zA-Z\s]*$/.test(nombreTrimmed)) {
        this.errors.nombre = 'El nombre no debe contener números ni caracteres especiales.';
        esValido = false;
      }

      const apellidosTrimmed = this.nuevoEstudiante.apellidos.trim();
      if (!apellidosTrimmed) {
        this.errors.apellidos = 'Los apellidos son obligatorios.';
        esValido = false;
      } else if (!/^[a-zA-Z\s]*$/.test(apellidosTrimmed)) {
        this.errors.apellidos = 'Los apellidos no deben contener números ni caracteres especiales.';
        esValido = false;
      }

      const ciTrimmed = this.nuevoEstudiante.ci.trim();
      if (!ciTrimmed) {
        this.errors.ci = 'El CI es obligatorio.';
        esValido = false;
      } else if (!/^\d+$/.test(ciTrimmed)) {
        this.errors.ci = 'El CI solo debe contener números.';
        esValido = false;
      } else if (ciTrimmed.length !== 11) {
        this.errors.ci = 'El CI debe tener exactamente 11 dígitos.';
        esValido = false;
      } else if (this.estudiantes.some(est => est.ci === ciTrimmed)) {
        this.errors.ci = 'Este CI ya ha sido registrado.';
        esValido = false;
      }

      return esValido;
    },

    agregarEstudiante() {
      this.mensajeExito = '';

      if (this.validarFormulario()) {
        this.estudiantes.push({
          nombre: this.nuevoEstudiante.nombre.trim(),
          apellidos: this.nuevoEstudiante.apellidos.trim(),
          ci: this.nuevoEstudiante.ci.trim(),
          nuevoIngreso: this.nuevoEstudiante.nuevoIngreso
        });
        
        this.nuevoEstudiante = { nombre: '', apellidos: '', ci: '', nuevoIngreso: false };
        
        this.mensajeExito = '¡Estudiante agregado exitosamente!';

        setTimeout(() => {
          this.mensajeExito = '';
        }, 3000);
      }
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
  background-color: #ffffff;
  padding: 25px;
  border-radius: 8px;
  margin-bottom: 30px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.formulario {
  display: flex;
  flex-direction: column;
  gap: 18px;
}

.campo {
  display: flex;
  flex-direction: column;
}

.campo label {
  font-weight: bold;
  margin-bottom: 6px;
  color: #555;
}

.campo input {
  padding: 12px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 16px;
  transition: border-color 0.3s, box-shadow 0.3s;
}

.campo input:focus {
  outline: none;
  border-color: #4CAF50;
  box-shadow: 0 0 8px rgba(76, 175, 80, 0.3);
}

.input-error {
  border-color: #e74c3c;
}

.input-error:focus {
  border-color: #c0392b;
  box-shadow: 0 0 8px rgba(231, 76, 60, 0.3);
}

.error-text {
  color: #e74c3c;
  font-size: 13px;
  margin-top: 5px;
  font-weight: 500;
}

.mensaje {
  padding: 15px;
  margin-bottom: 20px;
  border-radius: 4px;
  font-weight: bold;
  text-align: center;
}

.mensaje.exito {
  background-color: #d4edda;
  color: #155724;
  border: 1px solid #c3e6cb;
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
  margin-bottom: 0;
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
  transition: background-color 0.3s, transform 0.2s;
}

.btn-agregar:hover {
  background-color: #45a049;
  transform: translateY(-2px);
}

.seccion-lista {
  background-color: #fff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  overflow-x: auto;
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
  padding: 15px;
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

.pie-pagina {
  text-align: center;
  margin-top: 40px;
  padding-top: 20px;
  border-top: 1px solid #eee;
  color: #777;
  font-size: 14px;
}

@media (max-width: 768px) {
  .estudiantes-container {
    padding: 10px;
  }
  
  h1 {
    font-size: 24px;
  }

  .tabla-estudiantes thead {
    display: none;
  }

  .tabla-estudiantes, .tabla-estudiantes tbody, .tabla-estudiantes tr, .tabla-estudiantes td {
    display: block;
    width: 100%;
  }

  .tabla-estudiantes tr {
    margin-bottom: 15px;
    border: 1px solid #ddd;
    border-radius: 5px;
    box-shadow: 0 2px 2px rgba(0,0,0,0.05);
  }

  .tabla-estudiantes td {
    text-align: right;
    padding-left: 50%;
    position: relative;
    border-bottom: 1px solid #eee;
  }

  .tabla-estudiantes td:last-child {
    border-bottom: none;
  }

  .tabla-estudiantes td::before {
    content: attr(data-label);
    position: absolute;
    left: 15px;
    width: calc(50% - 30px);
    text-align: left;
    font-weight: bold;
    text-transform: uppercase;
    font-size: 13px;
    color: #333;
  }
}
</style>
<template lang="html">
  <div class="jumbotron">
    <h2>Formulario ingreso de gastos</h2>

    <vue-form :state="formState" @submit.prevent="enviar()">
      <validate tag="div">
        <label for="nombre">Nombre</label>
        <input class="form-control" type="text" id="nombre" name="nombre" v-model.trim="formData.nombre"
          autocomplete="off" :minlength="nombreMin" :maxlength="nombreMax" no-espacios required />
        <field-messages name="nombre" show="$dirty">
          <div slot="required" class="alert alert-dark mt-1">
            Campo requerido
          </div>
          <div slot="no-espacios" class="alert alert-dark mt-1">
            No se permiten espacios
          </div>
          <div slot="minlength" class="alert alert-dark mt-1">
            Este campo requiere un nombre de minimo {{ nombreMin }} letras
          </div>
          <div v-if="formData.nombre.length == nombreMax" class="alert alert-dark mt-1">
            El nombre debe tener como máximo {{ nombreMax }} caracteres
          </div>
        </field-messages>
      </validate>

      <validate tag="div">
        <label for="descripcion">Descripcion</label>
        <input class="form-control" type="text" id="descripcion" name="descripcion" v-model.trim="formData.descripcion"
          autocomplete="off" required />
        <field-messages name="descripcion" show="$dirty">
          <div slot="required" class="alert alert-dark mt-1">
            Campo requerido
          </div>
        </field-messages>
      </validate>

      <validate tag="div">
        <label for="importe">Importe</label>
        <input class="form-control" type="number" id="importe" name="importe" v-model.number="formData.importe"
          autocomplete="off" required />
        <field-messages name="importe" show="$dirty">
          <div slot="required" class="alert alert-dark mt-1">
            Campo requerido
          </div>
        </field-messages>
      </validate>

      <button class="btn btn-success my-3" :disabled="formState.$invalid" type="submit">
        Enviar
      </button>
    </vue-form>
    <br>
    <br>
    <br>
    <h1>
      <b>DETALLE DE GASTOS</b>

    </h1>

    <label for="presupuesto"><b><i>Presupuesto</i></b></label>
    <input class="form-control" type="text" id="presupuesto" name="presupuesto" no-espacios  v-model.number="formData.presupuesto" placeholder="Ingrese su presupuesto"/>
    <br>
    <br>

    <div v-if="importes.length" class="table-responsive">
      <table class="table table-hover table-dark">
        <tr>
          <th>Persona</th>
          <th>Descripcion</th>
          <th>importe</th>
          <th>Fecha</th>
        </tr>
        <tr v-for="(pers, i) in importes" :key="i">
          <td>{{ pers.nombre }}</td>
          <td>
            {{ pers.descripcion }}
          </td>
          <td>
            ${{ pers.importe }}
          </td>
          <td>
            {{ pers.fecha }}
          </td>
        </tr>
        <tr>

          <td colspan="2">
            <center><b>MONTO TOTAL</b></center>
          </td>

          <td v-if="acumimportes < 1000" style="color:green">

            <b>${{ acumimportes }}</b>
          </td>
          <td v-else-if="acumimportes >= 1000 && acumimportes <= 5000" style="color:blueviolet">

            <b>${{ acumimportes }}</b>
          </td>
          <td v-else style="color:orange">

            <b>${{ acumimportes }}</b>
          </td>
        </tr>

      </table>
    </div>
    <h3 class="alert alert-danger" v-else>¡No se encontraron importes!</h3>
  </div>
</template>

<script lang="js">

export default {
  name: 'src-components-formulario-importe',
  components: {},
  props: [],
  data() {
    return {
      formData: this.getFormInicial(),
      formState: {},
      nombreMin: 3,
      nombreMax: 15,
      importes: [],
      acumimportes: 0,
    }
  },
  computed: {},
  mounted() { },
  methods: {
    getFormInicial() {
      return {
        nombre: '',
        descripcion: '',
        importe: '',
        presupuesto:''
      }
    },
    enviar() {
      let note = this.formData
      note.fecha = new Date().toLocaleString()
      this.importes.push(note)
      this.acumimportes += note.importe
      this.formData = this.getFormInicial()
      this.formState._reset()
    }
  }
}
</script>

<style scoped lang="css">
.jumbotron {
  background-color: rgb(0, 0, 0);
  color: white;
}

hr {
  background-color: #eee;
}

h1 {
  color: #eee;
}
</style>

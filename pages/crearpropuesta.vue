<template>
  <div>
    <v-card>
      <v-form ref="formularioPropuesta">
        <v-card-title primary-title> Propuesta </v-card-title>
        <v-card-text>
          <v-text-field
            label="ID" color="white"
            v-model="id"
            :rules="requiredRule"
          ></v-text-field>
          <v-text-field
            label="Descripción" color="white"
            v-model="descripcion"
            :rules="requiredRule"
          ></v-text-field>
        </v-card-text>
        <v-card-actions>
          <v-btn color="success" @click="createPropuesta()">Postular Propuesta</v-btn>
        </v-card-actions>
      </v-form>
    </v-card>
  </div>
</template>
<script>
import config from "../assets/js/config";
export default {
  data() {
    return{
        id: null,
        descripcion: null,
        requiredRule: [(v) => !!v || "El campo es obligatorio"],
    };
  },
  methods: {
      async createPropuesta() {
        try{
            if (!this.$refs.formularioPropuesta.validate()) {
                return;
            }
            let url = config.URL_API + "/postulaciones";
        let payload = {};
        payload.id = parseInt(this.id);
        payload.id_usuario = localStorage.getItem("id");
        payload.id_reto = localStorage.getItem("id_reto");;
        payload.descripcion = this.descripcion;
        console.log(this.descripcion);
        let response = await this.$axios.post(url, payload)
        let data = response.data
        if (data.ok == false) {
          this.$swal({
            type: "error",
            icon: "error",
            title: "Oops...",
            text: data.message,
          })
        }else{
             console.log(data.info);
            this.$swal({
            type: "success",
            icon: "success",
            title: "Propuesta Creada",
            text: "Propuesta creadoa exitosamente",
          })
        }
        console.log(response);
        }catch (error){
            this.$swal({
            type: "error",
            icon: "error",
            title: "Oops...",
            text: "Ha ocurrido un error. No se ha podido conectar a API.",
        });
        console.log(error);
        }
      }
  },
};
</script>
<style scoped>
.theme--dark.v-card {
  background-color: #0077B6;
}
.theme--dark.success {
  background-color: #90E0EF !important;
  border-color: #90E0EF !important;
}
</style>
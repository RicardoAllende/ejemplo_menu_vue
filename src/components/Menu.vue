<template>
  <div class="">
    <div v-if="!loggedIn" class="login-form">
      <div>
        <h2 class="text-center">Log in</h2>
        <div class="form-group">
          <input
            type="text"
            class="form-control"
            placeholder="Username"
            required="required"
          />
        </div>
        <div class="form-group">
          <input
            type="password"
            class="form-control"
            placeholder="Password"
            required="required"
          />
        </div>
        <div class="clearfix">
          <label class="float-left form-check-label"
            ><input type="checkbox" /> Remember me</label
          >
          <a href="#" class="float-right">Forgot Password?</a>
        </div>
      </div>
      <p class="text-center"><a href="#">Create an Account</a></p>
    </div>
    <div v-else>
      <vue-file-toolbar-menu :content="menuInformation" />
      <div v-if="!showGroups">
        <img
          src="https://www.sicrea.com.mx/home/images/Logo-Grupo-SICREA.png"
          class="mt-5 pt-5"
        />
      </div>
      <div v-if="showGroups">
        <h2>Selecciona un grupo de la lista</h2>
        <h4 class="mb-5">Para mostrar los usuarios</h4>
        <div class="row w-100">
          <div class="col-12 col-sm-6 mt-5">
            <select
              class="form-control ml-2"
              v-model="selectedGroup"
              @change="cargarUsuarios()"
            >
              <option v-for="group in groups" :key="group">{{ group }}</option>
            </select>
          </div>
          <div class="col-12 col-sm-6">
            <div v-if="selectedGroup">
              <div
                class="form-row mt-4"
                :key="'user' + user.name"
                v-for="user in users"
              >
                <div class="form-group col-6">
                  <label>Nombre</label>
                  <input
                    type="email"
                    class="form-control"
                    :value="user.name"
                    placeholder="Email"
                  />
                </div>
                <div class="form-group col-6">
                  <label>Ciudad</label>
                  <input
                    type="text"
                    class="form-control"
                    :value="user.city"
                    placeholder="Password"
                  />
                </div>
                <div class="col-12">
                  <b-button
                    @click="show = true"
                    variant="primary"
                    v-b-modal="'modal_edicion_usuario'"
                    >Mostrar info para editar</b-button
                  >
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <b-modal
        id="modal_edicion_usuario"
        size="lg"
        title="Editando información del usuario"
      >
        <div class="container rounded bg-white mt-5">
          <div class="row">
            <div class="col-md-4 border-right">
              <div
                class="d-flex flex-column align-items-center text-center p-3 py-5"
              >
                <img
                  class="rounded-circle mt-5"
                  src="https://i.imgur.com/0eg0aG0.jpg"
                  width="90"
                /><span class="font-weight-bold">John Doe</span
                ><span class="text-black-50">john_doe12@bbb.com</span
                ><span>United States</span>
              </div>
            </div>
            <div class="col-md-8">
              <div class="p-3 py-5">
                <div
                  class="d-flex justify-content-between align-items-center mb-3"
                >
                  <div class="d-flex flex-row align-items-center back">
                    <i class="fa fa-long-arrow-left mr-1 mb-1"></i>
                    <h6>Back to home</h6>
                  </div>
                  <h6 class="text-right">Edit Profile</h6>
                </div>
                <div class="row mt-2">
                  <div class="col-md-6">
                    <input
                      type="text"
                      class="form-control"
                      placeholder="first name"
                      value="John"
                    />
                  </div>
                  <div class="col-md-6">
                    <input
                      type="text"
                      class="form-control"
                      value="Doe"
                      placeholder="Doe"
                    />
                  </div>
                </div>
                <div class="row mt-3">
                  <div class="col-md-6">
                    <input
                      type="text"
                      class="form-control"
                      placeholder="Email"
                      value="john_doe12@bbb.com"
                    />
                  </div>
                  <div class="col-md-6">
                    <input
                      type="text"
                      class="form-control"
                      value="+19685969668"
                      placeholder="Phone number"
                    />
                  </div>
                </div>
                <div class="row mt-3">
                  <div class="col-md-6">
                    <input
                      type="text"
                      class="form-control"
                      placeholder="address"
                      value="D-113, right avenue block, CA,USA"
                    />
                  </div>
                  <div class="col-md-6">
                    <input
                      type="text"
                      class="form-control"
                      value="USA"
                      placeholder="Country"
                    />
                  </div>
                </div>
                <div class="row mt-3">
                  <div class="col-md-6">
                    <input
                      type="text"
                      class="form-control"
                      placeholder="Bank Name"
                      value="Bank of America"
                    />
                  </div>
                  <div class="col-md-6">
                    <input
                      type="text"
                      class="form-control"
                      value="043958409584095"
                      placeholder="Account Number"
                    />
                  </div>
                </div>
                <div class="mt-5 text-right">
                  <button class="btn btn-primary profile-button" type="button">
                    Save Profile
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </b-modal>
    </div>
  </div>
</template>

<script>
import VueFileToolbarMenu from "vue-file-toolbar-menu";
import axios from "axios";

export default {
  components: { VueFileToolbarMenu },

  data() {
    return {
      happy: false,
      menuInformation: [
        {
          text: "USUARIOS",
          menu: [
            {
              text: "AUTOFINAL",
              menu: [
                {
                  text: "ASIGNADOS",
                  menu: [{ text: "Item 1.1.1" }, { text: "Item 1.1.2" }],
                },
                {
                  text: "CAPTURA DIRECTA",
                },
                { text: "HIST. GESTIONES" },
                { text: "HIST. CAMBIO DATOS" },
              ],
            },
            { text: "RENUEVA", click: () => (this.showGroups = true) },
          ],
        },
        {
          text: "IMPRESION",
        },
        {
          text: "FORMATOS",
        },
        {
          text: "REPS. DE OPERACIONES",
        },
        {
          text: "Window",
        },
      ],
      groups: [12, 15, 89, 78, 3125],
      selectedGroup: "",
      users: Array(),
      showGroups: false,
      loggedIn: false,
    };
  },
  methods: {
    login() {
      this.loggedIn = true;
    },
    async cargarUsuarios() {
      try {
        axios.get("https://api.mocki.io/v1/b043df5a").then(({ data }) => {
          console.log(data);
          this.users = data;
          return data;
        });
      } catch (error) {
        console.error(error);
      }
    },
  },
};
</script>

<style>
.login-form {
  width: 340px;
  margin: 50px auto;
  font-size: 15px;
}
.login-form form {
  margin-bottom: 15px;
  background: #f7f7f7;
  box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.3);
  padding: 30px;
}
.login-form h2 {
  margin: 0 0 15px;
}
.form-control,
.btn {
  min-height: 38px;
  border-radius: 2px;
}
.btn {
  font-size: 15px;
  font-weight: bold;
}
</style>
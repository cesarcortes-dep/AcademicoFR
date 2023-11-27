A<template>
  <div class="row mt-2">
    <div class="col-md-4 offset-md-4">
      <div class="card">
        <div class="card-header bg-dark text-white text-center">
          Registrar estudiante
        </div>
        <div class="card-body">
          <form v-on:submit="guardar">
            <div class="d-grid col-6 mx-auto mb-3">
              <img
                v-if="this.foto"
                height="50"
                :src="this.foto"
                id="fotoimg"
                class="img-thumbnail"
                alt=""
              />
              <img
                v-else
                height="100"
                src="https://cdn0.iconfinder.com/data/icons/cryptocurrency-137/128/1_profile_user_avatar_account_person-132-256.png"
                id="fotoimg"
                class="img-thumbnail"
                alt=""
              />
            </div>
            <div class="input-group mb-3">
              <span class="input-group-text"
                ><i class="fa-solid fa-user"></i
              ></span>
              <input
                type="text"
                v-model="nombre"
                id="nombre"
                placeholder="Ingrese un nombre"
                required
                maxlength="50"
                class="form-control"
              />
            </div>
            <div class="input-group mb-3">
              <span class="input-group-text"
                ><i class="fa-solid fa-user"></i
              ></span>
              <input
                type="text"
                v-model="apellido"
                id="apellido"
                placeholder="Ingrese un apellido"
                required
                maxlength="50"
                class="form-control"
              />
            </div>
            <div class="input-group mb-3">
              <span class="input-group-text"
                ><i class="fa-solid fa-gift"></i
              ></span>
              <input
                v-on:change="previsualizarFoto"
                type="file"
                accept="image/png, image/jpeg, img/gif"
                class="form-control"
              />
            </div>
            <div class="input-group mb-3">
              <div class="d-grid col-6 mx-auto mb-3">
                <button class="btn btn-success">
                  <i class="fa-solid fa-floppy-disk"></i>
                  Registrar
                </button>
              </div>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mostrarAlerta, enviarSolicitud } from "../funciones";
export default {
  data() {
    return {
      nombre: "",
      apellido: "",
      foto: '',
      url: "http://127.0.0.1:8000/api/v1/estudiantes",
      cargando: false,
    };
  },
  methods: {
    guardar() {
      event.preventDefault();
      var miFoto = document.getElementById("fotoimg");
      this.foto = miFoto.src;
      if (this.nombre.trim() === "") {
        mostrarAlerta("Ingrese un nombre", "warning", "nombre");
      } else if (this.apellido.trim() === "") {
        mostrarAlerta("Ingrese un apellido", "warning", "apellido");
      } else {
        var parametros = {
          nombre: this.nombre.trim(),
          apellido: this.apellido.trim(),
          foto: this.foto.trim(),
        };
        enviarSolicitud('POST', parametros,this.url,'Estudiante Registrado!')
      }
    },
    previsualizarFoto(event) {
      var reader = new FileReader();
      reader.readAsDataURL(event.target.files[0]);
      reader.onload = function () {
        var miFoto = document.getElementById("fotoimg");
        miFoto.src = reader.result;
        this.foto = miFoto.src;
      };
    },
  },
};
</script>

<template>
  <div class="app">
    <div class="cont_spinner" v-if="spinner">
      <div class="spinner"></div>
    </div>
    <div class="cont_btns">
      <button class="btn" @click.prevent="listarTodos()">listar todos</button>
      <button class="btn" @click.prevent="listarActivos()">
        listar activos
      </button>
      <button class="btn" @click.prevent="listarInactivos()">
        listar inactivos
      </button>
      <button class="btn" @click.prevent="crear()">crear</button>
    </div>
    <div class="cont_nombre_vue">
      <p class="nombre_vue">Fincas</p>
    </div>
    <div class="q-pa-md">
      <q-table :rows="rows" :columns="columns" row-key="name">
        <template v-slot:body-cell-estado="props">
          <q-td :props="props">
            <q-chip
              :label="props.row.estado == 0 ? 'Inactivo' : 'Activo'"
              :color="props.row.estado == 0 ? 'negative' : 'positive'"
              text-color="white"
            />
          </q-td>
        </template>
        <template v-slot:body-cell-acciones="props">
          <q-td :props="props">
            <q-btn
              @click="editar(props.row)"
              color="primary"
              label="Editar"
              :id="props.row.id"
              :style="{ marginRight: '5px' }"
            />
            <q-btn
              v-if="props.row.estado == 0"
              @click="activar(props.row)"
              color="positive"
              :id="props.row.id"
            >
              <svg
                version="1.1"
                viewBox="0 0 2048 2048"
                width="25"
                height="25"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  transform="translate(1837,280)"
                  d="m0 0h16l12 3 13 8 14 13 7 8 7 7 7 8 7 7 7 8 7 7v2h2l7 8 15 16 7 7 7 8 12 13 7 10 5 12 1 5v16l-3 12-6 11-11 13-28 28h-2v2l-7 6-86 86h-2l-2 4h-2l-2 4h-2l-2 4h-2v2h-2v2h-2v2h-2v2h-2v2h-2v2l-8 7-89 89-7 6-5 6-8 7-106 106h-2l-1 3-7 6-5 6-7 6-102 102-8 7-89 89h-2l-2 4h-2l-2 4h-2l-2 4h-2l-2 4h-2v2h-2v2h-2v2h-2v2h-2v2h-2v2l-8 7-85 85-7 6-5 6-7 6-111 111h-2l-1 3-7 6-5 6-7 6-98 98-8 7-93 93h-2l-2 4h-2l-2 4h-2l-2 4h-2l-2 4h-2v2h-2v2h-2v2h-2v2h-2v2h-2v2l-8 7-85 85-7 6-5 6-7 6-79 79-13 9-14 5-16 1-10-2-10-4-8-6-10-9-550-550-7-10-5-12-1-4v-19l3-10 5-10 7-9h2l2-4 94-94 10-7 11-5 9-2h13l13 3 11 6 13 11 22 22 8 7 34 34 2 1v2l4 2 40 40 8 7 32 32 6 5v2l4 2 26 26 8 7 32 32 8 7 36 36 8 7 32 32 6 5v2l4 2 26 26 8 7 28 28 8 7 21 21 3 1 50-50h2v-2h2v-2l8-7 74-74h2l1-3 7-6 102-102h2l2-4h2l2-4h2l2-4h2v-2h2v-2h2v-2h2v-2h2v-2l8-7 79-79h2l2-4h2l1-3 8-7 90-90h2v-2h2v-2h2v-2h2v-2l8-7 74-74h2l1-3 7-6 102-102h2l2-4h2l2-4h2l2-4h2v-2h2v-2h2v-2h2v-2h2v-2l8-7 79-79h2l2-4h2l1-3 8-7 90-90h2v-2h2v-2h2v-2h2v-2l8-7 74-74h2l2-4h2l1-3 7-6 98-98h2l2-4h2l2-4h2l2-4h2l2-4h2v-2h2v-2h2v-2h2v-2h2v-2l8-7 36-36 13-9 10-4z"
                  fill="#FEFEFE"
                />
              </svg>
            </q-btn>
            <q-btn
              v-else
              @click="desactivar(props.row)"
              color="negative"
              :id="props.row.id"
            >
              <svg
                version="1.1"
                viewBox="0 0 2048 2048"
                width="30"
                height="30"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  transform="translate(503,426)"
                  d="m0 0h18l15 3 12 5 13 8 13 11 449 449 4-2 453-453 14-10 12-6 14-4 7-1h18l15 3 12 5 13 8 13 11 8 10 8 13 5 13 3 15v15l-3 16-7 16-7 11-8 10h-2l-2 4-352 352h-2l-2 4h-2l-2 4h-2l-2 4h-2l-2 4h-2l-2 4h-2l-2 4-28 28h-2l-2 4-6 5-6 7-4 4h-2l-2 4-8 8h-2l-2 4-4 2v2h-2v2h-2l3 5 449 449 11 14 6 10 5 13 3 15v14l-3 16-5 13-8 14-9 11h-2l-1 3-13 10-16 8-16 4-7 1h-13l-13-2-10-3-12-6-11-8-457-457-4 1-8 7-5 6-7 6-5 6-7 6-5 6-7 6-5 6-7 6-5 6-6 5-6 7-6 5-6 7-6 5-6 7-6 5-6 7h-2l-2 4h-2l-2 4h-2l-2 4h-2l-2 4h-2l-2 4h-2l-2 4h-2l-2 4h-2l-2 4-272 272h-2l-2 4h-2l-2 4h-2l-2 4h-2l-2 4h-2l-2 4-12 12h-2l-2 4h-2l-1 3-13 10-16 8-16 4-7 1h-13l-13-2-15-5-13-8-12-11-10-11-8-13-6-16-2-11v-18l3-14 5-13 7-12 11-13 450-450-1-4-455-455-10-14-5-11-4-13-1-6v-19l4-18 8-16 10-14 8-8 14-10 12-6 14-4z"
                  fill="#fff"
                />
              </svg>
            </q-btn>
          </q-td>
        </template>
      </q-table>
    </div>
    <div class="cont_form" v-if="formulario">
      <div class="form">
        <svg
          class="cerrarForm"
          @click="cerrarForm()"
          version="1.1"
          viewBox="0 0 2048 2048"
          width="25"
          height="25"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            transform="translate(503,426)"
            d="m0 0h18l15 3 12 5 13 8 13 11 449 449 4-2 453-453 14-10 12-6 14-4 7-1h18l15 3 12 5 13 8 13 11 8 10 8 13 5 13 3 15v15l-3 16-7 16-7 11-8 10h-2l-2 4-352 352h-2l-2 4h-2l-2 4h-2l-2 4h-2l-2 4h-2l-2 4h-2l-2 4-28 28h-2l-2 4-6 5-6 7-4 4h-2l-2 4-8 8h-2l-2 4-4 2v2h-2v2h-2l3 5 449 449 11 14 6 10 5 13 3 15v14l-3 16-5 13-8 14-9 11h-2l-1 3-13 10-16 8-16 4-7 1h-13l-13-2-10-3-12-6-11-8-457-457-4 1-8 7-5 6-7 6-5 6-7 6-5 6-7 6-5 6-7 6-5 6-6 5-6 7-6 5-6 7-6 5-6 7-6 5-6 7h-2l-2 4h-2l-2 4h-2l-2 4h-2l-2 4h-2l-2 4h-2l-2 4h-2l-2 4h-2l-2 4-272 272h-2l-2 4h-2l-2 4h-2l-2 4h-2l-2 4h-2l-2 4-12 12h-2l-2 4h-2l-1 3-13 10-16 8-16 4-7 1h-13l-13-2-15-5-13-8-12-11-10-11-8-13-6-16-2-11v-18l3-14 5-13 7-12 11-13 450-450-1-4-455-455-10-14-5-11-4-13-1-6v-19l4-18 8-16 10-14 8-8 14-10 12-6 14-4z"
            fill="#000"
          />
        </svg>
        <div class="part1">
          <div class="titulo_form">
            <p v-if="variable === 0" class="text_titulo_form">crear</p>
            <p v-else class="text_titulo_form">editar</p>
          </div>
          <div class="cont_inputs">
            <p class="text_inputs">administrador</p>
            <select required v-model="adminOption">
              <option value="" disabled selected hidden></option>
              <option
              v-for="(admin, index) in administradores"
              :key="admin._id"
              :value="index + 1"
              >{{ admin.nombre }}</option>
            </select>
          </div>
          <div class="cont_inputs">
            <p class="text_inputs">nombre</p>
            <input type="text" class="inputs" v-model="nombre">
          </div>
          <div class="cont_inputs">
            <p class="text_inputs">rut</p>
            <input type="text" class="inputs" v-model="rut">
          </div>
          <div class="cont_inputs">
            <p class="text_inputs">direccion</p>
            <input type="text" class="inputs" v-model="direccion">
          </div>
          <div class="cont_inputs">
            <p class="text_inputs">ubicacion geografica</p>
            <input type="text" class="inputs" v-model="ubicacionGeografica">
          </div>
          <div class="cont_inputs">
            <p class="text_inputs">area</p>
            <input type="text" class="inputs" v-model="area">
          </div>
          <div class="cont_inputs">
            <p class="text_inputs">ciudad</p>
            <input type="text" class="inputs" v-model="ciudad">
          </div>
        </div>
        <div class="part2">
          <div class="cont_inputs">
            <p class="text_inputs">documentos</p>
            <input type="text" class="inputs" v-model="documentos">
          </div>
          <div class="cont_inputs">
            <p class="text_inputs">departamento</p>
            <input type="text" class="inputs" v-model="departamento">
          </div>
          <div class="cont_inputs">
            <p class="text_inputs">limite norte</p>
            <input type="text" class="inputs" v-model="limiteNorte">
          </div>
          <div class="cont_inputs">
            <p class="text_inputs">limite sur</p>
            <input type="text" class="inputs" v-model="limiteSur">
          </div>
          <div class="cont_inputs">
            <p class="text_inputs">limite este</p>
            <input type="text" class="inputs" v-model="limiteEste">
          </div>
          <div class="cont_inputs">
            <p class="text_inputs">limite oeste</p>
            <input type="text" class="inputs" v-model="limiteOeste">
          </div>
        </div>
        <div class="cont_btn_form">
          <button
            v-if="variable === 0"
            class="btn_form"
            @click.prevent="enviarCrear()"
          >
            crear
          </button>
          <button v-else class="btn_form" @click.prevent="enviarEditar()">
            editar
          </button>
        </div>
      </div>
    </div>
     <div :class="registroExitoso ? 'success1' : 'success'">
      <div class="success__icon">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="24"
          viewBox="0 0 24 24"
          height="24"
          fill="none"
        >
          <path
            fill-rule="evenodd"
            fill="#393a37"
            d="m12 1c-6.075 0-11 4.925-11 11s4.925 11 11 11 11-4.925 11-11-4.925-11-11-11zm4.768 9.14c.0878-.1004.1546-.21726.1966-.34383.0419-.12657.0581-.26026.0477-.39319-.0105-.13293-.0475-.26242-.1087-.38085-.0613-.11844-.1456-.22342-.2481-.30879-.1024-.08536-.2209-.14938-.3484-.18828s-.2616-.0519-.3942-.03823c-.1327.01366-.2612.05372-.3782.1178-.1169.06409-.2198.15091-.3027.25537l-4.3 5.159-2.225-2.226c-.1886-.1822-.4412-.283-.7034-.2807s-.51301.1075-.69842.2929-.29058.4362-.29285.6984c-.00228.2622.09851.5148.28067.7034l3 3c.0983.0982.2159.1748.3454.2251.1295.0502.2681.0729.4069.0665.1387-.0063.2747-.0414.3991-.1032.1244-.0617.2347-.1487.3236-.2554z"
            clip-rule="evenodd"
          ></path>
        </svg>
      </div>
      <div class="success__title">{{ text }}</div>
      <div class="success__close" @click="cerrar()">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="20"
          viewBox="0 0 20 20"
          height="20"
        >
          <path
            fill="#393a37"
            d="m15.8333 5.34166-1.175-1.175-4.6583 4.65834-4.65833-4.65834-1.175 1.175 4.65833 4.65834-4.65833 4.6583 1.175 1.175 4.65833-4.6583 4.6583 4.6583 1.175-1.175-4.6583-4.6583z"
          ></path>
        </svg>
      </div>
    </div>
    <div :class="registroFallido ? 'error1' : 'error'">
      <div class="error__icon">
        <svg
          fill="none"
          height="24"
          viewBox="0 0 24 24"
          width="24"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="m13 13h-2v-6h2zm0 4h-2v-2h2zm-1-15c-1.3132 0-2.61358.25866-3.82683.7612-1.21326.50255-2.31565 1.23915-3.24424 2.16773-1.87536 1.87537-2.92893 4.41891-2.92893 7.07107 0 2.6522 1.05357 5.1957 2.92893 7.0711.92859.9286 2.03098 1.6651 3.24424 2.1677 1.21325.5025 2.51363.7612 3.82683.7612 2.6522 0 5.1957-1.0536 7.0711-2.9289 1.8753-1.8754 2.9289-4.4189 2.9289-7.0711 0-1.3132-.2587-2.61358-.7612-3.82683-.5026-1.21326-1.2391-2.31565-2.1677-3.24424-.9286-.92858-2.031-1.66518-3.2443-2.16773-1.2132-.50254-2.5136-.7612-3.8268-.7612z"
            fill="#393a37"
          ></path>
        </svg>
      </div>
      <div class="error__title">{{ text }}</div>
      <div class="error__close" @click="cerrar()">
        <svg
          height="20"
          viewBox="0 0 20 20"
          width="20"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="m15.8333 5.34166-1.175-1.175-4.6583 4.65834-4.65833-4.65834-1.175 1.175 4.65833 4.65834-4.65833 4.6583 1.175 1.175 4.65833-4.6583 4.6583 4.6583 1.175-1.175-4.6583-4.6583z"
            fill="#393a37"
          ></path>
        </svg>
      </div>
    </div>
  </div>
</template>
<script setup>
import { onMounted, ref } from "vue";
import { useFincasStore} from "../../stores/fincas.js";
import { useAdministradoresStore } from "../../stores/administradores.js";

let useFincas = useFincasStore();
let useAdministradores = useAdministradoresStore();

let spinner = ref(false);
let registroFallido = ref(false);
let registroExitoso = ref(false);
let text = ref("");

const ocultar = () => {
  setTimeout(() => {
    registroExitoso.value = false;
    registroFallido.value = false;
  }, 3000);
};

const cerrar = () => {
  registroExitoso.value = false;
  registroFallido.value = false;
};
let r = null;
let a = ref([]);

let rows = ref([]);
let columns = ref([
  {
    name: "id_administrador",
    label: "Administrador",
    align: "center",
    field: (row) => {
      let admin = a.value.administradores;
      admin = admin.find((a) => a._id === row.id_administrador);
      return admin.nombre;
      
    },
  },
  {
    name: "nombre",
    label: "Nombre",
    align: "center",
    field: "nombre",
  },
  {
    name: "rut",
    label: "Rut",
    align: "center",
    field: "rut",
  },
  {
    name: "direccion",
    label: "Direccion",
    align: "center",
    field: "direccion",
  },
  {
    name: "ubicacionGeografica",
    label: "Ubicacion Geografica",
    align: "center",
    field: "ubicacionGeografica",
  },
  {
    name: "area",
    label: "Area",
    align: "center",
    field: "area",
  },
  {
    name: "ciudad",
    label: "Ciudad",
    align: "center",
    field: "ciudad",
  },
  {
    name: "documentos",
    label: "Documentos",
    align: "center",
    field: "documentos",
  },
  {
    name: "departamento",
    label: "Departamento",
    align: "center",
    field: "departamento",
  },
  {
    name: "estado",
    label: "Estado",
    align: "center",
    field: "estado",
  },
  {
    name: "acciones",
    label: "Acciones",
    align: "center",
    field: "acciones",
  }
]);

let listarTodos = async () => {
  spinner.value = true;
  r = await useFincas.getFincas();
  a.value = await useAdministradores.getAdministradores();
  rows.value = r.finca;
  spinner.value = false;
};

let listarActivos = async () => {
  spinner.value = true;
  r = await useFincas.getActivos();
  rows.value = r.finca;
  spinner.value = false;
};

let listarInactivos = async () => {
  spinner.value = true;
  r = await useFincas.getInactivos();
  rows.value = r.finca;
  spinner.value = false;
};

let activar = async (row) => {
  spinner.value = true;
  r = await useFincas.putActivar(row._id);
  listarTodos();
  spinner.value = false;
};

let desactivar = async (row) => {
  spinner.value = true;
  r = await useFincas.putDesactivar(row._id);
  listarTodos();
  spinner.value = false;
};

let formulario = ref(false);

let cerrarForm = () => {
  formulario.value = false;
  listarTodos();
  vaciarCampos();
};

let adminOption = ref("");
let nombre = ref("");
let rut = ref("");
let direccion = ref("");
let ubicacionGeografica = ref("");
let limitesPredios = ref("");
let area = ref("");
let ciudad = ref("");
let documentos = ref("");
let departamento = ref("");
let limiteNorte = ref("");
let limiteSur = ref("");
let limiteEste = ref("");
let limiteOeste = ref("");
let estado = ref(1);

let validaciones = () => {
  if(adminOption.value === ""){
    text.value = "El administrador es obligatorio";
    registroFallido.value = true;
    ocultar();
    return false;
  }
  if(nombre.value === "" || nombre.value.trim() === ""){
     text.value = "El nombre es obligatorio";
    registroFallido.value = true;
    ocultar();
    return false;
  }
  if(rut.value === "" || String(rut.value).trim() === ""){
    text.value = "El rut es obligatorio";
    registroFallido.value = true;
    ocultar();
    return false;
  }
  if(direccion.value === "" || direccion.value.trim() === ""){
    text.value = "La dirección es obligatoria";
    registroFallido.value = true;
    ocultar();
    return false;
  }
  if(ubicacionGeografica.value === "" || ubicacionGeografica.value.trim() === ""){
     text.value = "La ubicación geográfica es obligatoria";
    registroFallido.value = true;
    ocultar();
    return false;
  }
  if(area.value === "" || String(area.value).trim() === ""){
    text.value = "El área es obligatorio";
    registroFallido.value = true;
    ocultar();
    return false;
  }
  if (isNaN(area.value)) {
    text.value = "El área debe ser un valor numérico";
    registroFallido.value = true;
    ocultar();
    return false;
  }
  if(ciudad.value === "" || ciudad.value.trim() === ""){
    text.value = "La ciudad es obligatoria";
    registroFallido.value = true;
    ocultar();
    return false;
  }
  if(documentos.value === "" || documentos.value.trim() === ""){
    text.value = "Los documentos son obligatorios";
    registroFallido.value = true;
    ocultar();
    return false;
  }
  if(departamento.value === "" || departamento.value.trim() === ""){
    text.value = "El departamento es obligatorio";
    registroFallido.value = true;
    ocultar();
    return false;
  }
  if(limiteNorte.value === "" || limiteNorte.value.trim() === ""){
    text.value = "El limite norte es obligatorio";
    registroFallido.value = true;
    ocultar();
    return false;
  }
  if(limiteSur.value === "" || limiteSur.value.trim() === ""){
    text.value = "El limite sur es obligatorio";
    registroFallido.value = true;
    ocultar();
    return false;
  }
  if(limiteEste.value === "" || limiteEste.value.trim() === ""){
    text.value = "El limite este es obligatorio";
    registroFallido.value = true;
    ocultar();
    return false;
  }
  if(limiteOeste.value === "" || limiteOeste.value.trim() === ""){
    text.value = "El limite oeste es obligatorio";
    registroFallido.value = true;
    ocultar();
    return false;
  }
}

let vaciarCampos = () => {
  adminOption.value = "";
  nombre.value = "";
  rut.value = "";
  direccion.value = "";
  ubicacionGeografica.value = "";
  area.value = "";
  ciudad.value = "";
  documentos.value = "";
  departamento.value = "";
  limiteNorte.value = "";
  limiteSur.value = "";
  limiteEste.value = "";
  limiteOeste.value = "";
};

let administradores = ref([]);

let variable = ref(0);
let id = ref(null);

let crear = async () => {
  spinner.value = true;
  await useAdministradores.getAdministradores();
  administradores.value = useAdministradores.administradores.administradores;
  variable.value = 0;
  formulario.value = true;
  spinner.value = false;  
}

let editar = async (data) => {
  spinner.value = true;
  await useAdministradores.getAdministradores();
  administradores.value = useAdministradores.administradores.administradores;
  variable.value = 1;
  id.value = data._id;
  adminOption.value = administradores.value.findIndex((a) => a._id === data.id_administrador) + 1;
  nombre.value = data.nombre;
  rut.value = data.rut;
  direccion.value = data.direccion;
  ubicacionGeografica.value = data.ubicacionGeografica;
  area.value = data.area;
  ciudad.value = data.ciudad;
  documentos.value = data.documentos;
  departamento.value = data.departamento;
  limiteNorte.value = data.limites[0].norte;
  limiteSur.value = data.limites[0].sur;
  limiteEste.value = data.limites[0].este;
  limiteOeste.value = data.limites[0].oeste;
  formulario.value = true;
  spinner.value = false;
}

let enviarCrear = async () => {
  if(validaciones() === false){
    return;
  }
  let data = {
    id_administrador: administradores.value[adminOption.value - 1]._id,
    nombre: nombre.value,
    rut: rut.value,
    direccion: direccion.value,
    ubicacionGeografica: ubicacionGeografica.value,
    area: area.value,
    ciudad: ciudad.value,
    documentos: documentos.value,
    departamento: departamento.value,
    limites: [{
      norte: limiteNorte.value,
      sur: limiteSur.value,
      este: limiteEste.value,
      oeste: limiteOeste.value
    }],
    estado: estado.value
  };
  spinner.value = true;
  await useFincas.postFincas(data);
  cerrarForm();
  spinner.value = false;
  text.value = "Registro exitoso";
  registroExitoso.value = true;
  ocultar();
}

let enviarEditar = async () => {
  if(validaciones() === false){
    return;
  }
  let data = {
    id_administrador: administradores.value[adminOption.value - 1]._id,
    nombre: nombre.value,
    rut: rut.value,
    direccion: direccion.value,
    ubicacionGeografica: ubicacionGeografica.value,
    area: area.value,
    ciudad: ciudad.value,
    documentos: documentos.value,
    departamento: departamento.value,
    limites: [{
      norte: limiteNorte.value,
      sur: limiteSur.value,
      este: limiteEste.value,
      oeste: limiteOeste.value
    }],
    estado: estado.value
  };
  spinner.value = true;
  await useFincas.putFincas(id.value, data);
  cerrarForm();
  spinner.value = false;
  text.value = "Modificación exitosa";
  registroExitoso.value = true;
  ocultar();
}

onMounted(() => {
  listarTodos();
});
</script>
<style scoped>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app {
  margin-top: 90px;
  padding-top: 20px;
}

.cont_spinner {
  position: absolute;
  z-index: 1000;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.474);
  display: flex;
  justify-content: center;
  align-items: center;
}

.spinner {
  --size: 30px;
  --first-block-clr: #2e7d32;
  --second-block-clr: #77DD77;
  --clr: #111;
  width: 100px;
  height: 100px;
  position: relative;
}
.spinner::after,
.spinner::before {
  box-sizing: border-box;
  position: absolute;
  content: "";
  width: var(--size);
  height: var(--size);
  top: 50%;
  animation: up 2.4s cubic-bezier(0, 0, 0.24, 1.21) infinite;
  left: 50%;
  background: var(--first-block-clr);
}

.spinner::after {
  background: var(--second-block-clr);
  top: calc(50% - var(--size));
  left: calc(50% - var(--size));
  animation: down 2.4s cubic-bezier(0, 0, 0.24, 1.21) infinite;
}

@keyframes down {
  0%,
  100% {
    transform: none;
  }

  25% {
    transform: translateX(100%);
  }

  50% {
    transform: translateX(100%) translateY(100%);
  }

  75% {
    transform: translateY(100%);
  }
}

@keyframes up {
  0%,
  100% {
    transform: none;
  }

  25% {
    transform: translateX(-100%);
  }

  50% {
    transform: translateX(-100%) translateY(-100%);
  }

  75% {
    transform: translateY(-100%);
  }
}
.success {
  position: absolute;
  z-index: 10000;
  top: -100px;
  left: 50%;
  transform: translateX(-50%);
  font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
    Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  width: 320px;
  padding: 12px;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: start;
  background: #84d65a;
  border-radius: 8px;
  box-shadow: 0px 0px 5px -3px #111;
  transition: all 0.5s;
}

.success1 {
  position: absolute;
  top: 90px;
  left: 50%;
  transform: translateX(-50%);
  font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
    Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  width: 320px;
  padding: 12px;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: start;
  background: #84d65a;
  border-radius: 8px;
  box-shadow: 0px 0px 5px -3px #111;
  transition: all 0.5s;
}

.success__icon {
  width: 20px;
  height: 20px;
  transform: translateY(-2px);
  margin-right: 8px;
}

.success__icon path {
  fill: #393a37;
}

.success__title {
  font-weight: 500;
  font-size: 14px;
  color: #393a37;
}

.success__close {
  width: 20px;
  height: 20px;
  cursor: pointer;
  margin-left: auto;
}

.success__close path {
  fill: #393a37;
}

.error {
  position: absolute;
  top: -100px;
  left: 50%;
  transform: translateX(-50%);
  font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
    Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  width: 320px;
  padding: 12px;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: start;
  background: #fce8db;
  border-radius: 8px;
  box-shadow: 0px 0px 5px -3px #111;
  transition: all 0.5s;
}

.error1 {
  position: absolute;
  top: 90px;
  left: 50%;
  transform: translateX(-50%);
  font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
    Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  width: 320px;
  padding: 12px;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: start;
  background: #fce8db;
  border-radius: 8px;
  box-shadow: 0px 0px 5px -3px #111;
  transition: all 0.5s;
}

.error__icon {
  width: 20px;
  height: 20px;
  transform: translateY(-2px);
  margin-right: 8px;
}

.error__icon path {
  fill: #ef665b;
}

.error__title {
  font-weight: 500;
  font-size: 14px;
  color: #71192f;
}

.error__close {
  width: 20px;
  height: 20px;
  cursor: pointer;
  margin-left: auto;
}

.error__close path {
  fill: #71192f;
}

.cont_btns {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
  gap: 10px;
}

.btn {
  padding: 1.3em 3em;
  font-size: 12px;
  text-transform: uppercase;
  letter-spacing: 2.5px;
  font-weight: 500;
  color: #000;
  background-color: #fff;
  border: none;
  border-radius: 45px;
  box-shadow: 0px 8px 15px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease 0s;
  cursor: pointer;
  outline: none;
}

.btn:hover {
  background-color: #2e7d32;
  box-shadow: 0px 15px 20px #61ca66;
  color: #fff;
  transform: translateY(-7px);
}

.btn:active {
  transform: translateY(-1px);
}

.cont_nombre_vue {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 20px;
  font-size: 20px;
  text-transform: uppercase;
  font-weight: bold;
}

.cont_form {
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.474);
  top: 0;
  left: 0;
}

.form {
  margin-top: 45px;
  width: 55%;
  height: 85%;
  background: #ffffff;
  border-radius: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
}

.part1 {
  width: 50%;
  height: 75%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding-left: 10px;
}

.part2 {
  width: 50%;
  height: 75%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding-right: 10px;
}

.cerrarForm {
  position: absolute;
  z-index: 1000;
  top: 10px;
  right: 10px;
  cursor: pointer;
}

.titulo_form {
  margin-top: 20px;
  position: absolute;
  top: 2%;
  left: 50%;
  transform: translateX(-50%);
}

.text_titulo_form {
  font-size: 20px;
  text-transform: uppercase;
  font-weight: bold;
}

.cont_inputs {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  position: relative;
}

/* Estilo para inputs y select */
.inputs,
select {
  width: 85%;
  border: none;
  outline: none;
  background: none;
  border-bottom: 1px solid #000000;
}

.inputs:focus,
select:focus {
  border-bottom: 2px solid #2e7d32;
}

select {
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
  background-color: transparent;
  background-image: url('data:image/svg+xml;utf8,<svg version="1.1" viewBox="0 0 2048 2048" width="80" height="80" xmlns="http://www.w3.org/2000/svg"><path transform="translate(131,440)" d="m0 0h26l15 2 20 6 12 5 17 9 13 10 14 12 774 774 3 1 779-779 11-9 15-11 22-11 25-7 17-2h23l15 2 21 6 12 5 15 8 11 8 11 9 6 5 9 11 9 12 12 23 9 27h1v55h-2l-7 24-11 23-7 10-9 11-11 12-878 878-10 8-11 8-12 7-16 7-21 6-24 3h-12l-20-2-27-8-23-12-14-11-13-11-880-880-9-11-9-12-10-18-5-13-5-18-2-2v-50l3-9 6-19 5-12 9-16 13-16 9-10 10-8 11-8 18-10 19-7 18-4z" fill="%23FFFFFF"/></svg>');
  background-repeat: no-repeat;
  background-position: right 10px center;
  background-size: 16px;
}

.text_inputs {
  font-size: 14px;
  text-transform: uppercase;
  font-weight: bold;
  position: absolute;
  top: 5%;
  left: 7%;
}

.cont_btn_form {
  margin: 20px 0;
  position: absolute;
  bottom: 2%;
  left: 50%;
  transform: translateX(-50%);
}

.btn_form {
  padding: 14px 25px;
  border: none;
  border-radius: 25px;
  font-size: 14px;
  cursor: pointer;
  text-transform: uppercase;
  transition: all 0.3s ease;
  background-color: #2e7d32;
  color: #ffffff;
}

.btn_form:hover {
  background-color: #589f5c;
}
</style>
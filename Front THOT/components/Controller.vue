<template>
  <div id="controller-container" @keydown="moveWithKeyboard" tabindex="0">
    <div class="row align-items-center">
      <!-- ====================================================== -->
      <!-- Lights Switch Col -->
      <div class="col-12 col-md-6">
        <!-- LIGHTS -->
        <div class="d-flex align-items-center justify-content-center py-3">
          <h5 class="display-4">Luces</h5>
          <label class="switch mt-2 mx-3">
            <input
              type="checkbox"
              v-model="powerLights"
              @change="turnLights($event)"
            />
            <div class="slider round"></div>
            <small v-if="!powerLights" class="text-muted on">apagadas</small>
            <small v-if="powerLights" class="text-muted off">encendidas</small>
          </label>
        </div>
      </div>
      <!-- ====================================================== -->
      <!-- Direction Buttons Col -->
      <div class="col-12 col-md-6">
        <div class="row justify-content-center justify-content-md-start">
          <div class="col-10 py-3">
            <div class="row align-items-center">
              <div class="col-4 text-right">
                <!-- Empty Col1 -->
              </div>
              <div class="col-4 text-center">
                <!-- FORWARD BUTTON Col2 -->
                <button
                  type="button"
                  class="direction-button btn btn-ak"
                  v-b-tooltip.hover.top
                  title="Adelante"
                  v-on:click="move(MOVEMENT_DIRECTION.FORWARD)"
                >
                  <b-icon icon="arrow-up-circle-fill" font-scale="5.5"></b-icon>
                </button>
              </div>
              <div class="col-4">
                <!-- Empty Col3 -->
              </div>
            </div>

            <div class="row">
              <div class="col-4 text-right">
                <!-- LEFT BUTTON Col4 -->
                <button
                  type="button"
                  class="direction-button btn btn-ak"
                  v-b-tooltip.hover.left
                  title="Izquierda"
                  v-on:click="move(MOVEMENT_DIRECTION.LEFT)"
                >
                  <b-icon
                    icon="arrow-left-circle-fill"
                    font-scale="5.5"
                  ></b-icon>
                </button>
              </div>
              <div class="col-4">
                <!-- Empty Col5 -->
              </div>
              <div class="col-4 text-left">
                <!-- RIGHT BUTTON Col6 -->
                <button
                  type="button"
                  class="direction-button btn btn-ak"
                  v-b-tooltip.hover.right
                  title="Derecha"
                  v-on:click="move(MOVEMENT_DIRECTION.RIGHT)"
                >
                  <b-icon
                    icon="arrow-right-circle-fill"
                    font-scale="5.5"
                  ></b-icon>
                </button>
              </div>
            </div>

            <div class="row">
              <div class="col-4">
                <!-- Empty Col7 -->
              </div>
              <div class="col-4 text-center">
                <!-- BACKWARD BUTTON Col8 -->
                <button
                  type="button"
                  class="direction-button btn btn-ak"
                  v-b-tooltip.hover.bottom
                  title="Atrás"
                  v-on:click="move(MOVEMENT_DIRECTION.BACKWARD)"
                >
                  <b-icon
                    icon="arrow-down-circle-fill"
                    font-scale="5.5"
                  ></b-icon>
                </button>
              </div>
              <div class="col-4">
                <!-- Empty Col9 -->
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import {
  MOVEMENT_DIRECTION,
  MOVEMENT_ORIGIN,
  SOCKET_EVENT
} from "../env/constants";

export default {
  name: "ControllerComponent",
  data() {
    return {
      MOVEMENT_DIRECTION: MOVEMENT_DIRECTION,
      powerLights: false
    };
  },
  methods: {
    // --- Move
    move(direction) {
      this.socket.emit(SOCKET_EVENT.CREATE_MOVEMENT, {
        direction: direction,
        origin: MOVEMENT_ORIGIN.WEB
      });
    },
    // --- Move with keyboard
    moveWithKeyboard(event) {
      switch (event.key) {
        case "ArrowUp":
          this.move(MOVEMENT_DIRECTION.FORWARD);
          break;
        case "ArrowDown":
          this.move(MOVEMENT_DIRECTION.BACKWARD);
          break;
        case "ArrowLeft":
          this.move(MOVEMENT_DIRECTION.LEFT);
          break;
        case "ArrowRight":
          this.move(MOVEMENT_DIRECTION.RIGHT);
          break;
      }
    },
    // --- Turn the lights on/off
    turnLights(event) {
      this.socket.emit(SOCKET_EVENT.TURN_LIGHTS, {
        power: event.target.checked
      });

      this.powerLights = event.target.checked;
    }
  },
  mounted() {
    this.socket = this.$nuxtSocket({
      name: "main",
      channel: "/",
      reconnection: false
    });

    this.socket.on(SOCKET_EVENT.TURN_LIGHTS, data => {
      this.powerLights = data.power;
    });
  }
};
</script>

<style>
.direction-button {
  width: 93px;
  height: 93px;
  border-radius: 50%;
  padding: 0;
  transition: all 0.3s;
}

.direction-button:hover {
  -webkit-box-shadow: 0px 5px 4px 0px rgba(0, 0, 0, 0.65) !important;
  box-shadow: 0px 5px 4px 0px rgba(0, 0, 0, 0.65) !important;
}

.display-4 {
  font-size: 40px;
}

/* ======================== SLIDER ======================== */

/* Formateamos el label que servirá de contenedor */
.switch {
  position: relative;
  display: inline-block;
  width: 60px;
  height: 34px;
}

/* Ocultamos el checkbox html */
.switch input {
  display: none;
}

.switch small {
  position: absolute;
  top: 32px;
}

.switch .on {
  margin-left: 2px;
}

.switch .ff {
  margin-left: 3px;
}
/* Formateamos la caja del interruptor sobre la cual se deslizará la perilla de control o slider */
.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  -webkit-transition: 0.4s;
  transition: 0.4s;
}

/* Pintamos la perilla de control o slider usando el selector before */
.slider:before {
  position: absolute;
  content: "";
  height: 26px;
  width: 26px;
  left: 4px;
  bottom: 4px;
  background-color: white;
  -webkit-transition: 0.4s;
  transition: 0.4s;
}

/* Cambiamos el color de fondo cuando el checkbox esta activado */
input:checked + .slider {
  background-color: #f59b1a;
}

/* Deslizamos el slider a la derecha cuando el checkbox esta activado */
input:checked + .slider:before {
  -webkit-transform: translateX(26px);
  -ms-transform: translateX(26px);
  transform: translateX(26px);
}

/* Aplicamos efecto de bordes redondeados en slider y en el fondo del slider */
.slider.round {
  border-radius: 34px;
}

.slider.round:before {
  border-radius: 50%;
}
</style>

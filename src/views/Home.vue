<template>
  <div class="home bg-contact100">
    <div class="container-contact100">
      <div class="wrap-contact100">
        <div class="contact100-pic">
          <img src="~@/assets/images/img-01.png" alt="IMG">
        </div>

        <form @submit.prevent="send" class="contact100-form validate-form">
          <img src="~@/assets/images/logo-verde2.png">
          <span class="contact100-form-title">
            Template E-mail
          </span>

          <span class="title_section">Nome Empresa / Cliente</span>
          <div class="wrap-input100 validate-input">
            <input class="input100" type="text" name="name" placeholder="Nome" v-model="name" required>
            <span class="focus-input100"></span>
            <span class="symbol-input100">
              <i class="fa fa-user" aria-hidden="true"></i>
            </span>
          </div>
          <br>
          <span class="title_section">Central do Assinante</span>
          <div class="wrap-input100 validate-input">
            <input class="input100" type="text" name="address" placeholder="Endereço" v-model="address" required>
            <span class="focus-input100"></span>
            <span class="symbol-input100">
              <i class="fa fa-envelope" aria-hidden="true"></i>
            </span>
          </div>

          <div class="wrap-input100 validate-input">
            <input class="input100" type="text" name="login" placeholder="Login" v-model="login" required>
            <span class="focus-input100"></span>
            <span class="symbol-input100">
              <i class="fa fa-user" aria-hidden="true"></i>
            </span>
          </div>

           <div class="wrap-input100 validate-input">
            <input class="input100" type="text" name="password" placeholder="Senha" v-model="password" required>
            <span class="focus-input100"></span>
            <span class="symbol-input100">
              <i class="fa fa-lock" aria-hidden="true"></i>
            </span>
          </div>
          <br>
          <span class="title_section">A/C do Dep. T.I</span>
          <div class="wrap-input100 validate-input">
            <input class="input100" type="text" name="v_banda" placeholder="Velocidade de Banda" v-model="banda" required>
            <span class="focus-input100"></span>
            <span class="symbol-input100">
              <i class="fa fa-pencil" aria-hidden="true"></i>
            </span>
          </div>

           <div class="wrap-input100 validate-input">
            <input class="input100" type="text" name="ip" placeholder="IP" v-model="ip" required>
            <span class="focus-input100"></span>
            <span class="symbol-input100">
              <i class="fa fa-pencil" aria-hidden="true"></i>
            </span>
          </div>

          <div class="wrap-input100 validate-input">
            <input class="input100" type="text" name="gateway" placeholder="Gateway" v-model="gateway" required>
            <span class="focus-input100"></span>
            <span class="symbol-input100">
              <i class="fa fa-pencil" aria-hidden="true"></i>
            </span>
          </div>

          <div class="wrap-input100 validate-input">
            <input class="input100" type="text" name="dns" placeholder="DNS" v-model="dns" required>
            <span class="focus-input100"></span>
            <span class="symbol-input100">
              <i class="fa fa-pencil" aria-hidden="true"></i>
            </span>
          </div>

          <div class="container-contact100-form-btn">
            <button class="contact100-form-btn">
              ENVIAR
            </button>
          </div>

          <div class="container-contact100-form-btn">
            <button class="contact100-form-btn" type="button" @click="showConfig = true">
              CONFIG SMTP
            </button>
          </div>
        </form>
      </div>
    </div>
    <ConfigModal :visible="showConfig" @close="showConfig = false" />
  </div>
</template>

<script lang="ts">
import swal from 'sweetalert'
import { Options, Vue } from 'vue-class-component'
import ConfigModal from '@/components/ConfigModal.vue'
const { ipcRenderer } = window.require('electron')

@Options({
  components: {
    ConfigModal
  }
})
export default class Home extends Vue {
  name = '';
  address = ''
  login = '';
  password = '';
  banda = '';
  ip = '';
  gateway = '';
  dns = '';
  showConfig = false

  send () {
    swal({
      title: 'Enviando...',
      text: 'Aguarde',
      buttons: {
        cancel: false,
        confirm: false
      },
      closeOnClickOutside: false,
      closeOnEsc: false
    })

    const response = ipcRenderer.sendSync('sendEmail', {
      name: this.name,
      address: this.address,
      login: this.login,
      password: this.password,
      banda: this.banda,
      ip: this.ip,
      gateway: this.gateway,
      dns: this.dns

    })

    if (response === 'missing_transport') {
      swal('Oops!', 'The SMTP configs were not inserted.', 'error')
    } else if (response === 'success') {
      swal('Successo!', 'Email enviado.', 'success')
    } else {
      swal('Erro!', 'Não foi possível enviar.', 'error')
    }
  }
}
</script>

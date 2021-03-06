<template>
  <b-modal
    modal-class="login_modal"
    id="modal-login"
    ref="modal"
    body-class="py-0"
    header-border-variant="primary"
    hide-footer
  >
    <template #modal-title>
      <b>{{ $t('login-row:main-action').toUpperCase() }}</b>
    </template>
    <transition name="fade">
      <b-alert class="mt-3" show variant="danger" v-if="errorMessage">
        {{ errorMessage }}
      </b-alert>
    </transition>
    <b-form @submit.prevent="login" class="login_modal_content">
      <div class="custom-input-group">
        <b-input :placeholder="$t('login-modal:username')" v-model="form.username" required />
        <span class="input-group-append">
          <img src="@/assets/front-page/img/user.svg" />
        </span>
      </div>
      <div class="custom-input-group">
        <b-input
          type="password"
          :placeholder="$t('login-modal:password')"
          v-model="form.password"
          required
        />
        <span class="input-group-append">
          <img src="@/assets/front-page/img/lock.svg" />
        </span>
      </div>

      <p
        class="mt-0"
        @click="$bvModal.hide('modal-login')"
        v-b-modal.modal-reset-password
        style="text-align:end; text-decoration:underline;margin-top:"
      >
        {{ $t('login-sub:main-action') }}
      </p>
      <b-button type="submit" variant="primary" class="submit-button">{{
        $t('login-modal:main-action').toUpperCase()
      }}</b-button>

      <p style="text-align:center">
        {{ $t('login-modal:register-pre-text') }}
        <span
          style="text-decoration:underline"
          @click="$bvModal.hide('modal-login')"
          v-b-modal.modal-register
        >
          {{ $t('login-modal:register-action') }}
        </span>
      </p>
      <v-facebook-login
        @sdk-init="handleSdkInit"
        :app-id="fbID"
        @login="fbLogIn()"
      ></v-facebook-login>
    </b-form>
  </b-modal>
</template>

<script lang="ts">
  import { Component, Prop, Vue } from 'vue-property-decorator';
  import { BModal, BFormInput } from 'bootstrap-vue';
  import VueRecaptcha from 'vue-recaptcha';
  import axios from 'axios';
  // @ts-ignore
  import VFacebookLogin from 'vue-facebook-login-component';

  @Component({
    components: {
      VueRecaptcha,
      VFacebookLogin,
    },
  })
  export default class RegisterModal extends Vue {
    form = {
      username: '',
      password: '',
    };

    errorMessage = '';

    fb = null;

    private fbID = process.env.VUE_APP_FACEBOOK_API_ID;

    // TODO consolidate
    async fbLogIn() {
      this.$bvModal.hide('modal-login');
      const data = await this.$vxm.user.fbLogin(this.fb);
      if (data.success) {
        this.form.username = '';
        this.form.password = '';
        this.$router.push('/');
      } else {
        this.$vxm.user.showLoginFailedModal({ errorMessage: data.error });
      }
    }

    $refs!: {
      modal: BModal;
      rePassword: BFormInput;
    };

    // TODO consolidate
    handleSdkInit({ FB, scope }) {
      console.log('logged in', FB);
      this.FB = FB;
      // this.scope = scope;
    }

    async login() {
      this.$bvModal.hide('modal-login');
      if (this.form.username && this.form.password) {
        const data = await this.$vxm.user.login({
          username: this.form.username,
          password: this.form.password,
        });
        if (data.success) {
          this.$router.push('/');
        } else {
          this.$vxm.user.showLoginFailedModal({ errorMessage: data.error });
        }
      }
    }
  }
</script>

<style scoped lang="scss">
  .submit-button {
    margin-top: 22.5px;
  }

  .fade-enter-active,
  .fade-leave-active {
    transition: opacity 0.15s;
  }
  .fade-enter,
  .fade-leave-to {
    opacity: 0;
  }

  p {
    font-size: 11.25px;
    cursor: pointer;

    &:focus {
      outline: none;
    }
  }

  ::v-deep .modal-header {
    -webkit-backdrop-filter: blur(28.125px);
    backdrop-filter: blur(28.125px);
    background-color: #4a90e2;
    padding: 8.4375px;

    .modal-title {
      font-size: 15px;
      font-weight: bold;
      line-height: 1.38;
      margin: 0 auto;
      padding-left: 28.125px;
    }

    .close {
      opacity: 0.5;
      color: var(--white);
      margin-left: 0;

      &:focus {
        outline: none;
      }

      &:hover {
        opacity: 0.5;
      }
    }
  }

  ::v-deep .modal-dialog {
    max-width: 375px;
    width: 100%;
    height: 100%;
    margin: 0 auto;
    display: flex;
    align-items: center;

    .modal-body {
      padding: 0 32.8125px;
      margin-top: 22.5px;

      .login_modal_content {
        display: flex;
        flex-direction: column;
        margin-bottom: 12.1875px;

        .form-control {
          width: 100%;
          height: 40.3125px;
          margin: 0 auto 13.125px;
          border-radius: 4.6875px;
          opacity: 0.5;
          border: solid 1.0005px var(--white);
          background-color: #000000;
        }

        .custom-input-group {
          position: relative;
        }

        .custom-input-group {
          span {
            position: absolute;
            top: 9.375px;
            right: 11.25px;
          }

          .lock {
            right: 15.9375px;
          }
        }

        .submit-button {
          width: 173.4375px;
          height: 40.3125px;
          margin: 0 auto 15.9375px;
          font-size: 16.875px;
          font-weight: bold;
          line-height: 1.33;
        }

        p {
          font-size: 11.25px;
          cursor: pointer;

          &:focus {
            outline: none;
          }
        }
      }
    }
  }
</style>

<template>
<fragment>
    <h2 class="text-center font-weight-bold text-secondary">Login</h2>
    <validation-observer ref="form">
      <b-form @submit.prevent="handleSubmit">
        <b-form-group
          id="input-group-email"
          label="Email"
          label-for="input-email"
        >
          <validation-provider
            v-slot="{ errors }"
            name="email"
            rules="required"
          >
            <b-form-input
              id="input-email"
              v-model="form.email"
              type="email"
              required
              placeholder="Enter email"
              :class="errors.length ? 'border-danger' : ''"
            >
            </b-form-input>
            <span v-if="errors.length" class="text-danger">
              {{ errors[0] }}
            </span>
          </validation-provider>
        </b-form-group>

        <b-form-group
          id="input-group-password"
          label="Password"
          label-for="input-password"
        >
          <validation-provider
            v-slot="{ errors }"
            name="password"
            type="password"
            rules="required|min:6"
          >
            <b-form-input
              id="input-password"
              v-model="form.password"
              type="password"
              required
              placeholder="Enter password"
              :class="errors.length ? 'border-danger' : ''"
            >
            </b-form-input>
            <span v-if="errors.length" class="text-danger">
              {{ errors[0] }}
            </span>
          </validation-provider>
        </b-form-group>
        <b-button type="submit" block variant="outline-secondary mt-4" :disabled="loading">
          <b-spinner small type="grow" v-if="loading"></b-spinner>
          <b-icon icon="arrow-right-circle" aria-hidden="true" v-else></b-icon>
          Login
        </b-button>
        <div class="text-center mt-2">
          <b-link href="/auth/register" class="text-secondary">Not having an account?</b-link>
        </div>
      </b-form>
    </validation-observer>
  </fragment>

</template>

<script>


import Noty from 'noty';
import { ValidationObserver, ValidationProvider } from 'vee-validate';
// w
export default {
  layout: 'auth',
//   middleware: 'guest',
  components: {
    ValidationObserver,
    ValidationProvider,
  },
  data() {
    return {
      form: {},
      loading: false,
    };
  },
//   computed: {
//     ...mapGetters(['isAuthenticated', 'loggedInUser'])
//   },
  methods: {
    handleSubmit() {
      this.$refs.form.validate()
        .then(async success => {
          if (!success) {
            new Noty({
              text: 'Invild data!',
              type: 'error',
              timeout: 2000
            }).show();
            return false;
          }

          const vm = this;
          this.$set(this, 'loading', true);
          await this.$auth.loginWith('local', { data: this.form })
            .then(({ status }) => {
              if (status === 200) {
                new Noty({
                  text: `Welcome user <b>${this.loggedInUser.first_name} ${this.loggedInUser.last_name}</b>!`,
                  type: 'success',
                  timeout: 2000
                }).show();
              }
            })
            .catch(err => {
              let message = 'Error!';
              if (err.response.data && err.response.data.detail) {
                vm.$refs.form.setErrors(err.response.data.detail);
                message = err.response.data.detail;
              }

              new Noty({
                text: message,
                type: 'error',
                timeout: 2000
              }).show();
            });
          this.$set(this, 'loading', false);
        });
    }
  }
}
</script>

<style scoped>

</style>

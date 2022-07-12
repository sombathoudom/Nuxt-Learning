<template>
    <fragment>
        <h2 class="text-center font-weight-bold text-secondary">Sign Up</h2>
        <validation-observer ref="form">
            <b-form>
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
                    id="input-1"
                    v-model="form.email"
                    type="email"
                    placeholder="Enter email"
                    required
                    >
                    </b-form-input>
                     <span v-if="errors.length" class="text-danger">
                     {{ errors[0] }}
                    </span>
                     </validation-provider>
                </b-form-group>
                <b-form-group
                    id="input-group-phone"
                    label="Phone"
                    label-for="input-phone"
                    >
                    <b-form-input
                    id="input-1"
                    v-model="form.phone"
                    type="text"
                    placeholder="Enter Phone"
                    required
                    ></b-form-input>
                </b-form-group>
                <b-form-group
                    id="input-group-password"
                    label="Password"
                    label-for="input-password"
                    >
                    <b-form-input
                    id="input-1"
                    v-model="form.password"
                    type="text"
                    placeholder="Enter password"
                    required
                    ></b-form-input>
                </b-form-group>
                <b-form-group
                    id="input-group-password"
                    label="Password"
                    label-for="input-password"
                    >
                    <b-form-input
                    id="input-1"
                    v-model="form.password"
                    type="text"
                    placeholder="Enter password"
                    required
                    ></b-form-input>
                </b-form-group>
                <b-button type="submit" block variant="outline-secondary mt-4" :disabled="loading">
                    <b-spinner small type="grow" v-if="loading"></b-spinner>
                    <b-icon icon="arrow-right-circle" aria-hidden="true" v-else></b-icon>
                        Sign-Up
                </b-button>
                <div class="text-center mt-2">
                <b-link href="/auth/login" class="text-secondary">Already have an account?</b-link>
                </div>
            </b-form>   
        </validation-observer>  
    
    </fragment>
</template>

<script>
import { ValidationObserver, ValidationProvider } from 'vee-validate';
export default {
    components:{
        ValidationObserver,
        ValidationProvider
    },
    layout: 'auth',
    data(){
        return {
            form: {},
            loading: false,
        };
    }
}
</script>
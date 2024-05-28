<!-- src/views/HomePage.vue -->
<template>
    <v-container>
      <v-row align="center" justify="center">
        <v-col cols="12" md="6">
          <v-card>
            <v-card-title>Login</v-card-title>
            <v-card-text>
              <v-form v-model="form" @submit.prevent="onSubmit">
                <v-text-field
                  v-model="emailValue"
                  :readonly="loading"
                  :rules="[emailRules]"
                  :error-messages="emailErrors"
                  class="mb-2"
                  label="Email"
                  clearable
                  @blur="v$.emailValue.$touch()"
                ></v-text-field>
                <v-text-field
                  v-model="passwordValue"
                  :readonly="loading"
                  :rules="[passwordRules]"
                  :error-messages="passwordErrors"
                  label="Password"
                  placeholder="Enter your password"
                  clearable
                  :type="showPassword ? 'text' : 'password'"
                  :append-inner-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'"
                  @click:append-inner="showPassword = !showPassword"
                  @blur="v$.passwordValue.$touch()"
                ></v-text-field>
                <v-btn
                  :disabled="!form"
                  :loading="loading"
                  color="success"
                  size="large"
                  type="submit"
                  variant="elevated"
                  block
                >
                  Sign In
                </v-btn>
              </v-form>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </template>
  
  <script>
  import { ref, computed } from 'vue';
  import { required, email, minLength } from '@vuelidate/validators';
  import useVuelidate from '@vuelidate/core';
  import { useRouter } from 'vue-router';
  
  export default {
    setup() {
      const router = useRouter();
      const form = ref(false);
      const emailValue = ref('');
      const passwordValue = ref('');
      const loading = ref(false);
      const showPassword = ref(false);
  
      const rules = computed(() => ({
        emailValue: { required, email },
        passwordValue: { required, minLength: minLength(6) },
      }));
  
      const v$ = useVuelidate(rules, { emailValue, passwordValue });
  
      const onSubmit = () => {
        v$.value.$touch();
        if (!v$.value.$invalid) {
          loading.value = true;
          setTimeout(() => {
            loading.value = false;
            router.push('/game');
          }, 2000);
        }
      };
  
      const emailErrors = computed(() => v$.value.emailValue.$errors.map((error) => error.$message));
      const passwordErrors = computed(() => v$.value.passwordValue.$errors.map((error) => error.$message));
  
      return {
        form,
        emailValue,
        passwordValue,
        loading,
        showPassword,
        emailErrors,
        passwordErrors,
        onSubmit,
        v$,
      };
    },
  };
  </script>
  
  <style>
  .error {
    color: red;
    font-size: 0.875rem;
    margin-top: -0.5rem;
    margin-bottom: 1rem;
  }
  </style>
  
<template>
  <div class="account-page">
    <div class="account-content">
      <div class="login-wrapper forgot-pass-wrap bg-img">
        <div class="login-content">
          <form @submit.prevent="submitForm">
            <div class="login-userset">
              <div class="login-logo logo-normal">
                <img src="../assets/img/login-logo.png" alt="img" />
              </div>
              <router-link to="/dashboard" class="login-logo logo-white">
                <img src="../assets/img/logo-white.png" alt="" />
              </router-link>
              <div class="login-userheading">
                <h3>Forgot password?</h3>
                <h4>
                  If you forgot your password, well, then we'll email you instructions to reset your
                  password.
                </h4>
              </div>
              <div class="form-login">
                <label>Email <span class="required-star">*</span></label>
                <div class="form-addons">
                  <input
                    type="email"
                    class="form-control"
                    v-model="formData.email"
                    :class="{ 'is-invalid': v$.email.$error }"
                    @blur="v$.email.$touch"
                    placeholder="Enter Email"
                  />
                  <img src="../assets/img/icons/mail.svg" alt="img" />
                </div>
                <div v-if="v$.email.$error" class="validation-error">
                  <span>{{ v$.email.$errors[0].$message }}</span>
                </div>
              </div>
              <div class="form-login">
                <button type="submit" class="btn btn-login">Send Reset Link</button>
              </div>
              <div class="signinform text-center">
                <h4>Return to<router-link to="/" class="hover-a"> login </router-link></h4>
              </div>
              <div class="my-4 d-flex justify-content-center align-items-center copyright-text">
                <p>&copy; {{ new Date().getFullYear() }} Shivinfotech</p>
              </div>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup>
import { reactive } from 'vue'
import { useVuelidate } from '@vuelidate/core'
import { required, email, helpers } from '@vuelidate/validators'
import { useRouter } from 'vue-router'
const router = useRouter()
const formData = reactive({ email: '' })
const emailRequired = helpers.withMessage('Email is required.', required)
const emailValid = helpers.withMessage('Please enter a valid email address.', email)
const validationRules = { email: { emailRequired, emailValid } }
const v$ = useVuelidate(validationRules, formData)
function submitForm() {
  v$.value.$touch()
  if (!v$.value.$invalid) {
    router.push('/')
  }
}
</script>
<style scoped>
.validation-error {
  color: #ff0000;
  font-size: 12px;
  margin-top: 5px;
  display: flex;
  flex-direction: column;
  gap: 2px;
}
.validation-error span {
  color: #ff0000;
}
.form-control.is-invalid {
  border-color: #ff0000 !important;
}
.form-control.is-invalid:focus {
  border-color: #ff0000 !important;
  box-shadow: 0 0 0 0.2rem rgba(255, 0, 0, 0.25) !important;
}
.required-star {
  color: #ff0000;
  font-weight: bold;
}
.forgot-link-red {
  color: #ff0000;
  font-weight: 500;
  font-size: 14px;
  text-decoration: underline;
  margin-top: 4px;
  display: inline-block;
}
</style>

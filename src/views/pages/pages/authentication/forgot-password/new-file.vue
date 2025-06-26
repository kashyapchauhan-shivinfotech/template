<template>
  <div class="account-page">
    <div class="account-content">
      <div class="login-wrapper login-new">
        <div class="login-content user-login">
          <div class="login-logo">
            <router-link :to="{ name: ROUTE_NAMES.DASHBOARD }">
              <img src="../../assets/images/logo.png" alt="logo" />
            </router-link>
          </div>
          <form @submit.prevent="handleSubmit">
            <div class="login-userset">
              <div class="login-userheading">
                <h3>{{ labels.resetPassword }}</h3>
                <h4>{{ labels.resetPasswordText }}</h4>
              </div>
              <div class="form-login">
                <BaseInput :label="labels.eMail" :model-value="email" disabled />
              </div>
              <div class="form-login">
                <BaseInput
                  :label="labels.otp"
                  v-model="form.otp"
                  required
                  :error="errorMsg && !form.otp ? errorMsg : ''"
                />
              </div>
              <div class="form-login">
                <BaseInput
                  :label="labels.newPassword"
                  v-model="form.newPassword"
                  type="password"
                  required
                  :error="errorMsg && !form.newPassword ? errorMsg : ''"
                />
              </div>
              <div class="form-login">
                <BaseInput
                  :label="labels.confirmPassword"
                  v-model="form.confirmPassword"
                  type="password"
                  required
                  :error="errorMsg && !form.confirmPassword ? errorMsg : ''"
                />
              </div>
              <div class="form-login">
                <BaseButton type="submit" class="btn btn-login">{{
                  labels.changePassword
                }}</BaseButton>
              </div>
              <div class="signinform text-center">
                <h4>
                  {{ labels.returTo }}
                  <router-link :to="{ name: ROUTE_NAMES.SIGN_IN }" class="hover-a">{{
                    labels.logIn
                  }}</router-link>
                </h4>
              </div>
            </div>
            <div v-if="errorMsg" class="text-red-500 text-sm mb-2">{{ errorMsg }}</div>
          </form>
        </div>
        <div class="my-4 d-flex justify-content-center align-items-center copyright-text">
          <p>{{ labels.copyRighText }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import { labels } from '../../components/Constants/labels'
import { ROUTE_NAMES } from '../../components/Constants/routes'
import { updatePassword } from '../../api/apiService'
import { useToast } from 'vue-toastification'
import BaseInput from '../../components/Base/BaseInput.vue'
import BaseButton from '../../components/Base/BaseButton.vue'
import { allFieldsRequired, passwordsDoNotMatch } from '../../utils/validation'

const route = useRoute()
const router = useRouter()
const toast = useToast()

const email = ref(route.query.email || '')
const form = reactive({
  otp: '',
  newPassword: '',
  confirmPassword: '',
})
const errorMsg = ref('')

const handleSubmit = async () => {
  errorMsg.value = ''
  if (!form.otp || !form.newPassword || !form.confirmPassword) {
    errorMsg.value = allFieldsRequired()
    return
  }
  if (form.newPassword !== form.confirmPassword) {
    errorMsg.value = passwordsDoNotMatch()
    return
  }
  try {
    const response = await updatePassword({
      email: email.value,
      otp: Number(form.otp),
      newPassword: form.newPassword,
      confirmPassword: form.confirmPassword,
    })
    console.log('Backend response:', response)
    toast.success('Password updated successfully!')
    router.push({ name: ROUTE_NAMES.SIGN_IN })
  } catch (err) {
    if (err?.response?.data?.message) {
      errorMsg.value = err.response.data.message
      toast.error(err.response.data.message)
    } else {
      errorMsg.value = 'Failed to update password. Please check your OTP and try again.'
      toast.error(errorMsg.value)
    }
  }
}
</script>
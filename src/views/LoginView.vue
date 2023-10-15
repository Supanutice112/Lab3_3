<script setup lang="ts">
  import InputText from '@/components/InputText.vue'
  import * as yup from 'yup'
  import { useAuthStore } from '@/stores/auth';
  import { useRouter } from 'vue-router';
  import { useMessageStore } from '@/stores/message';
  const messageStore = useMessageStore()
  const router = useRouter()
  const authStore = useAuthStore()
  // import { ref } form 'yup'
  import { useField,useForm} from 'vee-validate'
  const validationSchema = yup.object({
    username: yup.string() .required('The Username is required'),
    password: yup.string() .required('The Password is required')
    
  })
  const { errors,handleSubmit } = useForm({
    validationSchema,
    initialValues: {
      username: '',
      password: ''
    }
  })
  const { value: username } = useField<string>('username')
  const { value: password } = useField<string>('password')
  const onSubmit = handleSubmit((values) => {
    authStore.login(values.username, values.password)
    .then(()=> {
      router.push({ name: 'event-list'})
    }).catch((err) => {
      messageStore.updateMessage('could not login')
      setTimeout(() => {
        messageStore.resetMessage()
      },3000)
    })
  })
  </script>
<template>

<div class="flex min-h-full flex-col justify-center px-6 py-12 lg:px-8">
    <div class="sm:mx-auto sm:w-full sm:max-w-sm">
      <img class="mx-auto h-10 w-auto" src="https://tailwindui.com/img/logos/mark.svg?color=indigo&shade=600" alt="Your Company">
      <h2 class="mt-10 text-center text-2xl font-bold leading-9 tracking-tight text-gray-900">Sign in to your account</h2>
    </div>
  
    <div class="mt-10 sm:mx-auto sm:w-full sm:max-w-sm">
      <form class="space-y-6" @submit.prevent= "onSubmit" >
        <div>
          <label for="username" class="block text-sm font-medium leading-6 text-gray-900">Username address</label>
          <InputText type="text" v-model="username" :error="errors['username']"></InputText>
        </div>
        
        <div>
          <div class="flex items-center justify-between">
            <label for="password" class="block text-sm font-medium leading-6 text-gray-900">Password</label>
            <div class="text-sm">
              <a href="#" class="font-semibold text-indigo-600 hover:text-indigo-500">Forgot password?</a>
            </div>
          </div>
          <InputText  v-model="password" type="password" :error="errors['password']"></InputText>
        </div>
        
        <div>
          <button type="submit" class="w-full bg-indigo-600 px-3 py-1.5 text-sm font-semibold leading-6 text-white shadow-sm hover:bg-indigo-500 focus:outline-none focus:ring focus:ring-indigo-600">Sign in</button>
        </div>
        
      </form>
      <p class="mt-10 text-center text-sm text-gray-500">
        Not a member?
        <a href="#" class="font-semibold leading-6 text-indigo-600 hover:text-indigo-500">Start a 14 day free trial</a>
      </p>
      
  </div>
</div>

  </template>
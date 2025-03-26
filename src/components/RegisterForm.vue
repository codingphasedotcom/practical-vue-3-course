<template>
  <div class="min-h-screen bg-gray-50 flex items-center justify-center p-4">
    <div class="max-w-md w-full bg-white rounded-xl shadow-lg p-8">
      <h2 class="text-2xl font-bold text-gray-800 mb-6">Create Account</h2>

      <form class="space-y-6" @submit.prevent="submitForm">
        <!-- Success message -->
        <div v-if="formSubmitted" class="p-4 bg-green-50 rounded-lg">
          <p class="text-sm text-green-800">Your account has been created successfully!</p>
        </div>
        <div>
          <label for="name" class="block text-sm font-medium text-gray-700 mb-1"
            >Full Name</label
          >
          <input
            v-model="form.name"
            id="name"
            type="text"
            class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-primary-500 focus:border-primary-500 transition-colors"
            :class="{'border-red-500': errors.name }"
            placeholder="John Doe"
          />
          <p v-if="errors.name" class="mt-1 text-sm text-red-600">{{ errors.name }}</p>
        </div>

        <div>
          <label
            for="email"
            class="block text-sm font-medium text-gray-700 mb-1"
            >Email Address</label
          >
          <input
            v-model="form.email"
            id="email"
            type="email"
            class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-primary-500 focus:border-primary-500 transition-colors"
            placeholder="john@example.com"
            :class="{'border-red-500': errors.email }"
          />
          <p v-if="errors.email" class="mt-1 text-sm text-red-600">{{ errors.email }}</p>
        </div>

        <div>
          <label
            for="password"
            class="block text-sm font-medium text-gray-700 mb-1"
            >Password</label
          >
          <div class="relative">
            <input
              v-model="form.password"
              id="password"
              type="password"
              class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-primary-500 focus:border-primary-500 transition-colors"
              placeholder="••••••••"
              :class="{'border-red-500': errors.password }"
            />
            <p v-if="errors.password" class="mt-1 text-sm text-red-600">{{ errors.password }}</p>
            <button
              type="button"
              class="absolute right-3 top-1/2 transform -translate-y-1/2 text-gray-500 hover:text-gray-700"
            >
              Show
            </button>
          </div>
        </div>

        <div class="flex items-start">
          <div class="flex items-center h-5">
            <input
              v-model="form.terms"
              id="terms"
              type="checkbox"
              :class="{'border-red-500': errors.terms }"
              class="h-4 w-4 text-primary-600 border-gray-300 rounded focus:ring-primary-500"
            />
          </div>
          <div class="ml-3">
            <label for="terms" class="text-sm text-gray-700">
              I agree to the
              <a href="#" class="text-primary-600 hover:text-primary-500"
                >Terms of Service</a
              >
              and
              <a href="#" class="text-primary-600 hover:text-primary-500"
                >Privacy Policy</a
              >
            </label>
          </div>
          
        </div>
        <p v-if="errors.terms" class="mt-1 text-sm text-red-600">{{ errors.terms }}</p>
        <div>
          <button
            type="submit"
            class="w-full flex justify-center py-2 px-4 border border-transparent rounded-lg shadow-sm text-sm font-medium text-white bg-primary-600 hover:bg-primary-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-primary-500 transition-colors"
            :disabled="isSubmitting"
          >
          <span v-if="isSubmitting">Processing...</span>
          <span v-else>Create Account</span>
          </button>
        </div>
        
      </form>

      <div class="mt-6 text-center">
        <p class="text-sm text-gray-600">
          Already have an account?
          <a
            href="#"
            class="font-medium text-primary-600 hover:text-primary-500"
            >Sign in</a
          >
        </p>
      </div>
    </div>
  </div>
</template>
<script setup>
import { ref, reactive } from "vue";
// Form State
const form = reactive({
  name: "",
  email: "",
  password: "",
  terms: false,
});

// UI state
const showPassword = ref(false);
const errors = reactive({});
const isSubmitting = ref(false);
const formSubmitted = ref(false);

const validateForm = () => {
  errors.name = !form.name ? "Name is required" : "";

  errors.email = !form.email ? "Email is required" : "";
  if (form.email && !/^\S+@\S+\.\S+$/.test(form.email)) {
    errors.email = "Please enter a valid email address";
  }

  errors.password = !form.password ? "Password is required" : "";
  if (form.password && form.password.length < 8) {
    errors.password = "Password must be at least 8 characters";
  }

  errors.terms = !form.terms ? "You must agree to the terms" : "";

  // Return true if no errors
  return !Object.values(errors).some((error) => error);
};

// Form submission
const submitForm = async () => {
  if (!validateForm()) return
  
  isSubmitting.value = true
  
  try {
    // Simulate API call
    await new Promise(resolve => setTimeout(resolve, 1500))
    
    // Show success message
    formSubmitted.value = true
    
    // Reset form after successful submission
    form.name = ''
    form.email = ''
    form.password = ''
    form.terms = false
    
    console.log('Form submitted successfully')
  } catch (error) {
    console.error('Error submitting form:', error)
  } finally {
    isSubmitting.value = false
  }
}
</script>

<style>
:root {
  --color-primary-500: #8b5cf6;
  --color-primary-600: #7c3aed;
  --color-primary-700: #6d28d9;
}

.text-primary-500 {
  color: var(--color-primary-500);
}

.text-primary-600 {
  color: var(--color-primary-600);
}

.hover\:text-primary-500:hover {
  color: var(--color-primary-500);
}

.hover\:text-primary-700:hover {
  color: var(--color-primary-700);
}

.bg-primary-600 {
  background-color: var(--color-primary-600);
}

.hover\:bg-primary-700:hover {
  background-color: var(--color-primary-700);
}

.border-primary-500 {
  border-color: var(--color-primary-500);
}

.focus\:ring-primary-500:focus {
  --tw-ring-color: var(--color-primary-500);
}

.focus\:border-primary-500:focus {
  border-color: var(--color-primary-500);
}
</style>

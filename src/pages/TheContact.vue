<script setup lang="ts">
import { reactive, ref } from 'vue'
import { z } from 'zod'

const contactSchema = z.object({
  name: z.string().min(2, 'Name must be at least 2 characters'),

  email: z.string().email('Invalid email address'),

  message: z
    .string()
    .min(10, 'Message must be at least 10 characters')
    .max(1000, 'Message is too long')
})

type ContactFormData = z.infer<typeof contactSchema>

const form = reactive<ContactFormData>({
  name: '',
  email: '',
  message: ''
})

const errors = reactive<Record<keyof ContactFormData, string | null>>({
  name: null,
  email: null,
  message: null
})

const isSubmitting = ref(false)
const isSuccess = ref(false)

const submit = async () => {
  isSuccess.value = false

  const result = contactSchema.safeParse(form)
  console.log('Check result Zod', result)

  if (!result.success) {
    const fieldErrors = result.error.flatten().fieldErrors

    errors.name = fieldErrors.name?.[0] ?? null
    errors.email = fieldErrors.email?.[0] ?? null
    errors.message = fieldErrors.message?.[0] ?? null

    return
  }

  errors.name = null
  errors.email = null
  errors.message = null

  isSubmitting.value = true

  try {
    await fetch('https://dfrnc.com/sandbox/migor/my/contact.php', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(result.data)
    })

    form.name = ''
    form.email = ''
    form.message = ''

    isSuccess.value = true
  } finally {
    isSubmitting.value = false
  }
}
</script>

<template>
  <div class="flex h-full grow items-center justify-center px-4">
    <div class="w-full max-w-2xl rounded-2xl bg-gray-100 p-6 shadow-sm">
      <h2 class="mb-6 text-center text-3xl font-semibold">Support</h2>

      <form class="space-y-4" @submit.prevent="submit">
        <div>
          <input
            v-model="form.name"
            type="text"
            placeholder="Your name"
            class="w-full rounded-lg border px-4 py-2 outline-none transition focus:border-blue-500"
          />
          <p v-if="errors.name" class="mt-1 text-sm text-red-500">
            {{ errors.name }}
          </p>
        </div>

        <div>
          <input
            v-model="form.email"
            type="email"
            placeholder="Email address"
            class="w-full rounded-lg border px-4 py-2 outline-none transition focus:border-blue-500"
          />
          <p v-if="errors.email" class="mt-1 text-sm text-red-500">
            {{ errors.email }}
          </p>
        </div>

        <div>
          <textarea
            v-model="form.message"
            rows="5"
            placeholder="Your message"
            class="w-full resize-none rounded-lg border px-4 py-2 outline-none transition focus:border-blue-500"
          />
          <p v-if="errors.message" class="mt-1 text-sm text-red-500">
            {{ errors.message }}
          </p>
        </div>

        <p v-if="isSuccess" class="text-center text-green-600">Message sent successfully ✅</p>

        <button
          type="submit"
          :disabled="isSubmitting"
          class="w-full rounded-lg bg-blue-600 py-2 font-medium text-white transition hover:bg-blue-700 disabled:opacity-50"
        >
          {{ isSubmitting ? 'Sending…' : 'Send message' }}
        </button>
      </form>
    </div>
  </div>
</template>

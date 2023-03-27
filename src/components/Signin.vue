<template>
<div class="max-w-sm m-auto my-8">
  <div class="border p-10 border-grey-light shadow rounded">
    <h3 class="text-2xl mb-6 text-grey-darkest">Sign In</h3>
    <form @submit.prevent="signin">
      <div class="text-red" v-if="error">
          {{ error }}
      </div>

      <div class="mb-6">
        <label for="email">E-mail</label>
      </div>
    </form>
  </div>
</div>
</template>

<script>

</script>

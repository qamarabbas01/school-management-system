<script>
  let email = "";
  let password = "";
  let isSubmitting = false;
  let errorMessage = "";

  $: isEmailValid = /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
  $: isPasswordValid = password.length >= 6;
  $: isFormValid = isEmailValid && isPasswordValid;

  async function handleSubmit() {
    if (!isFormValid) return;

    isSubmitting = true;
    errorMessage = "";

    try {
      await new Promise((resolve) => setTimeout(resolve, 1000));
      console.log("Login attempt with:", { email });
      alert("Login successful!");
    } catch (error) {
      errorMessage = "Invalid email or password";
    } finally {
      isSubmitting = false;
    }
  }
</script>

<div
  class="min-h-screen flex items-center justify-center bg-gradient-to-br from-purple-50 to-pink-50"
>
  <div class="w-full max-w-md p-8 space-y-8 bg-white rounded-lg shadow-lg">
    <div class="text-center">
      <h1 class="text-3xl font-bold text-gray-900">Welcome back</h1>
      <p class="mt-2 text-sm text-gray-600">Sign in to your account</p>
    </div>

    <form on:submit|preventDefault={handleSubmit} class="mt-8 space-y-6">
      {#if errorMessage}
        <div class="p-3 text-sm text-white bg-red-500 rounded-md">
          {errorMessage}
        </div>
      {/if}

      <div class="space-y-4">
        <div>
          <label for="email" class="block text-sm font-medium text-gray-700">
            Email address
          </label>
          <input
            id="email"
            type="email"
            bind:value={email}
            required
            class="w-full px-3 py-2 mt-1 text-gray-900 placeholder-gray-400 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-purple-500 focus:border-transparent"
            placeholder="you@example.com"
          />
          {#if email && !isEmailValid}
            <p class="mt-1 text-xs text-red-500">
              Please enter a valid email address
            </p>
          {/if}
        </div>

        <div>
          <label for="password" class="block text-sm font-medium text-gray-700">
            Password
          </label>
          <input
            id="password"
            type="password"
            bind:value={password}
            required
            class="w-full px-3 py-2 mt-1 text-gray-900 placeholder-gray-400 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-purple-500 focus:border-transparent"
            placeholder="••••••••"
          />
          {#if password && !isPasswordValid}
            <p class="mt-1 text-xs text-red-500">
              Password must be at least 6 characters
            </p>
          {/if}
        </div>

        <div class="flex items-center justify-between">
          <div class="flex items-center">
            <input
              id="remember-me"
              type="checkbox"
              class="w-4 h-4 text-purple-600 border-gray-300 rounded focus:ring-purple-500"
            />
            <label for="remember-me" class="block ml-2 text-sm text-gray-700">
              Remember me
            </label>
          </div>

          <div class="text-sm">
            <a
              href="/"
              class="font-medium text-purple-600 hover:text-purple-500"
            >
              Forgot your password?
            </a>
          </div>
        </div>
      </div>

      <div>
        <button
          type="submit"
          disabled={!isFormValid || isSubmitting}
          class="w-full px-4 py-2 text-sm font-medium text-white bg-purple-600 rounded-md hover:bg-purple-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-purple-500 disabled:opacity-50 disabled:cursor-not-allowed"
        >
          {isSubmitting ? "Signing in..." : "Sign in"}
        </button>
      </div>
    </form>

    <div class="text-center">
      <p class="text-sm text-gray-600">
        Don't have an account?
        <a href="/" class="font-medium text-purple-600 hover:text-purple-500">
          Sign up
        </a>
      </p>
    </div>
  </div>
</div>

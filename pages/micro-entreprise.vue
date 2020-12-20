<template>
  <div
    class="min-h-screen flex items-center justify-center bg-gray-50 py-12 px-4 sm:px-6 lg:px-8"
  >
    <div class="max-w-md w-full space-y-8">
      <div>
        <img
          class="mx-auto h-12 w-auto"
          src="https://tailwindui.com/img/logos/workflow-mark-indigo-600.svg"
          alt="Workflow"
        />
        <h2 class="mt-6 text-center text-3xl font-extrabold text-gray-900">
          ME tax calculator
        </h2>
      </div>
      <form class="mt-8 space-y-6" action="#" method="POST">
        <input type="hidden" name="remember" value="true" />
        <div class="rounded-md shadow-sm">
          <div>
            <label for="ca" class="font-medium">Chiffre d'affaire</label>
            <input
              id="ca"
              v-model="ca"
              name="ca"
              type="number"
              required
              class="mt-1 appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-t-md focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
              placeholder="Chiffre d'affaire"
            />
          </div>
        </div>

        <div class="border-t border-gray-400"></div>

        <div>
          <div class="mt-4">
            <label for="charges" class="font-medium">URSSAF</label>
            <input
              v-model="societyTaxes"
              type="number"
              disabled
              class="mt-1 appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-b-md focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
            />
          </div>
          <div class="mt-4">
            <label for="charges" class="font-medium">IR</label>
            <input
              :value="personalTaxes"
              type="number"
              disabled
              class="mt-1 appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-b-md focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
            />
          </div>
          <div class="mt-4">
            <label for="charges" class="font-medium">In my pocket</label>
            <input
              :value="inMyPocket"
              type="number"
              disabled
              class="mt-1 appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-b-md focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
            />
          </div>

          <div class="text-3xl mt-4 font-semibold">{{ ((inMyPocket / ca) * 100).toFixed(2) }} %</div>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      ca: 72000
    };
  },

  computed: {
    societyTaxes() {
      return this.ca * 0.22;
    },

    personalTaxes() {
      const anualSalary = this.ca * 0.78;
      if (anualSalary < 10065) {
        return 0
      } else if (anualSalary < 25659) {
        return (0.11 * anualSalary).toFixed(2);
      } else if (anualSalary < 73369) {
        return ((0.11 * 25659) + (0.3 * (anualSalary - 25659))).toFixed(2);
      }
    },

    inMyPocket() {
      return (
        (this.ca - this.societyTaxes) - this.personalTaxes
      ).toFixed(2);
    }
  }
};
</script>

<template>
  <div
    class="min-h-screen flex items-center justify-center bg-gray-50 py-12 px-4 sm:px-6 lg:px-8"
  >
    <div class="max-w-md w-full space-y-8">
      <div>
        <h2 class="mt-6 text-center text-3xl font-extrabold text-gray-800 uppercase tracking-tight">
          SAS tax calculator
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
          <div class="mt-4">
            <label for="salary" class="font-medium">Salaire par mois</label>
            <input
              id="salary"
              name="salary"
              v-model="salary"
              type="number"
              required
              class="mt-1 appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-b-md focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
              placeholder="Salaire"
            />
          </div>
          <div class="mt-4">
            <label for="charges" class="font-medium">Charges</label>
            <input
              id="charges"
              name="charges"
              v-model="charges"
              type="number"
              required
              class="mt-1 appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-b-md focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
              placeholder="Salaire"
            />
          </div>
        </div>

        <div class="border-t border-gray-400"></div>

        <div>
          <div>
            <label for="charges" class="font-medium">Profits <span class="text-xs">({{ this.ca }} - ({{ this.salary * 12 }} * 1.81 ) - {{ this.charges}})</span></label>
            <input
              :value="profits"
              type="number"
              disabled
              class="mt-1 appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-b-md focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
            />
          </div>
          <div class="mt-4">
            <label for="charges" class="font-medium">IR <span class="text-xs">(sur {{ 12 * salary }}€)</span></label>
            <div class="text-xs">
              <p>moins de 10 064€ : 0%</p>
              <p>10 064€ à 27 794€ : 14%</p>
              <p>27 794€ à 74 517€ : 30%</p>
              <p>74 517€ à 157 806€ : 41%</p>
              <p>+ de 157 806€ : 45%</p>
            </div>
            <input
              :value="personalTaxes"
              type="number"
              disabled
              class="mt-1 appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-b-md focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
            />
          </div>
          <div class="mt-4">
            <label for="charges" class="font-medium">IS <span class="text-xs">(sur {{ this.ca }}€, 15% jusqu'à 38120 puis 28%)</span></label>
            <input
              v-model="societyTaxes"
              type="number"
              disabled
              class="mt-1 appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-b-md focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
            />
          </div>
          <div class="mt-4">
            <label for="charges" class="font-medium"
              >Dividendes à prendre <span class="text-xs">(reste des profits)</span></label
            >
            <input
              :value="profits - societyTaxes"
              type="number"
              disabled
              class="mt-1 appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-b-md focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
            />
          </div>
          <div class="mt-4">
            <label for="charges" class="font-medium">In my pocket <span class="text-xs">(12 * {{ this.salary }} + (0.7 * {{ profits - societyTaxes }}))</span></label>
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
      <div>
        <NuxtLink class="text-indigo-500 hover:text-indigo-700 hover:underline" to="/micro-entreprise">
          tax calc pour ME
        </NuxtLink>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      ca: 100000,
      salary: 600,
      charges: 5000
    };
  },

  computed: {
    profits() {
      return this.ca - this.salary * 12 * 1.81 - this.charges;
    },

    societyTaxes() {
      if (this.profits < 38120) {
        return (0.15 * this.profits).toFixed(2);
      }
      return (0.15 * 38120 + (this.profits - 38120) * 0.28).toFixed(2);
    },

    personalTaxes() {
      const anualSalary = this.salary * 12;
      if (anualSalary < 10065) {
        return 0
      } else if (anualSalary < 25659) {
        return (0.11 * anualSalary).toFixed(2);
      } else if (anualSalary < 73369) {
        return ((0.11 * (25710 - 10084)) + (0.3 * (anualSalary - 25659))).toFixed(2);
      } else if (anualSalary < 73369) {
        return ((0.11 * (25710 - 10084)) + (0.3 * (anualSalary - 25659))).toFixed(2);
      } else if (anualSalary < 157806) {
        return ((0.11 * (25710 - 10084)) + (0.3 * (anualSalary - 73369)) + (0.41 * (anualSalary - 73369))).toFixed(2);
      } else {
        return ((0.11 * (25710 - 10084)) + (0.3 * (anualSalary - 73369)) + (0.41 * (anualSalary - 157806)) + (0.45 * (anualSalary - 157806))).toFixed(2);
      }
    },

    inMyPocket() {
      return (
        ((this.profits - this.societyTaxes) * 0.7 +
        12 * this.salary) - this.personalTaxes
      ).toFixed(2);
    }
  }
};
</script>

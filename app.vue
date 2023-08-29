<script setup lang="ts">
const buttons = [
  "7",
  "8",
  "9",
  "DEL",
  "4",
  "5",
  "6",
  "+",
  "1",
  "2",
  "3",
  "-",
  ".",
  "0",
  "/",
  "x",
];

const output = ref("");

const currentOperator = ref("");
const previousNumber = ref("");

const theme = ref(1);

const buttonsActions = (item: string) => {
  if (["+", "-", "/", "x"].includes(item)) {
    item === "x" ? (item = "*") : "";
    currentOperator.value = item;
    previousNumber.value = output.value;
    output.value = "";
  } else if (item === "DEL") {
    output.value = output.value.slice(0, -1);
  } else {
    output.value += item;
  }
};

const clearOutput = () => {
  output.value = "";
};

const showResult = () => {
  output.value = eval(
    previousNumber.value + currentOperator.value + output.value
  );
};

const changeTheme = () => {
  theme.value === 1 ? theme.value++ : (theme.value = 1);
};
</script>

<template>
  <div :class="{ dark: theme === 2 }">
    <div
      class="font-default text-default text-white bg-slate-600 h-screen flex dark:bg-neutral-100 dark:text-black"
    >
      <div class="font-bold max-w-lg m-auto">
        <header class="flex justify-between items-center">
          <div>calc</div>
          <div class="flex">
            <span class="text-xs mr-5 mt-auto mb-1">THEME</span>
            <div>
              <div
                class="bg-slate-800 w-14 h-6 rounded-full relative dark:bg-stone-300"
              >
                <button
                  @click="changeTheme"
                  :class="{ 'translate-x-8': theme === 2 }"
                  class="bg-red-600 w-4 h-4 absolute top-1 left-1 rounded-full hover:bg-red-500 dark:bg-orange-600 dark:hover:bg-orange-400"
                ></button>
              </div>
            </div>
          </div>
        </header>
        <main>
          <div
            class="bg-slate-900 mt-4 text-right p-5 rounded-xl text-4xl xs:p-6 sm:py-7 sm:text-5xl dark:bg-white"
          >
            {{ output || 0 }}
          </div>
          <div
            class="bg-slate-800 mt-4 p-5 rounded-xl xs:p-6 xs:mt-6 dark:bg-stone-300"
          >
            <div class="grid grid-cols-4 gap-2 xs:gap-3 sm:gap-5">
              <button
                @click="buttonsActions(item)"
                class="my-shadow text-center py-1 rounded-md w-12 xs:w-14 xs:py-2 sm:w-20 sm:py-1 sm:rounded-lg"
                :class="{
                  'bg-red-50 text-slate-700 hover:bg-white dark:bg-neutral-100 dark:hover:bg-white':
                    item !== 'DEL',
                  'bg-slate-600 text-white text-xl flex items-center justify-center hover:bg-slate-500 sm:text-2xl dark:bg-cyan-600 dark:hover:bg-cyan-500':
                    item === 'DEL',
                }"
                v-for="(item, index) in buttons"
                :key="index"
              >
                {{ item }}
              </button>
            </div>
            <div
              class="grid grid-cols-2 gap-2 mt-2 xs:gap-3 xs:mt-3 sm:gap-5 sm:mt-5"
            >
              <button
                @click="clearOutput"
                class="my-shadow text-center py-3 rounded-md bg-slate-600 text-white text-xl hover:bg-slate-500 xs:py-4 sm:py-3 sm:text-2xl dark:bg-cyan-600 dark:hover:bg-cyan-500"
              >
                RESET
              </button>
              <button
                @click="showResult"
                class="my-shadow text-center py-3 rounded-md bg-red-600 text-white text-lg hover:bg-red-500 xs:py-4 sm:py-3 sm:text-2xl dark:bg-orange-600 dark:hover:bg-orange-400"
              >
                =
              </button>
            </div>
          </div>
        </main>
      </div>
    </div>
  </div>
</template>

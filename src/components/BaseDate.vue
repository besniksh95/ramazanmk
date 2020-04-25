<template>
  <div>
    <div @click="otherTimes = !otherTimes" class="rounded-xl shadow-input cursor-pointer" style="background-color: rgba(255, 255, 255, 0.5);">
      <div class="px-6 h-20 flex flex-row items-center">
        <icon name="cloud" class="w-12 h-12 mr-4"></icon>
        <p class="text-2xl font-medium text-primary">
          {{ this.date }} <span class="font-semibold">{{ this.year }}</span>
        </p>
      </div>
    </div>


    <transition
        enter-active-class="transition-all ease-out duration-100"
        enter-class="transform opacity-0 scale-95"
        enter-to="transform opacity-0 scale-95"
        leave-class="transform opacity-100 scale-100"
        leave-to-class="transform opacity-0 scale-95"
        leave-active-class="transition-all ease-in duration-75"
      >
        <div
          v-show="otherTimes"
          class="absolute inset-0 flex items-center justify-center "
        >
          <div class="w-64 h-100 z-20 bg-white rounded-xl overflow-hidden">
            <div class="flex flex-row justify-between px-8 py-2 text-primary text-xl hover:bg-gray-300">
              <span>{{ $t("imsaku") }}</span>
              <span>{{ formatDate(this.current.schedule.sifir) }}</span>
            </div>
            <div class="flex flex-row justify-between px-8 py-2 text-primary text-xl hover:bg-gray-300">
              <span>{{ $t("sabahu") }}</span>
              <span>{{ formatDate(this.current.schedule.sabahu) }}</span>
            </div>
            <div class="flex flex-row justify-between px-8 py-2 text-primary text-xl hover:bg-gray-300">
              <span>{{ $t("dreka") }}</span>
              <span>{{ formatDate(this.current.schedule.dreka) }}</span>
            </div>
            <div class="flex flex-row justify-between px-8 py-2 text-primary text-xl hover:bg-gray-300">
              <span>{{ $t("ikindia") }}</span>
              <span>{{ formatDate(this.current.schedule.ikindia) }}</span>
            </div>
            <div class="flex flex-row justify-between px-8 py-2 text-primary text-xl hover:bg-gray-300">
              <span>{{ $t("akshami") }}</span>
              <span>{{ formatDate(this.current.schedule.iftar) }}</span>
            </div>
            <div class="flex flex-row justify-between px-8 py-2 text-primary text-xl hover:bg-gray-300">
              <span>{{ $t("jacia") }}</span>
              <span>{{ formatDate(this.current.schedule.jacia) }}</span>
            </div>
          </div>
        </div>
      </transition>

  <div
    @click="otherTimes = false"
    v-show="otherTimes"
    style="background-color: rgba(0, 0, 0, 0.69)"
    class="absolute inset-0 w-full h-full z-10"
  ></div>
  </div>
</template>

<script>
export default {
  props: {
    date: {
      type: String
    },
    year: {
      type: String
    },
    current: {
      type: Object,
      required: true
    }
  },
  data(){
    return {
      otherTimes: false
    }
  },
  watch: {
    otherTimes(value) {
      const bodyTag = document.querySelector("body");
      if (value) {
        bodyTag.classList.add("overflow-hidden");
        this.$emit("menuVisible", false)
      } else {
        bodyTag.classList.remove("overflow-hidden");
        this.$emit("menuVisible", true)
      }
    }
  },
  methods: {
    formatDate(item) {
      return this.$dayjs(item)
        .add(this.current.city.value, "minute")
        .format("HH:mm");
    }
  }
};
</script>

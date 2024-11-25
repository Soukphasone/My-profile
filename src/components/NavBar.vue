<template>
  <header
    class="flex justify-between items-center p-6 bg-opacity-50 relative z-20"
  >
    <div class="font-lao text-3xl font-bold">
      <!-- <div class="p-4 max-w-sm mx-auto">
        <select
          id="language"
          v-model="currentLanguage"
          @change="changeLanguage"
          class="border rounded p-2 text-sm"
        >
          <option value="en" class="flex items-center">EN</option>
          <option value="la" class="flex items-center">LA</option>
        </select>
      </div> -->
      <div class="custom-select">
        <button @click="toggleDropdown">
          <span v-if="check === 'la'">
            <img src="@/assets/laos-flag.png" alt="flag" class="flag-icon"
          /></span>
          <span v-else>
            <img
              src="@/assets/united-states-flag-icon.png"
              alt="flag"
              class="flag-icon"
          /></span>
          {{ $t("languages") }}
        </button>
        <ul v-if="isOpen" class="dropdown">
          <li
            v-for="option in options"
            :key="option.value"
            @click="selectOption(option.value)"
          >
            <img :src="getImagePath(option.img)" alt="flag" class="flag-icon" />

            {{ option.language }}
          </li>
        </ul>
      </div>
    </div>

    <!-- Mobile Toggle Button -->
    <div class="md:hidden z-30">
      <button
        type="button"
        class="block focus:outline-none"
        @click="isMenuOpen = !isMenuOpen"
      >
        <span v-if="isMenuOpen" class="text-5xl">
          <img
            src="https://img.icons8.com/ios-filled/100/ffffff/delete-sign.png"
            alt="close"
            width="50"
            height="50"
          />
        </span>
        <span v-else class="text-5xl">
          <img
            src="https://img.icons8.com/ios-filled/100/ffffff/menu--v6.png"
            alt="menu"
            width="50"
            height="50"
          />
        </span>
      </button>
    </div>
    <!-- Navbar Link -->
    <nav
      :class="[
        'fixed inset-0 z-20 flex flex-col items-center justify-center bg-[#111827] md:relative md:bg-transparent md:flex md:justify-between md:flex-row',
        isMenuOpen ? 'block' : 'hidden',
      ]"
    >
      <ul
        class="flex flex-col items-center space-y-5 md:flex-row md:space-x-5 md:space-y-0"
      >
        <li v-for="item in check === 'en' ? Menu_en : Menu" :key="item.name">
          <a
            :href="item.href"
            class="font-lao block text-white transition hover:text-primary ease-linear text-2xl md:text-lg"
            @click="scrollToSection(item.href)"
          >
            {{ item.name }}
          </a>
        </li>
      </ul>
    </nav>
  </header>
</template>
<script setup>
import { ref, watch } from "vue";
import { useI18n } from "vue-i18n";
import { currentLanguage } from "../i18n";
const Menu = ref([
  // { name: "Services", href: "#services" },
  { name: "ກ່ຽວກັບຂ້ອຍ", href: "#about" },
  { name: "ທັກສະ", href: "#skills" },
  // { name: "Projects", href: "#projects" },
  // { name: "Testimonials", href: "#testimonials" },
  { name: "ຊ່ອງທາງຕິດຕໍ່", href: "#contact" },
]);
const Menu_en = ref([
  // { name: "Services", href: "#services" },
  { name: "About Me", href: "#about" },
  { name: "Skills", href: "#skills" },
  // { name: "Projects", href: "#projects" },
  // { name: "Testimonials", href: "#testimonials" },
  { name: "Contact", href: "#contact" },
]);
const isMenuOpen = ref(false);
const scrollToSection = (href) => {
  isMenuOpen.value = false;
  const section = document.querySelector(href);
  if (section) {
    section.scrollIntoView({ behavior: "smooth" });
  }
};
// Handle language switching
const { locale } = useI18n();
const check = ref(currentLanguage.value);
// Watch for changes in the global language state and update the i18n locale
watch(currentLanguage, (newLanguage) => {
  locale.value = newLanguage;
  check.value = newLanguage;
});

// Dropdown functionality
const isOpen = ref(false);
const selected = ref(currentLanguage.value);
const options = ref([
  {
    value: "en",
    language: "EN(ອັງກິດ)",
    img: "united-states-flag-icon.png",
  },
  {
    value: "la",
    language: "ລາວ(LA)",
    img: "laos-flag.png",
  },
]);

const toggleDropdown = () => {
  isOpen.value = !isOpen.value;
};

const selectOption = (option) => {
  selected.value = option;
  currentLanguage.value = option;
  isOpen.value = false;
  localStorage.setItem("language", option);
};
const getImagePath = (img) => {
  return new URL(`/src/assets/${img}`, import.meta.url).href;
};
</script>
<style>
.custom-select {
  position: relative;
  display: inline-block;
}
.custom-select button {
  background: #f0f0f0;
  border: 1px solid #ccc;
  padding: 0 10px;
  cursor: pointer;
  display: flex;
  font-size: 15px;
}
.dropdown {
  position: absolute;
  top: 100%;
  left: 0;
  background: #fff;
  border: 1px solid #ccc;
  list-style: none;
  padding: 0;
  margin: 0;
  width: 100%;
}
.dropdown li {
  padding: 5px 5px 0 5px;
  cursor: pointer;
  font-size: 15px;
  display: flex;
}
.dropdown li:hover {
  background: #eee;
}
.flag-icon {
  width: 20px;
  height: 15px;
  margin: 10px 10px 0 0;
}
</style>

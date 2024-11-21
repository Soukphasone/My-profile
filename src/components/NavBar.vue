<template>
  <header
    class="flex justify-between items-center p-6 bg-opacity-50 relative z-20"
  >
    <div class="font-lao text-3xl font-bold">
      <div class="p-4 max-w-sm mx-auto">
          <select
            id="language"
            v-model="currentLanguage"
            @change="changeLanguage"
            class="border rounded p-2 text-sm"
          >
            <option value="en" class="flex items-center">EN</option>
            <option value="la" class="flex items-center">LA</option>
          </select>
      </div>

      <!-- <img src="@/assets/laos-flag.png" alt="LOGO" class="inline-block w-15 h-10" /> -->
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
        <li v-for="item in Menu" :key="item.name">
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
  { name: "ຕິດຕໍ່", href: "#contact" },
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

// Watch for changes in the global language state and update the i18n locale
watch(currentLanguage, (newLanguage) => {
  locale.value = newLanguage;
});

// Function to persist the selected language in localStorage
const changeLanguage = () => {
  localStorage.setItem("language", currentLanguage.value);
};
</script>

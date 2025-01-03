<template>
  <header
    class="flex justify-between items-center p-6 bg-opacity-50 relative z-20"
  >
    <div class="font-lao text-3xl font-bold">
      <div class="flex space-x-1">
        <div class="custom-select">
          <button class="bt-mb" @click="toggleDropdown">
            <span v-if="check === 'la'">
              <img src="@/assets/laos-flag.png" alt="flag" class="flag-bt"
            /></span>
            <span v-else-if="check === 'en'">
              <img
                src="@/assets/england-flag.png"
                alt="flag"
                class="flag-bt"
            /></span>
            <span v-else>
              <img
                src="@/assets/vietnam-flag.png"
                alt="flag"
                class="flag-bt"
            /></span>
          </button>
          <ul v-if="isOpen" class="dropdown-mb">
            <li
              v-for="option in options"
              :key="option.value"
              @click="selectOption(option.value)"
            >
              <img
                :src="getImagePath(option.img)"
                alt="flag"
                class="flag-icon-mb"
              />

              <span style="margin-top: -8px;"> {{ option.lg }}</span>
            </li>
          </ul>
        </div>
        <div class="text-white text-sm mt-2">{{ $t("lg") }}</div>
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
        <li
          v-for="item in check === 'en'
            ? Menu_en
            : check === 'vn'
            ? Menu_vn
            : Menu"
          :key="item.name"
        >
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
const Menu_vn = ref([
  // { name: "Services", href: "#services" },
  { name: "Giới thiệu", href: "#about" },
  { name: "Kỹ năng", href: "#skills" },
  // { name: "Projects", href: "#projects" },
  // { name: "Testimonials", href: "#testimonials" },
  { name: "Liên hệ", href: "#contact" },
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
    language: "US (EN)",
    lg: "EN",
    img: "england-flag.png",
  },
  {
    value: "la",
    language: "ລາວ (LA)",
    lg: "LA",
    img: "laos-flag.png",
  },
  {
    value: "vn",
    language: "Tiếng việt (VN)",
    lg: "VN",
    img: "vietnam-flag.png",
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
.custom-select .bt-mb {
  background: #f0f0f0;
  /* border: 2px solid black; */
  padding: 5px 5px;
  cursor: pointer;
  display: flex;
  font-size: 20px;
  color: black;
  border-radius: 50%;
  width: 35px;
  height: 35px;
  border: 1px solid rgb(67, 5, 5);
}

.custom-select .dropdown-mb {
  position: absolute;
  top: 100%;
  left: 0;
  background: #fff;
  border: 1px solid #ccc;
  list-style: none;
  /* padding: 5px 10px 5px 5px; */
  margin: 5px 0 0 0;
  width: 70px;
  color: #000;
  z-index: 100;
}

.dropdown-mb li {
  padding: 10px 7px 0px 7px;
  /* cursor: pointer; */
  font-size: 13px;
  display: flex;
  gap: 10px;

}

.flag-icon-mb {
  width: 25px;
  height: 18px;
  border: 1px solid rgb(193, 193, 193);

}

.bt-mb .flag-bt {
  width: 20px;
  height: 20px;
  margin: 1.8px 0 0 1.6px;
  border: 1px solid gray;
}

.bt-mb img {
  border-radius: 50%;
}

.dropdown-mb li:hover {
  background: #eee;
}
</style>

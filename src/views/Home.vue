<template>
  <div class="home">
    <HeaderLusy />
    <HeroSection />
    <WomanLusy />
    <InstaLusy />
    <PartnerLusy />
    <FooterLusy />
  </div>
</template>

<script>
// @ is an alias to /src
import HeaderLusy from "@/components/HeaderLusy.vue";
import HeroSection from "@/components/HeroSection.vue";
import WomanLusy from "@/components/WomanLusy.vue";
import InstaLusy from "@/components/InstaLusy.vue"; 
import PartnerLusy from "@/components/PartnerLusy.vue";
import FooterLusy from "@/components/FooterLusy.vue";

export default {
  name: "Home",
  components: {
    HeaderLusy,
    HeroSection,
    WomanLusy,
    InstaLusy,
    PartnerLusy,
    FooterLusy
  }
};
</script>

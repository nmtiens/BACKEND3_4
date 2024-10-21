<script>
import ContactForm from "@/components/ContactForm.vue";
import contactService from "@/services/contact.service";

export default {
  components: {
    ContactForm,
  },
  data() {
    return {
      contact: {
        name: "",
        email: "",
        address: "",
        phone: "",
        favourite: false,
      },
      message: "",
    };
  },
  methods: {
    async insertContact(data) {
      try {
        await contactService.create(data);
        alert("Liên hệ được thêm thành công.");
        this.$router.push({ name: "contactbook" });
      } catch (error) {
        console.log(error);
      }
    },
  },
  created() {
    this.message = "";
  },
};
</script>

<template>
  <div v-if="contact" class="page">
    <h4>Thêm liên hệ mới</h4>
    <ContactForm :contact="contact" @submit:contact="insertContact" />
    <p>{{ message }}</p>
  </div>
</template>

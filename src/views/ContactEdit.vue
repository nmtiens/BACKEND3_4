<script>
import ContactForm from "../components/ContactForm.vue";
import contactService from "@/services/contact.service";

export default {
  components: {
    ContactForm,
  },
  props: {
    id: { type: String, required: true },
  },
  data() {
    return {
      contact: null,
      message: "",
    };
  },
  methods: {
    async getContact(id) {
      try {
        this.contact = await contactService.get(id);
      } catch (error) {
        console.log(error);
        // Chuyen sang trang Not Found dong thoi giu cho URL khong doi
        this.$router.push({
          name: "notFound",
          params: {
            pathMatch: this.$router.path.split("/").slide(1),
          },
          query: this.$router.query,
          hash: this.$router.hash,
        });
      }
    },

    async updateContact(data) {
      try {
        if (!data) {
          throw new Error("Data không được cung cấp");
        }
        await contactService.update(this.contact._id, data);
        alert("Liên hệ được cập nhật thành công. ");
        this.$router.push({ name: "contactbook" });
      } catch (error) {
        console.log(error);
      }
    },

    async deleteContact() {
      if (confirm("Bạn muốn xóa liên hệ này?")) {
        try {
          await contactService.delete(this.contact._id);
          this.$router.push({ name: "contactbook" });
        } catch (error) {
          console.log(error);
          alert("Xóa liên hệ thất bại.");
        }
      }
    },
  },
  created() {
    this.getContact(this.id);
    this.message = "";
  },
};
</script>

<template>
  <div v-if="contact" class="page">
    <h4>Hiệu chỉnh liên hệ</h4>
    <ContactForm
      :contact="contact"
      @submit:contact="updateContact"
      @delete:contact="deleteContact"
    />
    <p>{{ message }}</p>
  </div>
</template>

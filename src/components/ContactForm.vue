<script>
import * as yup from "yup";
import { Form, Field, ErrorMessage } from "vee-validate";

export default {
  components: {
    Form,
    Field,
    ErrorMessage,
  },
  emits: ["submit:contact", "delete:contact"],
  props: {
    contact: {
      type: Object,
    },
  },
  data() {
    const contactFormSchema = yup.object().shape({
      name: yup
        .string()
        .required("Ten pha co gia tri")
        .min(2, "Ten phai co it nhat 2 ky tu.")
        .max(50, "Ten co nhieu nhat 50 ky tu."),
      email: yup
        .string()
        .email("Email khong dung.")
        .max(50, "E-mail toi da 50 ky tu"),
      address: yup.string().max(100, "Dia chi toi da 100 ky tu."),
      phone: yup
        .string()
        .matches(
          /((09|03|07|08|05)+([0-9]{8})\b)/g,
          "So dien thoai khong hop le."
        ),
    });
    return {
      // Chung ta se khong muon hieu chinh props, nen tao bien cuc bo
      // contactLocal de lien ket voi cac input tren form
      contactLocal: this.contact,
      contactFormSchema,
    };
  },
  methods: {
    submitContact() {
      this.$emit("submit:contact", this.contactLocal);
    },
    deleteContact() {
      this.$emit("delete:contact", this.contactLocal.id);
    },
    Cancel() {
      const reply = window.confirm(
        "You have unsaved changes! Do you want to leave?"
      );
      if (!reply) {
        // Stay on the page if user clicks 'Cancel'
        return false;
      } else {
        this.$router.push({ name: "contactbook" });
      }
    },
  },
};
</script>
<style scoped>
@import "@/assets/form.css";
</style>
<template>
  <Form @submit="submitContact" :validation-schema="contactFormSchema">
    <div class="form-group">
      <label for="name">Ten</label>
      <Field
        id="name"
        name="name"
        type="text"
        class="form-control"
        v-model="contactLocal.name"
      />
      <ErrorMessage name="name" class="error-feedback" />
    </div>

    <div class="form-group">
      <label for="email">E-mail</label>
      <Field
        id="email"
        name="email"
        type="email"
        class="form-control"
        v-model:model-value="contactLocal.email"
      />
      <ErrorMessage name="email" class="error-feedback" />
    </div>
    <div class="form-group">
      <label for="address">Dia chi</label>
      <Field
        name="address"
        class="form-control"
        type="text"
        v-model="contactLocal.address"
      />
      <ErrorMessage name="address" class="error-feedback" />
    </div>
    <div class="form-group">
      <label for="phone">So dien thoai</label>
      <Field
        id="phone"
        name="phone"
        type="tel"
        class="form-control"
        v-model="contactLocal.phone"
      />
      <ErrorMessage name="phone" class="error-feedback" />
    </div>
    <div class="form-group form-check">
      <input
        type="checkbox"
        id="favourite"
        name="favourite"
        v-model="contactLocal.favourite"
      />
      <label class="form-check-label" for="favourite">
        <strong>Lien he yeu thich</strong>
      </label>
    </div>
    <div class="form-group">
      <button class="btn btn-primary">Luu</button>
      <button
        v-if="contactLocal._id"
        type="button"
        class="ml-2 btn btn-danger"
        @click="deleteContact"
      >
        Xoa
      </button>
      <button class="ml-2 btn btn-danger" type="button" @click="Cancel">
        Thoat
      </button>
    </div>
  </Form>
</template>

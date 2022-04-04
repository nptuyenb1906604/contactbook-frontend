<!--<template>
    <div class="page">
        <h4>Thêm Liên hệ</h4>
        <form>
            <div class="form-group">
                <label for="name">Tên</label>
                <input name="name" type="text" class="form-control" v-model="contact.name" />
            </div>
            <div class="form-group">
                <label for="email">E-mail</label>
                <input name="email" type="email" class="form-control" v-model="contact.email" />
            </div>
            <div class="form-group">
                <label for="address">Địa chỉ</label>
                <input name="address" type="text" class="form-control" v-model="contact.address" />
            </div>
            <div class="form-group">
                <label for="phone">Điện thoại</label>
                <input name="phone" type="tel" class="form-control" v-model="contact.phone" />
            </div>
            <div class="form-group form-check">
                <input
                    name="favorite"
                    type="checkbox"
                    class="form-check-input"
                    v-model="contact.favorite"
                />
                <label for="favorite" class="form-check-label">
                    <strong>Liên hệ yêu thích</strong>
                </label>
            </div>
            <div class="form-group">
                <button class="btn btn-primary" @click="createContact">Lưu</button>
            </div>
        </form>
    </div>
</template>

<script>
import ContactService from "@/services/contact.service";

export default {
    data() {
        return {
            contact: { name: '', email: '', address: '', phone: '', favorite: '' },
            message: "",
        };
    },
    methods: {
        async createContact() {
            let newContact = {
                name: this.contact.name,
                email: this.contact.email,
                address: this.contact.address,
                phone: this.contact.phone,
                favorite: this.contact.favorite
            }
            try {
                await ContactService.create(newContact);
                confirm("Liên hệ được thêm thành công.");
            } catch (error) {
                console.log(error);
            }
        },
    },
};
</script>

<style scoped>
@import "@/assets/form.css";
</style>-->
<template>
    <div class="page">
        <h4>Thêm Liên hệ</h4>
        <Form @submit="submit" :validation-schema="contactFormSchema">
            <div class="form-group">
                <label for="name">Tên</label>
                <Field name="name" type="text" class="form-control" v-model="contact.name" />
                <ErrorMessage name="name" class="error-feedback" />
            </div>
            <div class="form-group">
                <label for="email">E-mail</label>
                <Field name="email" type="email" class="form-control" v-model="contact.email" />
                <ErrorMessage name="email" class="error-feedback" />
            </div>
            <div class="form-group">
                <label for="address">Địa chỉ</label>
                <Field name="address" type="text" class="form-control" v-model="contact.address" />
                <ErrorMessage name="address" class="error-feedback" />
            </div>
            <div class="form-group">
                <label for="phone">Điện thoại</label>
                <Field name="phone" type="tel" class="form-control" v-model="contact.phone" />
                <ErrorMessage name="phone" class="error-feedback" />
            </div>
            <div class="form-group form-check">
                <input
                    name="favorite"
                    type="checkbox"
                    class="form-check-input"
                    v-model="contact.favorite"
                />
                <label for="favorite" class="form-check-label">
                    <strong>Liên hệ yêu thích</strong>
                </label>
            </div>
            <div class="form-group">
                <button class="btn btn-primary" @click="createContact">Lưu</button>
            </div>
        </Form>
    </div>
</template>

<script>
import ContactService from "@/services/contact.service";
import * as yup from "yup";
import { Form, Field, ErrorMessage } from "vee-validate";

export default {
    components: {
        Form,
        Field,
        ErrorMessage,
    },
    data() {
        const contactFormSchema = yup.object().shape({
            name: yup
                .string()
                .required("Tên phải có giá trị.")
                .min(2, "Tên phải ít nhất 2 ký tự.")
                .max(50, "Tên có nhiều nhất 50 ký tự."),
            email: yup
                .string()
                .email("E-mail không đúng.")
                .max(50, "E-mail tối đa 50 ký tự."),
            address: yup.string().max(100, "Địa chỉ tối đa 100 ký tự."),
            phone: yup
                .string()
                .matches(
                    /((09|03|07|08|05)+([0-9]{8})\b)/g,
                    "Số điện thoại không hợp lệ."
                ),
        });
        return {
            contact: { name: '', email: '', address: '', phone: '', favorite: '' },
            contactFormSchema,
        };
    },
    methods: {
         createContact() {
            let newContact = {
                name: this.contact.name,
                email: this.contact.email,
                address: this.contact.address,
                phone: this.contact.phone,
                favorite: this.contact.favorite
            }
            try {
                ContactService.create(newContact);
                confirm("Liên hệ được thêm thành công.");
            } catch (error) {
                console.log(error);
            }
        },
    },
};
</script>

<style scoped>
@import "@/assets/form.css";
</style>
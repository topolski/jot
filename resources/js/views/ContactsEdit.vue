<template>
    <div>
        <div class="flex justify-between">
            <a href="#" class="text-blue-400" @click="$router.back()">
                < Back
            </a>
        </div>
        <form @submit.prevent="submitForm">
            <InputField name="name" label="Contact Name" placeholder="Contact Name" @update:field="form.name = $event" :data="form.name" :errors="errors" />
            <InputField name="email" label="Contact Email" placeholder="Contact Email" @update:field="form.email = $event" :data="form.email" :errors="errors" />
            <InputField name="company" label="Company" placeholder="Company" @update:field="form.company = $event" :data="form.company" :errors="errors" />
            <InputField name="birthday" label="Birthday" placeholder="MM/DD/YYYY" @update:field="form.birthday = $event" :data="form.birthday" :errors="errors" />

            <div class="flex justify-end">
                <button class="py-2 px-4 rounded text-red-700 border mr-5 hover:border-red-700">Cancel</button>
                <button class="bg-blue-500 py-2 px-4 rounded text-white hover:bg-blue-400">Save</button>
            </div>
        </form>
    </div>
</template>

<script>
    import InputField from "../components/InputField";

    export default {
        name: "ContactsEdit",
        components: {
            InputField
        },
        mounted() {
            axios.get('/api/contacts/' + this.$route.params.id)
                .then(response => {
                    this.form = response.data.data;
                    this.loading = false;
                })
                .catch(error => {
                    this.loading = false;

                    if (error.response.status === 404) {
                        this.$router.push('/contacts');
                    }
                });
        },
        data: function () {
            return {
                form: {
                    'name': '',
                    'email': '',
                    'company': '',
                    'birthday': '',
                },
                errors: null,
            }
        },
        methods: {
            submitForm: function () {
                axios.patch('/api/contacts/' + this.$route.params.id, this.form)
                    .then(response => {
                        this.$router.push(response.data.links.self);
                    })
                    .catch(errors => {
                        this.errors = errors.response.data.errors;
                    });
            }
        }
    }
</script>

<style scoped>

</style>

<template lang="en">
    <div>
        <div class="row">
            <router-link to="/customers" class="btn btn-primary">All Customers</router-link>
        </div>
        <div class="row justify-content-center">
            <div class="col-xl-12 col-lg-12 col-md-12">
                <div class="card shadow-sm my-5">
                    <div class="card-body p-0">
                        <div class="row">
                            <div class="col-lg-12">
                                <div class="login-form">
                                    <div class="text-center">
                                        <h1 class="h4 text-gray-900 mb-4">Add Customer</h1>
                                    </div>
                                    <form class="user" @submit.prevent="handleCustomerRegister"
                                        enctype="multipart/form-data">

                                        <div class="form-group">

                                            <div class="form-group">
                                                <div class="form-row">
                                                    <div class="col-md-6">
                                                        <input type="text" v-model="form.name" class="form-control"
                                                            id="inputName" placeholder="Enter Your full name">
                                                        <small class="text-danger"
                                                            v-if="errors.name">{{ errors.name[0] }}</small>

                                                    </div>
                                                    <div class="col-md-6">
                                                        <input type="email" v-model="form.email" class="form-control"
                                                            id="inputEmail" placeholder="Enter Your email">
                                                        <small class="text-danger"
                                                            v-if="errors.email">{{ errors.email[0] }}</small>

                                                    </div>
                                                </div>
                                            </div>

                                            <div class="form-group">
                                                <div class="form-row">
                                                    <div class="col-md-6">
                                                        <input type="text" v-model="form.address" class="form-control"
                                                            id="inputaddress" placeholder="Enter Your address">
                                                        <small class="text-danger"
                                                            v-if="errors.address">{{ errors.address[0] }}</small>

                                                    </div>

                                                    <div class="col-md-6">
                                                        <input type="text" v-model="form.phone" class="form-control"
                                                            id="inputPhone" placeholder="Enter Your phone number">
                                                        <small class="text-danger"
                                                            v-if="errors.phone">{{ errors.phone[0] }}</small>

                                                    </div>

                                                </div>
                                            </div>


                                        </div>


                                        <div class="form-group">
                                            <div class="form-row ml-1">
                                                <div class="col-md-6">
                                                    <input type="file" class="custom-file-input" id="customFile"
                                                        @change="onImageUpload">
                                                    <small class="text-danger"
                                                        v-if="errors.photo">{{ errors.photo[0] }}</small>
                                                    <label class="custom-file-label" for="customFile">Choose
                                                        file</label>

                                                </div>
                                                <div class="col-md-6">
                                                    <img v-if="form.photo" :src="form.photo"
                                                        style="height: 50px; width: 90px;">
                                                </div>
                                            </div>
                                        </div>

                                        <div class="form-group">
                                            <button type="submit" class="btn btn-primary">Add Customer</button>
                                        </div>
                                        <hr>
                                    </form>

                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
    </div>
</template>

<script>
    export default {
        created() {
            if (!User.loggedIn()) {
                this.$router.push({
                    name: '/'
                })
            }
        },
        data() {
            return {
                form: {
                    name: null,
                    email: null,
                    phone: null,
                    address: null,
                    photo: null,
                },
                errors: {},
            }
        },
        methods: {
            // Show Image Preview
            onImageUpload(event) {
                let file = event.target.files[0];
                if (file.size > 1048770) {
                    Notification.image_validation();
                } else {
                    let reader = new FileReader();
                    reader.onload = event => {
                        this.form.photo = event.target.result;
                        console.log(event.target.result);
                    };
                    reader.readAsDataURL(file);
                }

            },
            handleCustomerRegister() {
                axios.post('/api/customer', this.form)
                    .then(() => {
                        this.$router.push({
                            name: 'customers'
                        })
                        Notification.success();
                    })
                    .catch(
                        error => this.errors = error.response.data.errors
                    );
            },
        }
    }

</script>

<style lang="en">

</style>

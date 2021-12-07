<template>
    <b-container class="m-0 mb-3 d-flex flex-column align-items-center">
        <div class="header">
            <h4>Get in touch</h4>
            <p>
                Trysail transom furl Sea Legs scallywag Jack Ketch chandler mizzenmast reef sails skysail. Shiver me timbers llot bucko belaying Sea Legs boom gunwails booty fury most fore.
            </p>
        </div>
        <b-container class="form p-4 m-0">
            <h5 class="mb-4">Contact Form</h5>
            <b-form>
                <b-row>
                    <b-col class="col-sm-6 col-12 mb-3">
                        <label for="name">Name &#42;</label>
                        <b-form-input
                            type="text"
                            min="3"
                            name="name"
                            :required="true"
                        >
                        </b-form-input>
                    </b-col>
                    <b-col class="col-sm-6 col-12 mb-3">
                        <label for="email">Email Address &#42;</label>
                        <b-form-input
                            type="email"
                            name="email"
                            :required="true"
                        >
                        </b-form-input>
                    </b-col>
                </b-row>
                <b-row>
                    <b-col>
                        <label for="message">Message &#42;</label>
                        <b-form-textarea
                            type="textarea"
                            min="3"
                            :required="true"
                        >
                        </b-form-textarea>
                    </b-col>
                </b-row>
                <b-row class="d-flex justify-content-end px-3 pt-3">
                    <b-button class="py-1 px-4" type="submit" fluid="sm">
                        Send
                    </b-button>
                </b-row>
            </b-form>
        </b-container>
    </b-container>
</template>

<script>
    export default {
        name: "Contact",
        methods: {
            submitForm(e, values){
                console.log({ e, values })
            }
        }
    }
</script>

<style lang="css" scoped>
    .container {
        z-index: 3;
        width: 100vw;
    }
    @media screen and (max-width: 480px) {
		.container {
            padding-left: 0;
            padding-right: 0;
        }
	}
    .container > * {
        width: 100%;
        max-width: 35rem;
    }
    .form {
        background-color: #0e1a2b;
        border-radius: 5px;
    }
    .header {
        margin-bottom: 3rem;
    }
    .header h3 {
        color: #ffffff;
        text-transform: uppercase;
    }
    .header p {
        color: #c1d1e8; 
        font-size: 1rem;
    }

    label {
        font-size: .9rem;
    }
</style>
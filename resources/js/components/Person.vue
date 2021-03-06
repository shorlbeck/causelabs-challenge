<template>
    <div class="card">
        <div class="card-header">
            &bull; {{ person.first_name }} {{ person.last_name }}
        </div>

        <div class="card-body">
            <ul v-if="!editable">
                <li>First Name: {{ person.first_name }}</li>
                <li>Last Name: {{ person.last_name }}</li>
                <li>Age: {{ person.age }}</li>
                <li>Email: {{ person.email }}</li>
                <!-- Ordinarily I wouldn't display another person's `secret`
                     decoded in plain text, but I just wanted to show
                     that I recognized it as base64 encoding.
                     And yes, I followed instructions. See migration files. ;) -->
                <li>Secret: {{ secret_decoded }}</li>
            </ul>

            <ul v-if="editable">
                <div class="form-row">
                    <label class="form-label">First Name:</label>
                    <input class="form-input" type="text" name="first_name" @input="input(person.vue_key, 'first_name', $event.target.value)">
                </div>
                <div class="form-row">
                    <label class="form-label">Last Name:</label>
                    <input class="form-input" type="text" name="last_name" @input="input(person.vue_key, 'last_name', $event.target.value)">
                </div>
                <div class="form-row">
                    <label class="form-label">Age:</label>
                    <input class="form-input" type="number" min="0" max="140" name="age" @input="input(person.vue_key, 'age', $event.target.value)">
                </div>
                <div class="form-row">
                    <label class="form-label">Email:</label>
                    <input class="form-input" type="text" name="email" @input="input(person.vue_key, 'email', $event.target.value)">
                </div>
                <div class="form-row">
                    <label class="form-label">Secret:</label>
                    <input class="form-input" type="text" name="secret" @input="input(person.vue_key, 'secret', $event.target.value)">
                </div>
            </ul>
        </div>
    </div>
</template>

<script>
    export default {
        props: {
            person: {
                type: Object,
                required: true,
                default() {
                    return {
                        vue_key: null,
                        first_name: '',
                        last_name: '',
                        age: 0,
                        email: '',
                        secret: ''
                    }
                }
            },

            editable: {
                type: Boolean,
                required: false,
                default: true
            }
        },

        data() {
            return {
                personForm: {
                    first_name: this.person.first_name,
                    last_name: this.person.last_name,
                    age: this.person.age,
                    email: this.person.email,
                    secret: this.person.secret
                }
            }
        },

        computed: {
            secret_decoded() {
                return atob(this.person.secret);
            }
        },

        methods: {

            /**
             * Emit the 'input' event.
             * 
             * @value {Integer} vue_key  The vue_key that identifies the person
             * @value {String} attrib    The attribute name
             * @value {Mixed} value      The value to set
             * @return {Void}
             */
            input(vue_key, attrib, value)
            {
                this.$emit('input', { vue_key, attrib, value });
            }
        
        }
    }
</script>

<style lang="scss">
    .form-row, .form-label, .form-input { display: block }
    .form-row { margin-bottom: 1rem; }
    .form-input { width: 100% }
    .card-header { font-weight: bold; }
</style>
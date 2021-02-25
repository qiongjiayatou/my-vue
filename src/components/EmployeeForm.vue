<template>
    <div class="container">
        <form ref="first" @submit.prevent="onSubmit">
            <div class="form-group">
                <input v-model="employee.name"
                    :class="{ 'has-error': submitting && invalidName }"
                    @focus="clearStatus"
                    type="text" class="form-control" placeholder="Name">      
            </div>
            <div class="form-group">
                <input v-model="employee.email" 
                    :class="{ 'has-error': submitting && invalidEmail }"
                    @focus="clearStatus"
                    type="text" class="form-control" placeholder="Email">
            </div>

            <p v-if="error && submitting" class="error-message">
                ❗Please fill out all required fields
            </p>
            <p v-if="success" class="success-message">✅ Employee successfully added</p>

            <div class="form-group">
                <button class="btn btn-success">Add</button>
            </div>
        
        </form>
    </div>
    
</template>


<script>
export default {
    name: 'employee-form',
    data() {
        return {
            submitting: false,
            success: false,
            error: false,
            employee: {
                name: '',
                email: ''
            }
        }
    },
    methods: {
        onSubmit() {
            this.submitting = true;
            this.clearStatus();

            if (this.invalidName || this.invalidEmail) {
                this.error = true;
                return;
            }

            this.$emit('add:employee', this.employee);
            this.$refs.first.focus();

            this.employee = {
                name: '',
                email: ''
            }

            this.success = true;
            this.error = false;
            this.submitting = false;
            

        },

        clearStatus() {
            this.success = false;
            this.error = false;

            
        }
    },
    computed: {
        invalidName() {
            return this.employee.name === '';
        },

        invalidEmail() {
            return this.employee.email === '';
        }
    }
}
</script>

<style scoped>
  form {
    margin-bottom: 2rem;
  }

  [class*='-message'] {
    font-weight: 500;
  }

  .error-message {
    color: #d33c40;
  }

  .success-message {
    color: #32a95d;
  }
</style>
<template>
    <base-dialog v-if="inputIsInvalid" title="Invalid Input" @close="confirmError">
        <template #default>
            <p>Unfortunately,at least one input value is invalid.</p>
            <p>Please check all inputs and make sure you enter at least a few characters into each inputs</p>
        </template>

        <template #actions>
            <base-button @click="confirmError">OK</base-button>
        </template>
    </base-dialog>
    <base-card>
        <form @submit.prevent="submitData">
            <div class="form-control">
                <label for="title">Title</label>
                <input type="text" name="title" id="title" ref="title">
            </div>

            <div class="form-control">
                <label for="title">Description</label>
                <textarea rows="3" name="description" id="description" ref="desc"></textarea>
            </div>

            <div class="form-control">
                <label for="link">Link</label>
                <input type="text" name="link" id="link" ref="link">
            </div>

            <div>
                <base-button type="submit">Add Resource</base-button>
            </div>

        </form>
    </base-card>
</template>

<script>
export default {
    inject:['addResource'],
    data(){
        return{
            inputIsInvalid:false
        }
    },
    methods:{
        submitData(){
            const enteredData = {
                title:this.$refs.title.value,
                description:this.$refs.desc.value,
                link:this.$refs.link.value
            }

            if(
                enteredData.title.trim()==='' || 
                enteredData.description.trim() === '' || 
                enteredData.link.trim()===''
            ){    
                this.inputIsInvalid = true;
                return;
            }

            this.addResource(enteredData);

        },
        confirmError(){
            this.inputIsInvalid = false;
        }
    }
}
</script>

<style scoped>
label {
  font-weight: bold;
  display: block;
  margin-bottom: 0.5rem;
}

input,
textarea {
  display: block;
  width: 100%;
  font: inherit;
  padding: 0.15rem;
  border: 1px solid #ccc;
}

input:focus,
textarea:focus {
  outline: none;
  border-color: #3a0061;
  background-color: #f7ebff;
}

.form-control {
  margin: 1rem 0;
}
</style>


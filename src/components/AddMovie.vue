<template>
    <BaseModal :isOpen="isOpen">
        <template #heading>
            <h2 class="text-3xl font-semibold my-4 border-b px-4 pb-3">Add New Movie</h2>
        </template>
        <template #form>
            <form @submit.prevent="onSubmit" class="p-4">
                <div class="flex flex-col">
                    <label for="name" class="font-bold">Name:</label>
                    <input type="text" class="border border-gray-300 rounded    py-1" v-model="formData.name"
                        @blur="!formData.name ? errorFields.name = 'Can\'t be empty' : errorFields.name = ''" />
                    <span class="text-red-700">{{ errorFields.name }}</span>
                </div>
                <div class="flex flex-col">
                    <label for="description" class="font-bold">Description:</label>
                    <textarea type="text" class="border border-gray-300 rounded py-1" v-model="formData.description"
                        @blur="!formData.description ? errorFields.description = 'Can\'t be empty' : errorFields.description = ''"></textarea>
                    <span class="text-red-700">{{ errorFields.description }}</span>
                </div>
                <div class="flex flex-col">
                    <label for="image" class="font-bold">Image:</label>
                    <input type="text" class="border border-gray-300 rounded py-1" v-model="formData.image"
                        @blur="!formData.image ? errorFields.image = 'Can\'t be empty' : errorFields.image = ''" />
                    <span class="text-red-700">{{ errorFields.image }}</span>
                </div>
                <div class="flex flex-col">
                    <label for="genres" class="font-bold">Genres:</label>
                    <select class="border border-gray-300 rounded py-1" v-model="formData.genres"
                        @blur="formData.genres.length === 0 ? errorFields.genres = 'Can\'t be empty' : errorFields.genres = ''"
                        multiple>
                        <option v-for="option in options" :value="option.value" :key="option.value">{{ option.text }}
                        </option>
                    </select>
                    <span class="text-red-700">{{ errorFields.genres }}</span>

                </div>
                <div class="flex place-items-center my-4">
                    <input type="checkbox" class="border border-gray-300 rounded py-1" v-model="formData.inTheaters" />
                    <label for="inTheaters" class=" font-bold ml-4 ">Is Theater:</label>
                </div>

                <div class="flex place-content-between">
                    <button type="reset"
                        class="bg-gray-600 text-white px-4 py-2  rounded transition-all active:scale-90"
                        @click="resetForm">Cancel</button>
                    <button type="submit"
                        class="bg-blue-700 text-white px-4 py-2 rounded transition-all active:scale-90">Submit</button>
                </div>
            </form>
        </template>
    </BaseModal>
</template>

<script setup>
import { ref } from "vue";
import BaseModal from './utilities/BaseModal.vue';

const props = defineProps({
    isOpen: {
        type: Boolean,
        required: true
    }
});
const formData = ref({
    name: '',
    description: '',
    image: '',
    genres: [],
    inTheaters: false,
});

const errorFields = ref({
    name: '',
    description: '',
    image: '',
    genres: '',
});

const options = ref([
    { text: 'HipPop', value: 'hippop' },
    { text: 'Afro', value: 'afro' },
    { text: 'regea', value: 'Regea' }
])

const emits = defineEmits(['closeModal', 'addmovie']);

const onSubmit = () => {
    if (isValidForm()) {
        emits('addmovie',formData.value)
        clearForm();
        emits('closeModal');
    }
}


const validateForm = () => {
    for (const key in formData.value) {
        if (Object.hasOwnProperty.call(formData.value, key)) {
            if (formData.value[key] === '') {
                errorFields.value[key] = 'Can\'t be empty';
            } else {
                errorFields.value[key] = '';
            }
        }
    }
}

const isValidForm = () => {
    validateForm();
    return Object.entries(errorFields.value).every(f => f[1] === '');
}

const clearForm = () => {
    for (const key in formData.value) {
        if (Object.hasOwnProperty.call(formData.value, key)) {
            if (formData.value[key] != '') {
                formData.value[key] = '';
            } 
        }
    }
}

const resetForm = () => {
    for (const key in errorFields.value) {
        if (Object.hasOwnProperty.call(errorFields.value, key)) {
            if (errorFields.value[key] != '') {
                errorFields.value[key] = '';
                clearForm();
            } 
        }
    }
}
</script>
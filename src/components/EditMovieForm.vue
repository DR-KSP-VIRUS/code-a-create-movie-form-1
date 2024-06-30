<template>
    <BaseModal :isOpen="isEdit">
        <template #heading>
            <h2 class="text-3xl font-semibold my-4 border-b px-4 pb-3">Edit {{ movie.name }}</h2>
        </template>
        <template #form>
            <form @submit.prevent="onUpdateMovie" class="p-4">
                <div class="flex flex-col">
                    <label for="name" class="font-bold">Name:</label>
                    <input type="text" class="border border-gray-300 rounded    py-1" v-model="movie.name"
                        @blur="!movie.name ? errorFields.name = 'Can\'t be empty' : errorFields.name = ''" />
                    <span class="text-red-700">{{ errorFields.name }}</span>
                </div>
                <div class="flex flex-col">
                    <label for="description" class="font-bold">Description:</label>
                    <textarea type="text" class="border border-gray-300 rounded py-1" v-model="movie.description"
                        @blur="!movie.description ? errorFields.description = 'Can\'t be empty' : errorFields.description = ''"></textarea>
                    <span class="text-red-700">{{ errorFields.description }}</span>
                </div>
                <div class="flex flex-col">
                    <label for="image" class="font-bold">Image:</label>
                    <input type="text" class="border border-gray-300 rounded py-1" v-model="movie.image"
                        @blur="!movie.image ? errorFields.image = 'Can\'t be empty' : errorFields.image = ''" />
                    <span class="text-red-700">{{ errorFields.image }}</span>
                </div>
                <div class="flex flex-col">
                    <label for="genres" class="font-bold">Genres:</label>
                    <select class="border border-gray-300 rounded py-1" v-model="movie.genres"
                        @blur="movie.genres.length === 0 ? errorFields.genres = 'Can\'t be empty' : errorFields.genres = ''"
                        multiple>
                        <option v-for="option in movie.genres" :value="option" :key="option">{{ option }}
                        </option>
                    </select>
                    <span class="text-red-700">{{ errorFields.genres }}</span>

                </div>
                <div class="flex place-items-center my-4">
                    <input type="checkbox" class="border border-gray-300 rounded py-1" v-model="movie.inTheaters" />
                    <label for="inTheaters" class=" font-bold ml-4 ">Is Theater:</label>
                </div>

                <div class="flex place-content-between">
                    <button type="reset"
                        class="bg-gray-600 text-white px-4 py-2  rounded transition-all active:scale-90"
                        @click="resetForm">Cancel</button>
                    <button type="submit"
                        class="bg-blue-700 text-white px-4 py-2 rounded transition-all active:scale-90">Update</button>
                </div>
            </form>
        </template>
    </BaseModal>
</template>

<script setup>
import { ref } from "vue";
import BaseModal from './utilities/BaseModal.vue';

const props = defineProps({
    isEdit: {
        type: Boolean,
        required: true
    },
    movie: {
        type: Object,
        required: true
    }
});


const errorFields = ref({
    name: '',
    description: '',
    image: '',
    genres: '',
});



const emits = defineEmits(['closeModal','updateMovie']);

const onUpdateMovie = () => {
        emits('updateMovie', props.movie)
        emits('closeModal');
}

const validateForm = () => {
    for (const key in props.movie) {
        if (Object.hasOwnProperty.call(props.movie, key)) {
            if (props.movie[key] === '') {
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
    for (const key in props.movie) {
        if (Object.hasOwnProperty.call(props.movie, key)) {
            if (props.movie[key] != '') {
                props.movie[key] = '';
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
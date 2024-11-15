<template>
    <div class="container mt-2 mb-4">
        <div class="d-flex justify-content-center">
            <h1>Create Book</h1>
        </div>
        <form @submit.prevent="handleCreateBook" @change="handleFileUpload">
            <div class="mb-3">
                <label for="title" class="form-label">Title:</label>
                <input type="text" v-model="book.title" class="form-control" required />
            </div>
            <div class="mb-3">
                <label for="price" class="form-label">Price:</label>
                <input type="number" v-model="book.price" class="form-control" required />
            </div>
            <div class="mb-3">
                <label for="quantity" class="form-label">Quantity:</label>
                <input type="number" v-model="book.quantity" class="form-control" required />
            </div>
            <div class="mb-3">
                <label for="publicationYear" class="form-label">Publication Year:</label>
                <input type="number" v-model="book.publicationYear" class="form-control" />
            </div>
            <div class="mb-3">
                <label for="publisher" class="form-label">Publisher:</label>
                <select name="publisher" id="publisher" class="form-select">
                    <option v-for="publisher in publishers" :value="publisher._id">
                        {{ publisher.name + " " + publisher.address }}</option>
                </select>
            </div>
            <div class="mb-3">
                <label for="author" class="form-label">Author:</label>
                <input type="text" v-model="book.author" class="form-control" />
            </div>
            <div class="mb-3">
                <label for="image" class="form-label">Image:</label>
                <input type="file" @change="handleFileUpload" class="form-control" />
            </div>
            <div class="mt-4">
                <button type="button" class="btn ms-3 btn-secondary fs-6 btn-sm ms-3"
                    @click.prevent="handleExit">Exit</button>
                <button type="submit" class="btn btn-primary btn-sm ms-3 fs-6">Update Book</button>
            </div>
        </form>
    </div>
</template>

<script>
import { onMounted, ref } from 'vue';
import { getAllPublishers } from '../../API/publisherApi';
import { createBook, uploadBookImage } from '../../API/bookApi';
import router from '../../router';

export default {
    setup() {
        const book = ref({
            title: '',
            price: 0,
            quantity: 0,
            publicationYear: new Date().getFullYear(),
            publisher: '',
            author: '',
            image: null,
        });
        const publishers = ref([]);

        const handleFileUpload = (event) => {
            const files = event.target.files;
            if (files && files.length > 0) {
                book.value.image = files[0].name;
                book.value.file = files[0];
            }
        };

        const handleCreateBook = async () => {
            const data = {
                title: book.value.title,
                price: book.value.price,
                quantity: book.value.quantity,
                image: book.value.image,
                file: book.value.file,
            }
            const result = await createBook(data);
            await uploadBookImage(data.file);
            console.dir(result);
        };

        const handleExit = () => {
            router.push('/books');
        };

        onMounted(async () => {
            publishers.value = await getAllPublishers();
            console.log(publishers);
        });

        return {
            handleCreateBook,
            handleFileUpload,
            book,
            handleExit,
            publishers,
        };
    },
};
</script>

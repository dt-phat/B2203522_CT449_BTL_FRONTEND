<template>
    <div class="card book-card">
        <img :src="'http://localhost:3000/uploads/' + book.image" class="card-img-top" alt="Author Image" />
        <div class="card-body p-2">
            <h5 class="card-title text-primary fw-bold fs-5">
                <router-link :to="`/books/${book._id}`" class="no-underline">{{ book.title }}</router-link>
            </h5>
            <p class="card-text"><strong>Author:</strong> {{ book.author }}</p>
            <p class="card-text"><strong>Price:</strong> {{ book.price }}</p>
            <p class="card-text"><strong>Quantity:</strong> {{ book.quantity }}</p>
        </div>
        <hr>
        <div class="d-flex justify-content-end mt-2">
            <router-link class="btn btn-danger btn-sm fs-6 fw-normal"
                v-if="(user?.role === 'admin') || (user?.role === 'employee')"
                :to="{ name: 'edit-book', params: { id: book._id } }">Edit</router-link>
            <router-link class="btn btn-primary btn-sm fs-6 fw-normal"
                :to="{ name: 'bookdetails', params: { id: book._id } }">Details</router-link>
            <button :disabled="book.quantity < 1" class="btn btn-borrow btn-sm fs-6 fw-normal"
                @click="handleBorrow">Borrow</button>
        </div>
    </div>
</template>

<script>
import { useAuthStore } from '../stores/authStore';
import { createBorrowings } from '../API/borrowingApi';
export default {
    props: {
        book: Object,
    },
    setup(props) {
        const authStore = useAuthStore();
        const user = authStore.user;
        const handleBorrow = async () => {
            if (confirm(`Do you want to order "${props.book.title}"?`)) {
                await createBorrowings({ book: props.book._id });
                window.location.reload();
            }
        };
        return {
            user,
            handleBorrow,
        };
    },
}
</script>

<style scoped>
.book-card {
    width: 18rem;
    height: 375px;
    overflow: hidden;
}

.card-title {
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
    text-overflow: ellipsis;
    height: 3em;
    margin-bottom: 0;

}

.no-underline {
    text-decoration: none;
}

.card-title:hover {
    text-decoration: underline;
    cursor: pointer;
}

.card-img-top {
    height: 150px;
    object-fit: cover;
}

.card-body {
    flex: none;
}

.card-text {
    margin-bottom: 5px;
}

.btn {
    margin-right: 10px;
}

.btn-borrow {
    color: white;
    background-color: #6610f2;
}

.btn-borrow:hover {
    background-color: #b400b4;
}

.btn:last-child {
    margin-right: 10px;
}

button:disabled {
    background-color: #ccc;
    color: #666;
    cursor: not-allowed;
    opacity: 0.6;
}

hr {
    margin: 0;
    margin-bottom: 5px;
}
</style>

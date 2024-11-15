<template>
    <div class="container row align-items-center border rounded mt-2">
        <div class="col-md-2">
            <img :src="'http://localhost:3000/uploads/' + borrowing.book.image" alt="Book Image"
                class="img-fluid rounded">
        </div>
        <div class="col-md-4">
            <h5 class="mb-0">{{ borrowing.book.title }}</h5>
            <p class="mb-0 mt-2" v-if="borrowing.returnDate"><strong>Return Date:</strong> {{ borrowing.returnDate ?
                formatDate(borrowing.returnDate) :
                'Not returned yet' }}</p>

            <p class="mb-0 mt-2" v-else-if="borrowing.borrowDate"><strong>Borrow Date:</strong> {{ borrowing.borrowDate
                ?
                formatDate(borrowing.borrowDate) :
                'Not borrowed yet' }}</p>
            <p class="mb-0 mt-2" v-else><strong>Request Date:</strong> {{
                formatDate(borrowing.requestDate) }}</p>

        </div>
        <div class="col-md-4 text-center">
            <p class="mb-0 fs-6">{{ (borrowing.status).charAt(0).toUpperCase() + (borrowing.status).slice(1) }}</p>
        </div>
        <div class="col-md-2 text-end">
            <router-link :to="{ name: 'bookdetails', params: { id: borrowing.book._id } }" class="btn btn-primary"
                @click="viewDetails">View Book</router-link>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        borrowing: {
            type: Object,
            required: true
        }
    },
    methods: {
        formatDate(date) {
            return new Date(date).toLocaleDateString();
        },
    }
};
</script>

<style scoped>
.container {
    background-color: #fafafa;
}
</style>
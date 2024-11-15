<template>
    <div class="container">
        <!-- Navbar -->
        <div class="d-flex justify-content-center align-items-center">
            <div class="navbar-brand text-dark fw-normal fs-2 fw-bold">Borrowed Books</div>
            <ul class="navbar-nav ms-auto d-flex flex-row m-auto">
                <li class="nav-item">
                    <button class="nav-link text-primary fs-6 fw-normal" :class="{ active: activeItem === 'all' }"
                        @click="setActive('all')">All</button>
                </li>
                <li class="nav-item">
                    <button class="nav-link text-primary fs-6 fw-normal"
                        :class="{ active: activeItem === 'processing' }"
                        @click="setActive('processing')">Requested</button>
                </li>
                <li class="nav-item">
                    <button class="nav-link text-primary fs-6 fw-normal" :class="{ active: activeItem === 'borrowed' }"
                        @click="setActive('borrowed')">Borrowed</button>
                </li>
                <li class="nav-item">
                    <button class="nav-link text-primary fs-6 fw-normal" :class="{ active: activeItem === 'returned' }"
                        @click="setActive('returned')">Returned</button>
                </li>
            </ul>
        </div>
        <div v-if="borrowings.length" class="mt-2">
            <BorrowedBookCard v-for="borrowing in borrowings" :borrowing="borrowing" class="borrow-card" />
        </div>
        <div v-else class="alert alert-dark text-center bg-white mt-2">
            <p class="fs-6">No books have been borrowed.</p>
        </div>
    </div>
</template>

<script>
import BorrowedBookCard from "../../components/BorrowedBookCard.vue";
import { ref, onMounted } from "vue";
import { getCurrentBorrowings } from "../../API/borrowingApi";
export default {
    components: {
        BorrowedBookCard,
    },
    setup() {
        const allBorrowings = ref([]);
        const borrowings = ref([]);
        const activeItem = ref('all');
        onMounted(async () => {
            allBorrowings.value = await getCurrentBorrowings();
            borrowings.value = allBorrowings.value;
        });
        const setActive = (value) => {
            activeItem.value = value;
            if (value === 'all') {
                borrowings.value = allBorrowings.value;
            } else {
                borrowings.value = allBorrowings.value.filter(borrowing => borrowing.status === value);
            }
        }
        return {
            borrowings,
            setActive,
            activeItem,
        };
    },
};
</script>

<style scoped>
.navbar-brand {
    color: #4921f3 !important;
}

.nav-link {
    width: 120px;
    border-radius: 10px;
    font-weight: bolder !important;
}

.active {
    background-color: #0d6efd !important;
    color: white !important;
}

.navbar-nav .nav-link:hover {
    font-weight: bold !important;
    font-size: 1.1em;
    margin: 0px !important;
}
</style>

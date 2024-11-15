<template>
    <div class="profile-container">
        <h1>User Profile</h1>
        <div class="profile-item">
            <label>Username:</label>
            <span>{{ user.username }}</span>
        </div>
        <div class="profile-item">
            <label>Email:</label>
            <span>{{ user.email }}</span>
        </div>
        <div class="profile-item">
            <label>Role:</label>
            <span>{{ user.role }}</span>
        </div>
        <div class="profile-item">
            <label>Full name:</label>
            <span>{{ user.firstname + ' ' + user.lastname }}</span>
        </div>
        <div class="profile-item">
            <label>Gender:</label>
            <span>{{ user.gender }}</span>
        </div>
        <div class="profile-item">
            <label>Position:</label>
            <span>{{ user.position }}</span>
        </div>
        <div class="profile-item">
            <label>Address:</label>
            <span>{{ user.address }}</span>
        </div>
        <div class="profile-item">
            <label>Phone Number:</label>
            <span>{{ user.phoneNumber }}</span>
        </div>
        <button class="edit-button" @click="editProfile">Edit Profile</button>
    </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import { useAuthStore } from '../../stores/authStore';
import { useRouter } from 'vue-router';
import { showMe } from '../../API/userApi';

export default {
    setup() {
        const authStore = useAuthStore();
        const user = ref({});
        const router = useRouter();

        const editProfile = () => {
            router.push('/edit-profile');
        };

        onMounted(async () => {
            user.value = (await showMe()).data.user;
        });

        return {
            user,
            editProfile,
        };
    },
};
</script>

<style scoped>
.profile-container {
    max-width: 600px;
    margin: 0 auto;
    padding: 20px;
    background-color: #f8f9fa;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.profile-item {
    display: flex;
    justify-content: space-between;
    margin-bottom: 10px;
}

.profile-item label {
    font-size: 18px;
    font-weight: bold;
    color: #343a40;
}

.profile-item span {
    font-size: 18px;
    color: #495057;
}

h1 {
    text-align: center;
    color: #343a40;
    margin-bottom: 20px;
}

.edit-button {
    display: block;
    width: 100%;
    padding: 10px;
    background-color: #900090;
    color: #ffffff;
    border: none;
    border-radius: 4px;
    font-size: 16px;
    cursor: pointer;
    text-align: center;
    margin-top: 20px;
}

.edit-button:hover {
    background-color: #803080;
}
</style>

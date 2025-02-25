<template>
    <div class="max-w-4xl mx-auto my-8 p-6 bg-white shadow-lg rounded-lg">
        <h2 class="text-3xl font-bold text-center mb-6 text-blue-700">Student Records</h2>

        <div v-if="!isAuthenticated" class="text-center mb-4">
            <input v-model="password" type="password" placeholder="Enter Password" 
                   class="border p-2 rounded-md w-full sm:w-auto focus:outline-none focus:ring focus:border-blue-300">
            <button @click="checkPassword" class="bg-blue-500 hover:bg-blue-700 text-white px-4 py-2 rounded mt-2 sm:mt-0">Submit</button>
        </div>

        <div class="mb-4 flex flex-col sm:flex-row justify-between items-center">
            <label class="font-bold">Select Class:</label>
            <select v-model="selectedClass" class="border p-2 rounded w-full sm:w-1/3 focus:outline-none focus:ring focus:border-blue-300">
                <option value="">All</option>
                <option v-for="classItem in allClasses" :key="classItem" :value="classItem">
                    {{ classItem }}
                </option>
            </select>
        </div>

        <div v-if="isAuthenticated" class="text-right mb-4">
            <button @click="openAddModal" class="bg-green-500 hover:bg-green-700 text-white px-4 py-2 rounded w-full sm:w-auto">
                Add Student
            </button>
        </div>

        <div class="overflow-x-auto">
            <table class="w-full border-collapse border rounded-lg shadow-lg text-sm sm:text-base">
                <thead>
                    <tr class="bg-blue-600 text-white">
                        <th class="border p-2 sm:p-4">Student Name</th>
                        <th class="border p-2 sm:p-4">Subject</th>
                        <th class="border p-2 sm:p-4">Number</th>
                        <th class="border p-2 sm:p-4">Class</th>
                        <th class="border p-2 sm:p-4">Class Teacher</th>
                        <th class="border p-2 sm:p-4">Attendance</th>
                        <th v-if="isAuthenticated" class="border p-2 sm:p-4">Actions</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(user, index) in filteredUsers" :key="index" class="odd:bg-gray-100 even:bg-white">
                        <td class="border p-2 sm:p-4">{{ user.name }}</td>
                        <td class="border p-2 sm:p-4">{{ user.subject }}</td>
                        <td class="border p-2 sm:p-4 font-semibold">{{ user.score }}</td>
                        <td class="border p-2 sm:p-4">{{ user.class }}</td>
                        <td class="border p-2 sm:p-4">{{ user.teacher }}</td>
                        <td class="border p-2 sm:p-4">
                            <span :class="user.score >= 30 ? 'text-green-600 font-bold' : 'text-red-600 font-bold'">
                                {{ user.score >= 30 ? 'Pass' : 'Fail' }}
                            </span>
                        </td>
                        <td v-if="isAuthenticated" class="border p-2 sm:p-4">
                            <button @click="editStudent(index)" class="bg-yellow-500 hover:bg-yellow-700 text-white px-2 py-1 rounded mr-1 sm:px-3 sm:py-1">Edit</button>
                            <button @click="deleteStudent(index)" class="bg-red-500 hover:bg-red-700 text-white px-2 py-1 rounded sm:px-3 sm:py-1">Delete</button>
                        </td>
                    </tr>
                </tbody>
            </table>
            <p class="mt-2 text-gray-400 text-center">Created by Aman Sharma</p>
        </div>

        <div v-if="showModal" class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50 p-4">
            <div class="bg-white p-6 rounded shadow-lg w-full max-w-md">
                <h3 class="text-xl font-bold mb-4">{{ isEditing ? 'Edit Student' : 'Add Student' }}</h3>

                <input v-model="student.name" type="text" placeholder="Student Name" class="border p-2 w-full mb-2">
                <input v-model="student.subject" type="text" placeholder="Subject" class="border p-2 w-full mb-2">
                <input v-model="student.score" type="number" placeholder="Score" class="border p-2 w-full mb-2">
                <select v-model="student.class" class="border p-2 w-full mb-2">
                    <option v-for="classItem in allClasses" :key="classItem" :value="classItem">{{ classItem }}</option>
                </select>
                <input v-model="student.teacher" type="text" placeholder="Class Teacher" class="border p-2 w-full mb-2">

                <div class="flex flex-col sm:flex-row justify-end mt-4">
                    <button @click="closeModal" class="bg-gray-400 hover:bg-gray-500 text-white px-4 py-2 rounded mb-2 sm:mb-0 sm:mr-2 w-full sm:w-auto">Cancel</button>
                    <button @click="saveStudent" class="bg-blue-500 hover:bg-blue-700 text-white px-4 py-2 rounded w-full sm:w-auto">
                        {{ isEditing ? 'Update' : 'Add' }}
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Student',
    data() {
        return {
            users: JSON.parse(localStorage.getItem("students")) || [],
            isAuthenticated: false,
            password: "",
            showModal: false,
            isEditing: false,
            student: { name: "", score: "", class: "", teacher: "", subject: ""},
            editIndex: null,
            selectedClass: "",
            allClasses: ["Nursery", "UKG", "1st", "2nd", "3rd", "4th", "5th", "6th", "7th", "8th", "9th", "10th"]
        };
    },
    computed: {
        filteredUsers() {
            return this.selectedClass ? this.users.filter(user => user.class === this.selectedClass) : this.users;
        }
    },
    methods: {
        checkPassword() {
            if (this.password === "aman123") {
                this.isAuthenticated = true;
            } else {
                alert("Incorrect Password!");
            }
        },
        openAddModal() {
            this.showModal = true;
            this.isEditing = false;
            this.student = { name: "", score: "", class: "", teacher: "", subject: "" };
        },
        editStudent(index) {
            this.editIndex = index;
            this.student = { ...this.users[index] };
            this.isEditing = true;
            this.showModal = true;
        },
        saveStudent() {
            if (!this.student.name || !this.student.score || !this.student.class || !this.student.teacher) {
                alert("All fields are required!");
                return;
            }
            if (this.isEditing) {
                this.users[this.editIndex] = { ...this.student };
            } else {
                this.users.push({ ...this.student });
            }
            this.saveToLocalStorage();
            this.closeModal();
        },
        deleteStudent(index) {
            if (confirm("Are you sure you want to delete this student?")) {
                this.users.splice(index, 1);
                this.saveToLocalStorage();
            }
        },
        saveToLocalStorage() {
            localStorage.setItem("students", JSON.stringify(this.users));
        },
        closeModal() {
            this.showModal = false;
        }
    }
};
</script>

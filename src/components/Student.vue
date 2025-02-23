<template>
    <div class="max-w-4xl mx-auto my-8">
        <h2 class="text-3xl font-bold text-center mb-6">Student Records</h2>

        <!-- Password Input -->
        <div v-if="!isAuthenticated" class="text-center mb-4">
            <input v-model="password" type="password" placeholder="Enter Password" 
                   class="border p-2 rounded-md mr-2">
            <button @click="checkPassword" class="bg-blue-500 text-white px-4 py-2 rounded">Submit</button>
        </div>

        <!-- Add Student Button (Only if Authenticated) -->
        <div v-if="isAuthenticated" class="text-right mb-4">
            <button @click="showAddModal = true" class="bg-green-500 text-white px-4 py-2 rounded">
                Add Student
            </button>
        </div>

        <div class="overflow-x-auto">
            <table class="w-full border-collapse border rounded-lg shadow-lg">
                <thead>
                    <tr class="bg-blue-600 text-white">
                        <th class="border p-4">Student Name</th>
                        <th class="border p-4">Number</th>
                        <th class="border p-4">Class</th>
                        <th class="border p-4">Class Teacher</th>
                        <th class="border p-4">Attendance</th>
                        <th v-if="isAuthenticated" class="border p-4">Actions</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(user, index) in users" :key="index" 
                        :class="{'bg-gray-100': index % 2 === 0, 'bg-white': index % 2 !== 0}">
                        <td class="border p-4">{{ user.name }}</td>
                        <td class="border p-4 font-semibold">{{ user.score }}</td>
                        <td class="border p-4">{{ user.class }}</td>
                        <td class="border p-4">{{ user.teacher }}</td>
                        <td class="border p-4">
                            <span :class="user.score >= 30 ? 'text-green-600 font-bold' : 'text-red-600 font-bold'">
                                {{ user.score >= 30 ? 'Pass' : 'Fail' }}
                            </span>
                        </td>
                        <td v-if="isAuthenticated" class="border p-4">
                            <button @click="editStudent(index)" class="bg-yellow-500 text-white px-3 py-1 rounded mr-2">Edit</button>
                            <button @click="deleteStudent(index)" class="bg-red-500 text-white px-3 py-1 rounded">Delete</button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>

        <!-- Add/Edit Modal -->
        <div v-if="showAddModal || showEditModal" class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50">
            <div class="bg-white p-6 rounded shadow-lg w-96">
                <h3 class="text-xl font-bold mb-4">{{ showEditModal ? 'Edit Student' : 'Add Student' }}</h3>

                <input v-model="student.name" type="text" placeholder="Student Name" class="border p-2 w-full mb-2">
                <input v-model="student.score" type="number" placeholder="Score" class="border p-2 w-full mb-2">
                <input v-model="student.class" type="text" placeholder="Class" class="border p-2 w-full mb-2">
                <input v-model="student.teacher" type="text" placeholder="Class Teacher" class="border p-2 w-full mb-2">

                <div class="flex justify-end mt-4">
                    <button @click="showAddModal = showEditModal = false" class="bg-gray-400 text-white px-4 py-2 rounded mr-2">Cancel</button>
                    <button @click="saveStudent" class="bg-blue-500 text-white px-4 py-2 rounded">
                        {{ showEditModal ? 'Update' : 'Add' }}
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
            users: JSON.parse(localStorage.getItem("students")) || [
                { name: 'Aman', score: 99, class: '1st', teacher: 'Sakshi' },
                { name: 'Vishal', score: 30, class: '10th', teacher: 'Saloni' },
                { name: 'Sachin', score: 26, class: '9th', teacher: 'Priti' },
                { name: 'Rahul', score: 17, class: '5th', teacher: 'Aman' },
                { name: 'Neha', score: 35, class: '2nd', teacher: 'Sakshi' },
            ],
            isAuthenticated: false,
            password: "",
            showAddModal: false,
            showEditModal: false,
            student: { name: "", score: "", class: "", teacher: "" },
            editIndex: null
        };
    },
    methods: {
        checkPassword() {
            if (this.password === "aman123") {
                this.isAuthenticated = true;
            } else {
                alert("Incorrect Password!");
            }
        },
        addStudent() {
            this.showAddModal = true;
            this.student = { name: "", score: "", class: "", teacher: "" };
        },
        editStudent(index) {
            this.editIndex = index;
            this.student = { ...this.users[index] };
            this.showEditModal = true;
        },
        saveStudent() {
            if (this.student.name && this.student.score && this.student.class && this.student.teacher) {
                if (this.showEditModal) {
                    this.users[this.editIndex] = { ...this.student };
                } else {
                    this.users.push({ ...this.student });
                }
                this.saveToLocalStorage();
                this.showAddModal = this.showEditModal = false;
            } else {
                alert("All fields are required!");
            }
        },
        deleteStudent(index) {
            if (confirm("Are you sure you want to delete this student?")) {
                this.users.splice(index, 1);
                this.saveToLocalStorage();
            }
        },
        saveToLocalStorage() {
            localStorage.setItem("students", JSON.stringify(this.users));
        }
    }
};
</script>
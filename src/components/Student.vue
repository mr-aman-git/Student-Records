<!-- 

<template>
    <div class="max-w-4xl mx-auto my-8">
        <h2 class="text-3xl font-bold text-center mb-6">Student Records</h2>

        <div v-if="!isAuthenticated" class="text-center mb-4">
            <input v-model="password" type="password" placeholder="Enter Password" 
                   class="border p-2 rounded-md mr-2">
            <button @click="checkPassword" class="bg-blue-500 text-white px-4 py-2 rounded">Submit</button>
        </div>

        <div class="mb-4">
            <label class="font-bold">Select Class:</label>
            <select v-model="selectedClass" class="border p-2 rounded ml-2">
                <option value="">All</option>
                <option v-for="classItem in uniqueClasses" :key="classItem" :value="classItem">
                    {{ classItem }}
                </option>
            </select>
        </div>

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
                        <th class="border p-4">Result</th>
                        <th v-if="isAuthenticated" class="border p-4">Actions</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(user, index) in filteredUsers" :key="index" 
                        :class="{'bg-gray-100': index % 2 === 0, 'bg-white': index % 2 !== 0}">
                        <td class="border p-4">{{ user.name }}</td>
                        <td class="border p-4 font-semibold">{{ user.score }}</td>
                        <td class="border p-4">{{ user.class }}</td>
                        <td class="border p-4">{{ user.teacher }}</td>
                        <td class="border p-4">
                            <span :class="user.score >= 15 ? 'text-green-600 font-bold' : 'text-red-600 font-bold'">
                                {{ user.score >= 15 ? 'Pass' : 'Fail' }}
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

        <div v-if="showAddModal || showEditModal" class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50">
            <div class="bg-white p-6 rounded shadow-lg w-96">
                <h3 class="text-xl font-bold mb-4">{{ showEditModal ? 'Edit Student' : 'Add Student' }}</h3>

                <input v-model="student.name" type="text" placeholder="Student Name" class="border p-2 w-full mb-2">
                <input v-model="student.score" type="number" placeholder="Score" class="border p-2 w-full mb-2">
                <select v-model="student.class" class="border p-2 w-full mb-2">
                    <option v-for="classItem in uniqueClasses" :key="classItem" :value="classItem">{{ classItem }}</option>
                </select>
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
                { name: '', score: null, class: '', teacher: '' },
            ],
            isAuthenticated: false,
            password: "",
            showAddModal: false,
            showEditModal: false,
            student: { name: "", score: "", class: "", teacher: "" },
            editIndex: null,
            selectedClass: ""
        };
    },
    computed: {
        uniqueClasses() {
            return [...new Set(this.users.map(user => user.class))];
        },
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
</script> -->


<!-- <template>
    <div class="max-w-4xl mx-auto my-8">
        <h2 class="text-3xl font-bold text-center mb-6">Student Records</h2>

        <div v-if="!isAuthenticated" class="text-center mb-4">
            <input v-model="password" type="password" placeholder="Enter Password" 
                   class="border p-2 rounded-md mr-2">
            <button @click="checkPassword" class="bg-blue-500 text-white px-4 py-2 rounded">Submit</button>
        </div>

        <div class="mb-4">
            <label class="font-bold">Select Class:</label>
            <select v-model="selectedClass" class="border p-2 rounded ml-2">
                <option value="">All</option>
                <option v-for="classItem in allClasses" :key="classItem" :value="classItem">
                    {{ classItem }}
                </option>
            </select>
        </div>

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
                    <tr v-for="(user, index) in filteredUsers" :key="index" 
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

        <div v-if="showAddModal || showEditModal" class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50">
            <div class="bg-white p-6 rounded shadow-lg w-96">
                <h3 class="text-xl font-bold mb-4">{{ showEditModal ? 'Edit Student' : 'Add Student' }}</h3>

                <input v-model="student.name" type="text" placeholder="Student Name" class="border p-2 w-full mb-2">
                <input v-model="student.score" type="number" placeholder="Score" class="border p-2 w-full mb-2">
                <select v-model="student.class" class="border p-2 w-full mb-2">
                    <option v-for="classItem in allClasses" :key="classItem" :value="classItem">{{ classItem }}</option>
                </select>
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
        }
    }
};
</script> -->


<template>
    <div class="max-w-4xl mx-auto my-8 p-6 bg-white shadow-lg rounded-lg">
        <h2 class="text-3xl font-bold text-center mb-6 text-blue-700">Student Records</h2>

        <!-- Password Input -->
        <div v-if="!isAuthenticated" class="text-center mb-4">
            <input v-model="password" type="password" placeholder="Enter Password" 
                   class="border p-2 rounded-md mr-2 focus:outline-none focus:ring focus:border-blue-300">
            <button @click="checkPassword" class="bg-blue-500 hover:bg-blue-700 text-white px-4 py-2 rounded">Submit</button>
        </div>

        <!-- Class Selection -->
        <div class="mb-4 flex justify-between items-center">
            <label class="font-bold">Select Class:</label>
            <select v-model="selectedClass" class="border p-2 rounded w-1/3 focus:outline-none focus:ring focus:border-blue-300">
                <option value="">All</option>
                <option v-for="classItem in allClasses" :key="classItem" :value="classItem">
                    {{ classItem }}
                </option>
            </select>
        </div>

        <!-- Add Student Button (Only if Authenticated) -->
        <div v-if="isAuthenticated" class="text-right mb-4">
            <button @click="openAddModal" class="bg-green-500 hover:bg-green-700 text-white px-4 py-2 rounded">
                Add Student
            </button>
        </div>

        <div class="overflow-x-auto">
            <table class="w-full border-collapse border rounded-lg shadow-lg">
                <thead>
                    <tr class="bg-blue-600 text-white">
                        <th class="border p-4">Student Name</th>
                        <th class="border p-4">Subject</th>
                        <th class="border p-4">Number</th>
                        <th class="border p-4">Class</th>
                        <th class="border p-4">Class Teacher</th>
                        <th class="border p-4">Attendance</th>
                        <th v-if="isAuthenticated" class="border p-4">Actions</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(user, index) in filteredUsers" :key="index" class="odd:bg-gray-100 even:bg-white">
                        <td class="border p-4">{{ user.name }}</td>
                        <td class="border p-4">{{ user.subject }}</td>
                        <td class="border p-4 font-semibold">{{ user.score }}</td>
                        <td class="border p-4">{{ user.class }}</td>
                        <td class="border p-4">{{ user.teacher }}</td>
                        <td class="border p-4">
                            <span :class="user.score >= 30 ? 'text-green-600 font-bold' : 'text-red-600 font-bold'">
                                {{ user.score >= 30 ? 'Pass' : 'Fail' }}
                            </span>
                        </td>
                        <td v-if="isAuthenticated" class="border p-4">
                            <button @click="editStudent(index)" class="bg-yellow-500 hover:bg-yellow-700 text-white px-3 py-1 rounded mr-2">Edit</button>
                            <button @click="deleteStudent(index)" class="bg-red-500 hover:bg-red-700 text-white px-3 py-1 rounded">Delete</button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>

        <!-- Add/Edit Modal -->
        <div v-if="showModal" class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50">
            <div class="bg-white p-6 rounded shadow-lg w-96">
                <h3 class="text-xl font-bold mb-4">{{ isEditing ? 'Edit Student' : 'Add Student' }}</h3>

                <input v-model="student.name" type="text" placeholder="Student Name" class="border p-2 w-full mb-2">
                <input v-model="student.subject" type="text" placeholder="Subject" class="border p-2 w-full mb-2">
                <input v-model="student.score" type="number" placeholder="Score" class="border p-2 w-full mb-2">
                <select v-model="student.class" class="border p-2 w-full mb-2">
                    <option v-for="classItem in allClasses" :key="classItem" :value="classItem">{{ classItem }}</option>
                </select>
                <input v-model="student.teacher" type="text" placeholder="Class Teacher" class="border p-2 w-full mb-2">

                <div class="flex justify-end mt-4">
                    <button @click="closeModal" class="bg-gray-400 hover:bg-gray-500 text-white px-4 py-2 rounded mr-2">Cancel</button>
                    <button @click="saveStudent" class="bg-blue-500 hover:bg-blue-700 text-white px-4 py-2 rounded">
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


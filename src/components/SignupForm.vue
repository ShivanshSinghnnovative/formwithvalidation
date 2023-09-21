<template>
    <div class="form">
        <h3>EMAIL:</h3>
        <input type="text" v-model="email" @keydown="emailWrite" placeholder="Enter Email">
        <div v-if="errorMessages.length > 0 && email.length == 0" class="error">
            <p>Mail field is required *</p>
        </div>
        <div v-if="errorMessages.length > 0 && email.length != 0 && validEmail" class="error">
            <p>Mail is in email format*</p>
        </div>

        <h3>PASSWORD:</h3>
        <input type="text" v-model="password" @keydown="passwordWrite" placeholder="Enter Password">

        <div v-if="errorMessages.length > 0 && password.length == 0" class="error">
            <p>password field is required *</p>

        </div>

        <div v-if="errorMessages.length > 0 && password.length != 0 && passwordisValid" class="error">
            <p>Password must be at least 8 characters long and contain at least 1 special character, 1 number, 1 uppercase
                letter, and 1 lowercase letter *</p>
        </div>


        <h3>ROLE: </h3>
        <select id="bottomRole" v-model="selectedRole">
            <option>Web Devloper</option>
            <option>Web Designer</option>
        </select>
        <div v-if="errorMessages.length > 0 && selectedRole.length == 0" class="error">
            <p>Please select the role *</p>
        </div>
        <h3> SKILLS: </h3>
        <input v-model="newSkill" @keydown="handleKeyDown" @keyup.enter="addSkill" placeholder="Enter skills">

        <div id="addSkill">
            <p v-for="(skill, index) in skills" :key="index">
                <span>{{ skill }}</span>
                <button class="delete" @click="removeSkill(index)">x</button>
            </p>
        </div>
        <div v-if="errorMessages.length > 0 && skills.length == 0" class="error">
            <p>Please enter the skills *</p>
        </div>
        <label>
            <input id="checkBox" type="checkbox" v-model="termsChecked"> Accept Term and Condition
        </label>
        <div v-if="errorMessages.length > 0 && !termsChecked" class="error">
            <p>Please Accept Term and Condition *</p>
        </div>
        <button id="submit" @click="submitForm">Create An Account</button>
        <button id="reset" @click="empty">Reset form</button>
    </div>

    <div v-if="showResult" class="result">
        <h3>Form Data:</h3>
        <p>Email: {{ email }}</p>
        <p>Password:{{ password }}</p>
        <p>Role:{{ selectedRole }}</p>
        <p>Skills:</p>
        <ul>
            <li v-for="(skill, index) in skills" :key="index">{{ skill }}</li>
        </ul>
    </div>
</template>

<script>
export default {
    name: 'signForm',
    data() {
        return {
            email: '',
            password: '',
            selectedRole: '',
            newSkill: '',
            skills: [],
            termsChecked: false,
            passwordisValid: false,
            showResult: false,
            validEmail: false,
            errorMessages: [],
        };
    },
    methods: {
        addSkill() {
            const skillText = this.newSkill.trim();
            if (skillText !== "") {
                this.skills.push(skillText);
                this.newSkill = '';
                console.log(this.skills);
            }
        },
        removeSkill(index) {
            this.skills.splice(index, 1);
        },
        handleKeyDown(event) {
            if (event.key === ',') {
                event.preventDefault();
                this.addSkill();
            }

        },
        emailWrite() {
            this.validEmail = false
        },
        passwordWrite() {
            this.passwordisValid = false
        },
        submitForm() {
            this.errorMessages = [];
            if (!this.email || !this.password || !this.selectedRole || !this.termsChecked) {
                this.errorMessages.push('All fields are required');

            }
            if (!this.isValidEmail(this.email)) {
                this.validEmail = true
                this.errorMessages.push('Please enter a valid email address');
                console.log("email")
            }
            if (!this.isValidPassword(this.password)) {
                this.errorMessages.push('Password must be at least 8 characters long and contain at least 1 special character, 1 number, 1 uppercase letter, and 1 lowercase letter.');
                this.passwordisValid = true
                console.log("password")
                console.log(this.passwordisValid)
            }

            if (this.errorMessages.length === 0) {
                this.showResult = true;


            } else {
                this.showResult = false;
            }
        },
        isValidEmail(email) {
            const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
            return emailRegex.test(email);
        },

        isValidPassword(password) {
            const passwordRegex = /^(?=.*[!@#$%(^&)>.<,/*])(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,}$/;
            return passwordRegex.test(password);

        },




        empty() {
            this.email = ''
            this.password = ''
            this.selectedRole = ''
            this.newSkill = ''
            this.skills = []
            this.termsChecked = false
        }
    },


}
</script>
<style>
.delete {
    font-size: 10px;
    height: 20px;
    width: 20px;
    border-radius: 50%;

    cursor: pointer;
}

#reset {
    border: none;
    color: red;
    background-color: white;
    cursor: pointer;
}

#addSkill {
    display: flex;
    gap: 10px;
}

#submit {
    width: fit-content;
    text-align: center;
    margin-left: auto;
    margin-right: auto;
    padding: 5px 10px;
    background-color: rgb(61, 110, 223);
    color: white;
    font-size: 20px;
    font-weight: 600;
    border: none;
    border-radius: 1rem;
    cursor: pointer;
}

.error {
    color: red;
    font-size: 10px;
}

.form {
    margin: 3vh 1% 1vh 1%;
    padding: 5vh 5%;
    font-size: 20px;
    border-radius: 2rem;
    display: flex;
    flex-direction: column;
    gap: 1.3rem;
    width: 88%;
    background-color: rgb(255, 255, 255);
}

.errors {

    color: red;
}

.result {
    border: 1px solid black;
    border-radius: 1rem;
    background-color: lightgrey;
    margin: 3rem;
    padding: 3rem;
    display: flex;
    flex-direction: column;
    gap: .4rem;
}

span {
    font-size: 20px;
    width: fit-content;
    gap: 3px;
    width: fit-content;
    border: .5px solid black;
    background-color: lightgray;
    border-radius: 1rem;
    padding: 7px;
}

#checkBox {
    width: fit-content;
    cursor: pointer;
}

input {
    font-size: 1.5rem;
    width: 80%;
    border: none;
    border-bottom: 1px solid black;
}

h3 {
    width: 80%;
}

#bottomRole {
    width: 80%;
    border: none;
    border-bottom: 1px solid black;
}</style>
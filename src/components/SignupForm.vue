<template>
    <div class="form">
        <h3>EMAIL:</h3>
        <input type="text" v-model="email" placeholder="mario@yahoo.io">
        <h3>PASSWORD:</h3>
        <input type="text" v-model="password" placeholder="**************">
        <h3>ROLE: </h3>
        <select v-model="selectedRole">
            <option>Web Devloper</option>
            <option>Web Designer</option>
        </select>
        <h3> SKILLS: </h3>
        <input v-model="newSkill" @keyup.enter="addSkill" placeholder="Enter skills">

        <p v-for="(skill, index) in skills" :key="index">
            <span>{{ skill }}</span>
            <button class="delete" @click="removeSkill(index)">x</button>
        </p>
        <label>
            <input id="checkBox" type="checkbox" v-model="termsChecked"> Accept Term and Condition
        </label>
        <button id="submit" @click="submitForm">Create An Account</button>
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

    <div v-if="errorMessages.length > 0" class="errors">
        <h3>Error Messages:</h3>
        <ul class="error">
            <li v-for="(error, index) in errorMessages" :key="index">{{ error }}</li>
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
            showResult: false,
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
        submitForm() 
        {
            this.errorMessages = [];
            if (!this.email || !this.password || !this.selectedRole || !this.termsChecked) {
                this.errorMessages.push('All fields are required');
                this.empty();
            }
            if (!this.isValidPassword(this.password)) {
                this.errorMessages.push('Password must be at least 8 characters long and contain at least 1 special character, 1 number, 1 uppercase letter, and 1 lowercase letter.');
                this.empty();
            }
            if (this.errorMessages.length === 0) {
                this.showResult = true;
            } else {
                this.showResult = false;
            }
           
        },
        isValidPassword(password) {
            const passwordRegex = /^(?=.*[!@#$%(^&)>.<,/*])(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,}$/;
            return passwordRegex.test(password);
        },
        empty(){
            this.email= ''
            this.password= ''
            this.selectedRole= ''
            this.newSkill=''
            this.skills=[]
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

.result,
.errors {
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
}

input {
    font-size: 1.5rem;
    width: 80%;
    border: none;
    border-bottom: 1px solid black;
}

select {
    width: 80%;
    border: none;
    border-bottom: 1px solid black;
}</style>
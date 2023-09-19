<template>
   <div class="form">
        <h3>EMAIL:</h3>
        <input type="text" placeholder="mario@yahoo.io">
        <h3>PASSWORD</h3>
        <input type="text" placeholder="**************">
        <h3>ROLE: </h3>
        <select>
            <option>Web Devloper</option>
            <option>Web Designer</option>
        </select>
            <h3> SKILLS:  </h3>
            <input v-model="newSkill" @keyup.enter="addSkill" @keyup.space="addSkill" placeholder="Enter skills">
          
              <p v-for="(skill, index) in skills" :key="index">
                <span>{{ skill }}</span>
                <button class="delete" @click="removeSkill(index)">x</button>
              </p>
          <label>
            <input id="checkBox" type="checkbox" v-model="termsChecked"> Accept Term and Condition
          </label>
          <button id="submit" type="submit">Create An Account</button>
        </div> 
    
</template>

<script>
export default {
    name: 'signForm',
    data() {
        return {
            newSkill: '',
            skills: [],
            termsChecked: false,
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
    },


}
</script>
<style>
.delete{
    font-size: 10px;
    height: 20px;
    width: 20px;
    border-radius: 50%;
   
    cursor: pointer;
}
#submit{
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
    margin: 1vh 1%;
    padding: 5vh 5%;
    font-size: 20px;
    border-radius: 2rem;
    display: flex;
    flex-direction: column;
    gap: 1.3rem;
    width: 88%;
    height: 88vh;
    background-color: white;
}
span{
    font-size: 20px;
    width: fit-content;
    gap: 3px;
    width: fit-content;
    border: .5px solid black;
    background-color: lightgray;
    border-radius: 1rem;
    padding: 7px;
}
#checkBox{
    width: 10%;
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
}
</style>
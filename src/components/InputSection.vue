<template>
    <div class="MainSection">
        Enter spending
        <div class="inputSection__Fields">
            
        <input :value="exps.sum"  @input="exps.sum = $event.target.value" class="inputField" type="number" required>
        <input v-show="exps.category && exps.sum>0" @click="createPost" class="inputField__submit" type="submit" value="Spend">
        </div>
        <div class="InputSection__categories"> Choose category
            <div class="InputSection__categories__buttons">
            <input  class="InputSection__categories__button" id="one" type="radio" value="food" v-model="exps.category"  >
            <label for="one">food</label>
            <input class="InputSection__categories__button" id="two" type="radio" value="transport" v-model="exps.category" >
            <label for="two">transport</label>
            <input class="InputSection__categories__button"  id="three" type="radio" value="entertainment" v-model="exps.category" >
            <label for="three">entertainment</label>
            <input class="InputSection__categories__button"  id="four" type="radio" value="electronic" v-model="exps.category" >
            <label for="four">electronic</label>
            <input class="InputSection__categories__button" id="five" type="radio" value="clothes" v-model="exps.category" >
            <label for="five">clothes</label>
            </div>
        </div>
    </div>
    
</template>

<script>

export default {
   
    data() {
        return {
            
            exps: {
                sum: {
                    type:Number,
                },
                category: '',
                id:'',
                    
                
            },
            food:'',
            transport:'',
            entertainment:'',
            electronic:'',
            clothes:'',
        }
    },
    methods: {
        createPost() {
            
            this.exps.id =Date.now();
            if(this.exps.category =='food') {
                let food = Number(localStorage.getItem('food')) + Number(this.exps.sum)
                localStorage.setItem('food',food)
                
            }
            if(this.exps.category =='transport') {
                let transport = Number(localStorage.getItem('transport')) + Number(this.exps.sum)
                localStorage.setItem('transport',transport)
                
            }
            if(this.exps.category =='entertainment') {
                let entertainment = Number(localStorage.getItem('entertainment')) + Number(this.exps.sum)
                localStorage.setItem('entertainment',entertainment)
                
            }
            if(this.exps.category =='electronic') {
                let electronic = Number(localStorage.getItem('electronic')) + Number(this.exps.sum)
                localStorage.setItem('electronic',electronic)
                
            }
            if(this.exps.category =='clothes') {
                let clothes = Number(localStorage.getItem('clothes')) + Number(this.exps.sum)
                localStorage.setItem('clothes',clothes)
                
            }
            
            this.exps.sum = this.exps.sum + ' coins'
            
            this.$emit('create', this.exps) 
            localStorage.setItem(Date.now(), JSON.stringify(this.exps));
            this.exps = {
                sum: '',
                category:'',
            }
            

// Извлечение объекта из localStorage

           location.reload();
          
            
        },
        
    },
    
}
</script>

<style>
.focused {
    color: yellow;
}
.MainSection {
    display: flex;
    flex-direction: column;
    gap: 20px;
    padding-top: 50px;
    padding-bottom: 50px;

    padding-left: 100px;
    padding-right: 100px;
}
.inputSection__Fields {
    display: flex;
    
    gap: 40px;
}
.inputField {
    border: 3px solid blue;
    height: 40px;
    border-radius: 10px;
    width: 300px;
    
}
input[type=number]::-webkit-inner-spin-button,
input[type=number]::-webkit-outer-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
.inputField__submit {
    border: 3px solid blue;
    height: 40px;
    border-radius: 10px;
    
    
}
.InputSection__categories{
    display: flex;
    flex-direction: column;
    gap: 20px;
}
.InputSection__categories__buttons {
    display: flex;
    gap: 10px;
    cursor: pointer;
    
}
.InputSection__categories__button {

border: 3px solid black;
    height: 40px;
    border-radius: 10px;
    cursor: pointer;
    
}
.InputSection__categories__buttons input[type=radio] {
	display: none;
}
.InputSection__categories__buttons label {
	display: inline-block;
	cursor: pointer;
	padding: 0px 15px;
	line-height: 34px;
	border: 1px solid #999;
	border-radius: 6px;
	user-select: none;
}
 
/* Checked */
.InputSection__categories__buttons input[type=radio]:checked + label {
	background: #ffe0a6;
}
 
/* Hover */
.InputSection__categories__buttons label:hover {
	color: #666;
}
 
/* Disabled */
.InputSection__categories__buttons input[type=radio]:disabled + label {
	background: #efefef;
	color: #666;
}
</style>
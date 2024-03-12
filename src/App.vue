<template>
    <div class="app">
    <div class="header">
        <div>
            UrWallet
        </div>
        <div class="header__anchors">
         <div>
            Exps by category
         </div>
         <div>
             Exps by month
         </div>
         <div>
            Contacts
         </div>
        </div>
    </div>
    
    <input-section @create="createPost"/>
    <input  @click="ff" class="inputField__submit" type="submit" value="kek">
    <div v-if="expsP.data"  :expsP="expsP">
        <div v-show="expsP.data[0].sum">{{ expsP.data[0].sum }}</div>
        <div v-show="expsP.data[0].category ">{{ expsP.data[0].category }}</div>
        <div v-show="expsP.data[1].sum">{{ expsP.data[1].sum }}</div>
        <div v-show="expsP.data[1].category ">{{ expsP.data[1].category }}</div>
        <div v-show="expsP.data[2].sum">{{ expsP.data[2].sum }}</div>
        <div v-show="expsP.data[2].category ">{{ expsP.data[2].category }}</div>
    </div>
    <div class="exps">
    <exps-list :exps="exps"/>
    
        
                
                <!-- <div class="expsList">
            <div  class="expsList__fields" v-for="expsP in expsP">
                <div>{{ expsP.data.this.sum}}</div></div> -->
                <!-- <div>{{ exp.id }}</div> -->
              
    <div v-for="page in totalPages" :key="page" @click="changePage(page)">{{ page }}</div>
        <div class="exps-filter">
            <div >фильтр по месяцам {{food}}</div>
            <div>фильтр по категориям{{transport}}</div>
            <div>фильтр по категориям{{entertainment}}</div>
            <div>фильтр по категориям{{electronic}}</div>
            <div>фильтр по категориям{{clothes}}</div>
            </div>
        
    </div>
    
</div>
    
</template>

<script>
import InputSection from '@/components/InputSection.vue'
import ExpsList from './components/ExpsList.vue';
import axios   from 'axios' 
// import Pagination from 'v-pagination-3';

// import Paginate from './App.vue'

export default {
    components: {

        InputSection,ExpsList
        
    },
data() {

    return {

        food:Number(localStorage.getItem('food')),
        transport:Number(localStorage.getItem('transport')),
        entertainment:Number(localStorage.getItem('entertainment')),
        electronic:Number(localStorage.getItem('electronic')),
        clothes:Number(localStorage.getItem('clothes')),
       
        
        exps:[],
        page: 1,
        limit:3,
        totalPages:1,
         expsP:[],
    }

},
mounted() {
    if(localStorage.getItem('food')==null) {
        localStorage.setItem('food',0)
    }
    if(localStorage.getItem('transport')==null) {
        localStorage.setItem('transport',0)
    }
    if(localStorage.getItem('entertainment')==null) {
        localStorage.setItem('entertainment',0)
    }
    if(localStorage.getItem('electronic')==null) {
        localStorage.setItem('electronic',0)
    }
    if(localStorage.getItem('clothes')==null) {
        localStorage.setItem('clothes',0)
    }
    if(localStorage.length > 0) {
        for(let i = 0; i <localStorage.length;i++){
            if((localStorage.key(i))=='food'||(localStorage.key(i))=='transport'||(localStorage.key(i))=='entertainment'||(localStorage.key(i))=='electronic'||(localStorage.key(i))=='clothes') {
                continue;
            }
             
            //  console.log(JSON.parse(localStorage.getItem(localStorage.key(i)) )) ;
             const data1 =  JSON.parse(localStorage.getItem(localStorage.key(i)) );
             
             this.exps.push(data1);
        }
    }
    this.fetchPosts();
    
        this.changePage(this.page);
    
    
    // this.changePage(this.page);
},
methods: {
    ff() {
        console.log(this.expsP.data)
console.log(JSON.stringify(this.expsP.data))
    },
 async changePage(page) {
this.page = page,
this.expsP = await axios.get("http://localhost:3000/exps",{
            sum:this.exps.sum,
            category:this.exps.category,
            id:this.exps.id,
})
console.log(this.exps)
// this.expsP.data = JSON.parse(this.exps.slice(this.page*3-3,this.page*3))
if(this.expsP.data[2].sum){
    this.expsP.data = this.exps.slice(this.page*3-3,this.page*3)
}
else if(this.expsP.data[1].sum) {
    this.expsP.data = this.exps.slice(this.page*3-3,this.page*3-1)
}
else if(this.expsP.data[0].sum) {
    this.expsP.data = this.exps.slice(this.page*3-3,this.page*3-2)
}
// console.log(this.exps.slice(this.page*3-3,this.page*3))
// console.log(page)
console.log(this.expsP.data)
console.log(JSON.stringify(this.expsP.data))

 },
  async  createPost(exps) {
        this.exps.push(exps)
      await  axios.post("http://localhost:3000/exps", {
            sum:exps.sum,
            category:exps.category,
            id:exps.id,
    })
        
    },
    async fetchPosts() {
            // try {
            //     const response = await axios.get('http://localhost:3000/exps',
                    
                    
            //     );
                this.totalPages = Math.ceil(this.exps.length/this.limit)
                // console.log(response.data)
                
            // }
            // catch(e) {
            //     console.log('ere')
            // }
        }
//     async fetchPost() {
//         try {
//             const response1 = await axios.get('http://localhost:3000/exps',{
// params:{
    
//     page:this.page,
//     limit:this.limit,
// }
            
// })
// console.log(response1.data)
// console.log(this.exps)

//             this.totalPages = Math.ceil(this.exps.length/this.limit)
//             response1.data = this.exps.slice(page*3)
// //             const response = await axios.get('http://localhost:3000/exps',)
//             console.log(response1.data)
// //             console.log(this.totalPages + ' ' + this.limit)
// //             console.log(this.exps)

            
//         }
//         catch(e) {
//            alert('errer')
//         }
//     },
    
},

// watch: {
//     page() {
//         this.fetchPosts();
//     }
// }
}

</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Nunito:ital,wght@0,500;1,500&display=swap');
* {
    box-sizing: border-box;}
html {
    width: 1820px;
    width: 100%;

}
body {

    margin: 0;
    padding: 0;
    /* width: 1620px; */
    margin-left: auto;
    margin-right: auto;
    font-family:'Nunito',sans-serif;
    
}
.header {
    padding-top: 20px;
    display:flex;
    justify-content: space-between;
    font-size: 50px;
    align-items: center;
    padding-left: 100px;
    padding-right: 100px;
}
.header__anchors {
    
    display:flex;
    gap: 30px;
    font-size: 20px;

}
.exps {
    display: flex;
    padding-left: 100px;
    padding-right: 100px;
    gap: 20px;
}
.expsList {
    display:flex;
    max-width: 100%;
    flex-direction: column;
    gap: 2px;
    
    
}
ul {
    padding: 0;
    margin:0;
    display: flex;
    flex-direction: column;
    gap: 2px;
    
    
}
.expsList__fields {
    display:flex;
    justify-content: space-between;
    gap: 700px;
    background-color: rgb(79, 76, 76);
    color: rgb(255, 255, 0);
    border-radius: 10px;
    padding-left: 5px;
    padding-right: 5px;
    padding-top: 20px;
    padding-bottom: 20px;
    
    
}
</style>
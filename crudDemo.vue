<template>
    <h1> CRUD Operation </h1>
    <form>
        <br>
        <label> Name: </label>
        <input type="text" placeholder="Enter your Name" v-model="form.Name"/>
        <br>
        <label> Age: </label>
        <input type = "text" placeholder="Enter your Age" v-model="form.Age" />
        <br>
        <label> Mobile: </label>
        <input type="text" placeholder= "Enter you mobile number" v-model="form.Mobile">
        <br>
        <button v-on:click.prevent="addData"> Add Data </button>
        <br>
        <button v-on:click.prevent="showData"> Show Data </button>
    </form>
    <div v-if="dataVisible">
        <h2>Data</h2>
        <table>
            <thead>
                <tr>
                    <td>Name</td>
                    <td>Age</td>
                    <td>Mobile No.</td>
                    <td>Action</td>
                </tr>
            </thead>
            <tbody>
                <tr v-for="item, index in items" :key="index">
                    <td>{{ item.name }}</td>
                    <td>{{ item.age }}</td>
                    <td>{{ item.mobile }}</td>
                    <td><button v-on:click.prevent="deleteData(index)"> Delete Data </button></td>
                </tr>
            </tbody>
        </table>
    </div>
</template>
<script>
export default {
    name: 'CrudDemo',
    data(){
        return{
            items: null,
            form:{
                Name: "" , 
                Age: "",
                Mobile: ""
            },
            dataVisible: false
        }
    },
    methods:{
        addData(){
            let localData = localStorage.getItem( 'MyVueAppData' )
            let newItem = {
                "name": this.form.Name,
                "age": this.form.Age,
                "mobile": this.form.Mobile
            }

            let tempD = null
            if (localData == null) {
                tempD = JSON.stringify( [ newItem ] )
            } else {
                tempD = JSON.parse( localData )
                tempD.push( newItem )
                tempD = JSON.stringify( tempD )
            }

            localStorage.setItem( 'MyVueAppData', tempD )
            this.dataVisible = false;
            alert("Item has been added successfully.")
        },
        deleteData(index){
            let localData = localStorage.getItem( 'MyVueAppData' )
            let tempD = JSON.parse( localData ).filter(function(item, i) {
                return i !== index
            })
            localStorage.setItem( 'MyVueAppData', JSON.stringify(tempD) )
            this.items = tempD
        },
        showData(){
            let localData = localStorage.getItem( 'MyVueAppData' )

            if (localData == null) return;

            this.items = JSON.parse( localData )
            this.dataVisible = true;
        }
    }
    
}
    
</script>

<style>
h1{
    color:red;
}

table {
    margin-left: auto;
    margin-right: auto;
    width: 50%;
}

table, th, td {
  border: 1px solid black;
  border-collapse: collapse;
}
</style>

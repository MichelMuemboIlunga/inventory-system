<template>
    <main class="home-page">
        <h1>Product List</h1>
        <p style="color:crimson">Manage Product</p>
        <div v-if="appModel.roleId === 3 || appModel.roleId === 2">
            <button type="button" class="btn btn-primary btn-sm" data-bs-toggle="modal" data-bs-target="#exampleModal">
                Add Product
            </button>
        </div>        
        
        <div class="container" v-if="appModel.roleId ===  1">
            <div id="notfound">
                <div class="notfound">
                    <div class="notfound-404">
                        <h1>401</h1>
                    </div>
                    <h2>Oops Unauthorized, The Page you are looking for can't be Access!</h2>
                </div>
            </div>
        </div>
        <div class="container" v-if="appModel.roleId === 3 || appModel.roleId === 2">
           <table id="dtBasicExample" class="table table-striped table-dark table-hover table-sm">
            <thead>
                <tr>
                    <th>Product Name</th>
                    <th>Product Code</th>
                    <th>Price</th>
                    <th>Quantity</th>
                    <th>Category</th>
                    <th>Supplier</th>
                    <th>Action</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="product in products" :key="product.productCode">
                    <td>{{product.productName}}</td>
                    <td>{{product.productCode}}</td>
                    <td>{{product.price}}</td>
                    <td>{{product.quantity}}</td>
                    <td v-if="product.categoryId === 1">
                      DRINKS
                    </td>
                    <td v-if="product.categoryId === 2">
                      MINERALS
                    </td>
                    <td v-if="product.categoryId === 3">
                      FATS
                    </td>
                    <td v-if="product.categoryId === 4">
                      PROTEINS
                    </td>
                    <td v-if="product.categoryId === 5">
                      BEAUTY_AND_BODY
                    </td>
                    <td>{{product.supplierId}}</td>
                    <td v-if="appModel.roleId ===  1">
                        <button type="button" class="material-icons btn-sm bg-warning text-white" disabled data-toggle="tooltip" data-placement="top" title="No Edit Action">
                            block
                        </button>
                        &nbsp;&nbsp;
                        <button type="button" class="material-icons  btn-sm bg-danger text-white" disabled data-toggle="tooltip" data-placement="top" title="No Delete Action">
                            block
                        </button>
                    </td>
                    <td v-if="appModel.roleId ===  2 || appModel.roleId === 3">
                        <button type="button" class="material-icons btn-sm bg-warning text-white" data-bs-toggle="modal" data-bs-target="#editModal" @click="editData(product.productCode)">create</button>
                        &nbsp;&nbsp;
                        <button type="button" class="material-icons  btn-sm bg-danger text-white" data-bs-toggle="modal" data-bs-target="#deleteModal" @click="getDeleteProductCode(product.productCode)">delete_outline</button>
                    </td>
                </tr>                
            </tbody>
           </table>
        </div>

        <!-- Modal Add Customer -->
        <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog modal-lg">
            <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLabel">
                    Add Product
                    <span class="label label-success">new</span>
                </h5>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                <div v-if="isProccess">
                    <div class="spinner-grow text-primary" role="status">
                    <span class="visually-hidden">Loading...</span>
                    </div>
                    <div class="spinner-grow text-secondary" role="status">
                    <span class="visually-hidden">Loading...</span>
                    </div>
                    <div class="spinner-grow text-success" role="status">
                    <span class="visually-hidden">Loading...</span>
                    </div>
                    <div class="spinner-grow text-danger" role="status">
                    <span class="visually-hidden">Loading...</span>
                    </div>
                    <div class="spinner-grow text-warning" role="status">
                    <span class="visually-hidden">Loading...</span>
                    </div>
                </div>
                <div class="mb-3">
                    <label for="productName" class="form-label">Product Name</label>
                    <input type="text" 
                           class="form-control form-control-sm" 
                           id="txtProductName" 
                           placeholder="Product Name"
                           v-model="productName" required>
                </div>
                <div class="mb-3">
                    <label for="productCode" class="form-label">Product Code</label>
                    <input type="text" 
                           class="form-control form-control-sm" 
                           id="txtProductCode" 
                           placeholder="Product Code"
                           v-model="productCode" required>
                </div>
                <div class="mb-3">
                    <label for="productQuantity" class="form-label">Product Price</label>
                    <input type="number" 
                           class="form-control form-control-sm" 
                           id="txtProductPrice" 
                           placeholder="Product Price"
                           v-model="productPrice" required>
                </div>
                <div class="mb-3">
                    <label for="productQuantity" class="form-label">Product Quantity</label>
                    <input type="text" 
                           class="form-control form-control-sm" 
                           id="txtProductQuantity" 
                           placeholder="Product Quantity"
                           v-model="productQuantity" required>
                </div>
                <div class="mb-3">
                    <label for="Surname" class="form-label">Category</label>
                    <select class="form-select form-select-sm" aria-label=".form-select-sm example"  v-model="categoryId">
                        <option value="0">Select Product Category</option>
                        <option value="1">Drinks</option>
                        <option value="2">Minerals</option>
                        <option value="3">Fats</option>
                        <option value="4">Proteins</option>
                        <option value="5">Beauty and Body</option>
                    </select>
                </div>     
                <div class="mb-3">
                    <label for="Surname" class="form-label">Supplier</label>
                    <select class="form-select form-select-sm" aria-label=".form-select-sm example"  v-model="SupplierId">
                        <option value="0">Please Select Supplier</option>
                        <option v-for="item in suppliers" :key="item.SupplierId">
                            {{item.supplierName}}
                        </option>
                    </select>
                </div>     
                
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-danger btn-sm" data-bs-dismiss="modal">Cancel</button>
                <button type="button" class="btn btn-success btn-sm" @click="addProduct">Save</button>
            </div>
            </div>
        </div>
        </div>


         <!-- Edit  Product -->
         <div class="modal fade" id="editModal" tabindex="-1" aria-labelledby="editModalLabel" aria-hidden="true" >
        <div class="modal-dialog modal-lg">
            <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="editModal">
                    Maintain Product
                    <span class="label label-warning">Update</span>
                </h5>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                <div v-if="isProccessMaintain">
                    <div class="spinner-grow text-primary" role="status">
                    <span class="visually-hidden">Loading...</span>
                    </div>
                    <div class="spinner-grow text-secondary" role="status">
                    <span class="visually-hidden">Loading...</span>
                    </div>
                    <div class="spinner-grow text-success" role="status">
                    <span class="visually-hidden">Loading...</span>
                    </div>
                    <div class="spinner-grow text-danger" role="status">
                    <span class="visually-hidden">Loading...</span>
                    </div>
                    <div class="spinner-grow text-warning" role="status">
                    <span class="visually-hidden">Loading...</span>
                    </div>
                </div>
                <div class="mb-3">
                    <label for="productName" class="form-label">Product Name</label>
                    <input type="text" 
                           class="form-control form-control-sm" 
                           id="txtProductName" 
                           placeholder="Product Name"
                           v-model="productNameUpdate" required>
                </div>
                <div class="mb-3">
                    <label for="productCode" class="form-label">Product Code</label>
                    <input type="text" 
                           class="form-control form-control-sm" 
                           id="txtProductCode" 
                           placeholder="Product Code"
                           v-model="productCodeUpdate" required disabled>
                </div>
                <div class="mb-3">
                    <label for="productQuantity" class="form-label">Product Price</label>
                    <input type="number" 
                           class="form-control form-control-sm" 
                           id="txtProductPrice" 
                           placeholder="Product Price"
                           v-model="productPriceUpdate" required>
                </div>
                <div class="mb-3">
                    <label for="productQuantity" class="form-label">Product Quantity</label>
                    <input type="text" 
                           class="form-control form-control-sm" 
                           id="txtProductQuantity" 
                           placeholder="Product Quantity"
                           v-model="productQuantityUpdate" required>
                </div>
                <div class="mb-3">
                    <label for="Surname" class="form-label">Category</label>
                    <select class="form-select form-select-sm" aria-label=".form-select-sm example"  v-model="productCategoryUpdate" disabled> 
                        <option>{{productCategoryUpdate}}</option>
                    </select>
                </div>     
                <div class="mb-3">
                    <label for="Surname" class="form-label">Supplier</label>
                    <select class="form-select form-select-sm" aria-label=".form-select-sm example"  v-model="SupplierIdUpdate">
                        
                        <option v-for="item in suppliers" :key="item.SupplierIdUpdate">
                            {{item.supplierName}}
                        </option>
                    </select>
                </div>     
                
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-danger btn-sm" data-bs-dismiss="modal">Cancel</button>
                <button type="button" class="btn btn-success btn-sm" @click="maintainProduct">Update</button>
            </div>
            </div>
        </div>
        </div>

        <!-- Delete Product -->
        <div id="deleteModal" class="modal fade">
            <div class="modal-dialog modal-confirm">
                <div class="modal-content">
                    <div class="modal-header flex-column">
                        <div class="icon-box">
                            <i class="material-icons">&#xE5CD;</i>
                        </div>						
                        <h4 class="modal-title w-100">Are you sure?</h4>	
                        <button type="button" class="close" data-bs-dismiss="modal" aria-hidden="true">&times;</button>
                    </div>
                    <div class="modal-body">
                        <div v-if="isProccessDelete">
                            <div class="spinner-grow text-primary" role="status">
                            <span class="visually-hidden">Loading...</span>
                            </div>
                            <div class="spinner-grow text-secondary" role="status">
                            <span class="visually-hidden">Loading...</span>
                            </div>
                            <div class="spinner-grow text-success" role="status">
                            <span class="visually-hidden">Loading...</span>
                            </div>
                            <div class="spinner-grow text-danger" role="status">
                            <span class="visually-hidden">Loading...</span>
                            </div>
                            <div class="spinner-grow text-warning" role="status">
                            <span class="visually-hidden">Loading...</span>
                            </div>
                        </div>
                        <p hidden="true"> {{ deleteMobileNumber }}</p>
                        <p>Do you really want to delete these Product? This process cannot be undone.</p>
                    </div>
                    <div class="modal-footer justify-content-center">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancel</button>
                        <button type="button" class="btn btn-danger" @click="deleteProduct()">Delete</button>
                    </div>
                </div>
            </div>
        </div> 
    </main>
    <div hidden="true">
      <p>{{ appModel.email }}</p>
    </div>
    <div hidden="true">
      <p>{{ appModel.roleId }}</p>
    </div>
</template>

<script>

//Bootstrap and jQuery libraries
import 'bootstrap/dist/css/bootstrap.min.css';
import 'jquery/dist/jquery.min.js';
//Datatable Modules
import "datatables.net-dt/js/dataTables.dataTables"
import "datatables.net-dt/css/jquery.dataTables.min.css"
import $ from 'jquery'; 

import axios from "axios"

export default {
  mounted(){
        let appData = JSON.parse(localStorage.getItem("app.data"))
        this.appModel = appData;

          // GEt Supplier
        axios.get("http://localhost:8080/supplier/all")
        .then((res)=>
        {
            this.suppliers = res.data;
            this.SupplierIdUpdate = res.data;
        }),

        //API Call Get All Products
        axios
        .get("http://localhost:8080/product/all")
        .then((res)=>
        {
            this.products = res.data;

            console.log(this.products)

            if(this.products.length > 0){
                let categoryConverter = res.data[0].categoryId;
                if(categoryConverter === 1){
                    this.categoryId = "DRINKS"
                }else if(categoryConverter === 2){
                    this.categoryId = "MINERALS"
                }else if(categoryConverter === 3){
                    this.categoryId = "FATS"
                }else if(categoryConverter === 4){
                    this.categoryId = "PROTEINS"
                }else if(categoryConverter === 5){
                    this.categoryId = "BEAUTY_AND_BODY"
                }
            }
           
            $(document).ready(function () {
            $('#dtBasicExample').DataTable({
                lengthMenu: [
                    [5, 10, 25, 50, -1],
                    [5, 10, 25, 50, 'All'],
                ],
                
            });
            $('.dataTables_length').addClass('bs-select');
            });
        })

      

  },
  data: function() {
        return {
            products:[],
            suppliers:[],
            SupplierId:"0",
            categoryId:"0",
            productName: "",
            productCode: "",
            productPrice:"",
            productQuantity:"",
            price: null,
            quantity: null,
            appModel: {
                email: '',
                roleId: ''
            },
            isProccess: false,
            isProccessMaintain: false,
            isProccessDelete: false,

            productNameError:"",
            productCodeError:"",
            productPriceError:"",
            productQuantityError:"",
            categoryIdError:"",
            supplierConverter: null,

            SupplierIdUpdate:[],
            categoryIdUpdate:"0",
            productNameUpdate: "",
            productCodeUpdate: "",
            productPriceUpdate:"",
            productQuantityUpdate:"",
            productCategoryUpdate:"0",

            productCodeUpdateError :"",
            categoryIdUpdateError :"",
            productQuantityUpdateError: "",
            productPriceUpdateError :"",
            productNameUpdateError :"",
            supplierNameUpdateError : "",

            deleteProductCode: ''
       }
    },
    methods:{
        addProduct(){            
            if(this.productName === ""){
                
                this.productCodeError = "",
                this.categoryIdError = "",
                this.productQuantityError = "",
                this.productPriceError = ""
                this.productNameError = "Product Name is required";
                return;
            }else if(this.productCode === ""){
                this.productNameError = ""
                this.categoryIdError = "",
                this.productQuantityError = "",
                this.productPriceError = "",               
                this.productCodeError = "Product Code is required"
                return;
            }else if(this.productPrice === ""){
                this.productNameError = "",
                this.productCodeError = "",
                this.categoryIdError = "",
                this.productQuantityError = "",
                this.productPriceError = "Product Price is required"
                return;
            }
            else if(this.productQuantity === ""){
                this.productNameError = "",
                this.productCodeError = "",
                this.productPriceError = "",
                this.categoryIdError = ""
                this.productQuantityError = "Product Quantity is required"
                return;
            }else if(this.categoryId === "0"){
                this.productNameError = "",
                this.productCodeError = "",
                this.productPriceError = "",
                this.productQuantityError ="",
                this.categoryIdError = "Category is required"
                return;
            }
            else if(this.SupplierId === "0"){
                this.productNameError = "",
                this.productCodeError = "",
                this.productPriceError = "",
                this.productQuantityError ="",
                this.categoryIdError = "Supplier is required"
                return;
            }
            else{                
              // GEt Supplier
           
              axios.get(`http://localhost:8080/supplier/find/${this.SupplierId}`) .then(response => {
                
                if(response.status === 200){
                    // set values
                    this.supplierConverter = response.data.supplierId       
                    console.log("isss" + this.supplierConverter)                                 
                }

                const formData = { 
                    productName: this.productName, 
                    productCode: this.productCode,
                    quantity: this.productQuantity, 
                    price: this.productPrice,
                    categoryId: this.categoryId,
                    supplierId: this.supplierConverter ,
                    createdBy: this.appModel.email
                }

                this.isProccess = true
                setTimeout(_ => {
                axios.post("http://localhost:8080/product/add", formData) .then(response => {
                
                if(response.status === 201){
                    this.isProccess = false;
                    alert("Product Successfully added");    
                    
                    }else{
                        alert(response.data);
                    }
                    }) 
                }, 5000); 
            }) 
        }
        },   
        editData(data) {
           this.productCodeUpdate = data;
           axios.get(`http://localhost:8080/product/find/${data}`) .then(response => {
               
               if(response.status === 200){
                 // set values

                 console.log(response.data);
                 this.productNameUpdate = response.data.productName;
                 this.productPriceUpdate = response.data.price;
                 this.productQuantityUpdate = response.data.quantity;
                 this.productCategoryUpdate = response.data.categories.description;
                 
        
                }else{
                    console.log(response.data);
                }
            }) 


        },
        maintainProduct(){

            if(this.productNameUpdate === ""){
                this.productCodeUpdateError = ""
                this.categoryIdUpdateError = ""
                this.productQuantityUpdateError = ""
                this.productPriceUpdateError = ""
                this.productNameUpdateError = "Product Name is required"
            }else if(this.productCodeUpdate === ""){
                this.categoryIdUpdateError = ""
                this.productQuantityUpdateError = ""
                this.productPriceUpdateError = ""
                this.productNameUpdateError = ""
                this.productCodeUpdateError = "Product code is required"
            }else if(this.productPriceUpdate === ""){
                this.categoryIdUpdateError = ""
                this.productQuantityUpdateError = ""
                this.productPriceUpdateError = "Product price is required"
                this.productNameUpdateError = ""
            }else if(this.productQuantityUpdate === ""){
                this.categoryIdUpdateError = ""
                this.productQuantityUpdateError = "Product quantiy is required"
                this.productPriceUpdateError = ""
                this.productNameUpdateError = ""
            }else if(this.SupplierIdUpdate === ""){
                this.categoryIdUpdateError = ""
                this.productQuantityUpdateError = ""
                this.productPriceUpdateError = ""
                this.productNameUpdateError = ""
                this.supplierNameUpdateError = "Supplier name is required"
            }else{

                    // GEt Supplier
                console.log("sss" + this.SupplierIdUpdate)
                axios.get(`http://localhost:8080/supplier/find/${this.SupplierIdUpdate}`) .then(response => {
                    
                    if(response.status === 200){
                        // set values
                        this.supplierConverter = response.data.supplierId       
                        console.log("isss" + this.supplierConverter)                                 
                    }
                    const formData = { 
                        productName: this.productNameUpdate, 
                        productCode: this.productCodeUpdate,
                        quantity: this.productQuantityUpdate, 
                        price: this.productPriceUpdate,
                        categoryId: this.categoryIdUpdate,
                        supplierId: this.supplierConverter ,
                        modifiedBy: this.appModel.email

                    }
                this.isProccessMaintain = true; 
                setTimeout(_ => {
                axios.put(`http://localhost:8080/product/maintain/${formData.productCode}`, formData) .then(response => {
                
                if(response.status === 200){
                    this.isProccessMaintain = false;
                    alert("Product Successfully Updated");    
                    
                    }else{
                        alert(response.data);
                    }
                    }) 
                }, 5000); 

            });
                
        }
    },
    getDeleteProductCode(data){
        console.log(data)
        this.deleteProductCode = data;
        axios.get(`http://localhost:8080/product/find/${data}`) 
    },
    deleteProduct(){
    
        this.isProccessDelete = true; 
        setTimeout(_ => {
        axios.delete(`http://localhost:8080/product/delete/${this.deleteProductCode}`) .then(response => {
        
        if(response.status === 200){
            this.isProccessDelete = false;
            alert("Product Successfully Deleted");    
            
            }else{
                alert(response.data);
            }
            }) 
        }, 5000); 
        }
    }
    
}
</script>

<style lang="css" scoped>
.home-page{
  background-color: #D3D3D3;
}

.label-success{
    background-color: rgb(44, 241, 44);
    width: 60px;
    border-radius:3px;
}

.label-warning{
    background-color: rgb(241, 139, 44);
    width: 60px;
    border-radius:3px;
}


.modal-confirm {		
	color: #636363;
	width: 400px;
}
.modal-confirm .modal-content {
	padding: 20px;
	border-radius: 5px;
	border: none;
	text-align: center;
	font-size: 14px;
}
.modal-confirm .modal-header {
	border-bottom: none;   
	position: relative;
}
.modal-confirm h4 {
	text-align: center;
	font-size: 26px;
	margin: 30px 0 -10px;
}
.modal-confirm .close {
	position: absolute;
	top: -5px;
	right: -2px;
}
.modal-confirm .modal-body {
	color: #999;
}
.modal-confirm .modal-footer {
	border: none;
	text-align: center;		
	border-radius: 5px;
	font-size: 13px;
	padding: 10px 15px 25px;
}
.modal-confirm .modal-footer a {
	color: #999;
}		
.modal-confirm .icon-box {
	width: 80px;
	height: 80px;
	margin: 0 auto;
	border-radius: 50%;
	z-index: 9;
	text-align: center;
	border: 3px solid #f15e5e;
}
.modal-confirm .icon-box i {
	color: #f15e5e;
	font-size: 46px;
	display: inline-block;
	margin-top: 13px;
}
.modal-confirm .btn, .modal-confirm .btn:active {
	color: #fff;
	border-radius: 4px;
	background: #60c7c1;
	text-decoration: none;
	transition: all 0.4s;
	line-height: normal;
	min-width: 120px;
	border: none;
	min-height: 40px;
	border-radius: 3px;
	margin: 0 5px;
}
.modal-confirm .btn-secondary {
	background: #c1c1c1;
}
.modal-confirm .btn-secondary:hover, .modal-confirm .btn-secondary:focus {
	background: #a8a8a8;
}
.modal-confirm .btn-danger {
	background: #f15e5e;
}
.modal-confirm .btn-danger:hover, .modal-confirm .btn-danger:focus {
	background: #ee3535;
}




#notfound {
  position: relative;
}

#notfound .notfound {
  position: absolute;
  left: 50%;
  top: 50%;
  -webkit-transform: translate(-50%, -50%);
      -ms-transform: translate(-50%, -50%);
          transform: translate(-50%, -50%);
}

.notfound {
  max-width: 710px;
  width: 100%;
  text-align: center;
  padding: 0px 15px;
  line-height: 1.4;
}

.notfound .notfound-404 {
  height: 200px;
  line-height: 200px;
}

.notfound .notfound-404 h1 {
  font-family: 'Fredoka One', cursive;
  font-size: 168px;
  margin: 0px;
  color: #ff508e;
  text-transform: uppercase;
}

.notfound h2 {
  font-family: 'Raleway', sans-serif;
  font-size: 22px;
  font-weight: 400;
  text-transform: uppercase;
  color: #222;
  margin: 0;
}

.notfound-search {
  position: relative;
  padding-right: 123px;
  max-width: 420px;
  width: 100%;
  margin: 30px auto 22px;
}

.notfound-search input {
  font-family: 'Raleway', sans-serif;
  width: 100%;
  height: 40px;
  padding: 3px 15px;
  color: #222;
  font-size: 18px;
  background: #f8fafb;
  border: 1px solid rgba(34, 34, 34, 0.2);
  border-radius: 3px;
}

.notfound-search button {
  font-family: 'Raleway', sans-serif;
  position: absolute;
  right: 0px;
  top: 0px;
  width: 120px;
  height: 40px;
  text-align: center;
  border: none;
  background: #ff508e;
  cursor: pointer;
  padding: 0;
  color: #fff;
  font-weight: 700;
  font-size: 18px;
  border-radius: 3px;
}

.notfound a {
  font-family: 'Raleway', sans-serif;
  display: inline-block;
  font-weight: 700;
  border-radius: 15px;
  text-decoration: none;
  color: #39b1cb;
}

.notfound a>.arrow {
  position: relative;
  top: -2px;
  border: solid #39b1cb;
  border-width: 0 3px 3px 0;
  display: inline-block;
  padding: 3px;
  -webkit-transform: rotate(135deg);
      -ms-transform: rotate(135deg);
          transform: rotate(135deg);
}

@media only screen and (max-width: 767px) {
  .notfound .notfound-404 {
    height: 122px;
    line-height: 122px;
  }
  .notfound .notfound-404 h1 {
    font-size: 122px;
  }

}
</style>
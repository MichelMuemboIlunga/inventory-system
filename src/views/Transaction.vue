<template>
  <main class="home-page">
        <h1>Transaction List</h1>
        <p style="color:crimson">Manage Transaction</p>    
        <br />
        <div class="container" v-if="appModel.roleId ===  1 || appModel.roleId ===  2">
            <div id="notfound">
                <div class="notfound">
                    <div class="notfound-404">
                        <h1>401</h1>
                    </div>
                    <h2>Oops Unauthorized, The Page you are looking for can't be Access!</h2>
                </div>
            </div>
        </div>
        <div class="container" v-if="appModel.roleId ===  3">
           <table id="dtBasicExample" class="table table-striped table-dark table-hover table-sm">
            <thead>
                <tr>
                    <th>Transaction Number</th>
                    <th>Customer</th>
                    <th>Order</th>
                    <th>Order Status</th>
                    <th>Price</th>
                    <th>Product</th>
                    <th>Quantity</th>
                    <th>SupplierId</th>
                    <th>Transaction Status</th>
                    <th>Action</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="transaction in transactions" :key="transaction.transactionId">
                    <td>{{transaction.transactionId}}</td>
                    <td>{{transaction.customerId}}</td>
                    <td>{{transaction.orderId}}</td>
                    <td>{{transaction.orderStatusId}}</td>
                    <td>{{transaction.price}}</td>
                    <td>{{transaction.product}}</td>
                    <td>{{transaction.quantity}}</td>
                    <td>{{transaction.supplier}}</td>
                    <td>{{transaction.transactionStatus}}</td>
                    <td v-if="appModel.roleId ===  1 || appModel.roleId ===  2">
                      <button type="button" class="material-icons btn-sm bg-warning text-white" data-bs-toggle="modal" data-bs-target="#editModal">
                        block
                      </button>
                        &nbsp;&nbsp;
                        <button type="button" class="material-icons  btn-sm bg-danger text-white" disabled data-toggle="tooltip" data-placement="top" title="No Delete Action">
                          block
                        </button>
                    </td>
                    <td v-if="appModel.roleId ===  3">
                        <button type="button" class="material-icons btn-sm bg-warning text-white" data-bs-toggle="modal" data-bs-target="#editModal" @click="editData(transaction.transactionId)">create</button>
                        &nbsp;&nbsp;
                        <button type="button" class="material-icons  btn-sm bg-danger text-white" data-bs-toggle="modal" data-bs-target="#deleteModal" @click="getDeleteSupplier(transaction.transactionId)">delete_outline</button>
                    </td>
                </tr>
                
            </tbody>
           </table>
        </div>

           <!-- Modal Add Order -->
        <!-- <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog modal-lg">
            <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLabel">
                    Add Order
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
                    <label for="supplierName" class="form-label">Supplier Name</label>
                    <input type="text" 
                           class="form-control form-control-sm" 
                           id="txtSupplierName" 
                           v-model="supplierName" required>
                </div>
                <div v-if="supplierNameError" class="text-danger">
                    {{ supplierNameError }}
                </div>
  
                <div class="mb-3">
                    <label for="supplierDescription" class="form-label">Supplier Description</label>
                    <input type="txet" 
                           class="form-control form-control-sm" 
                           id="txtsupplierDescription" 
                           v-model="supplierDescription" required>
                </div>
                <div v-if="supplierDescriptionError" class="text-danger">
                    {{ supplierDescriptionError }}
                </div>
                
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-danger btn-sm" data-bs-dismiss="modal">Cancel</button>
                <button type="button" class="btn btn-success btn-sm" @click="addSupplier">Save</button>
            </div>
            </div>
        </div>
        </div> -->

          <!-- Edit  Supplier -->
        <!-- <div class="modal fade" id="editModal" tabindex="-1" aria-labelledby="editModalLabel" aria-hidden="true" >
        <div class="modal-dialog modal-lg">
            <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="editModal">
                    Maintain Supplier
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
                    <label for="supplierName" class="form-label">Supplier Name</label>
                    <input type="text" 
                           class="form-control form-control-sm" 
                           id="txtSupplierNameUpdate" 
                           v-model="supplierNameUpdate" required disabled>
                </div>
                <div v-if="supplierNameUpdateError" class="text-danger">
                    {{ supplierNameUpdateError }}
                </div>
                <div class="mb-3">
                    <label for="SupplierDescription" class="form-label">Supplier Description</label>
                    <input type="txet" 
                           class="form-control form-control-sm" 
                           id="txtSupplierDescription" 
                           v-model="supplierDescriptionUpdate" required>
                </div>
                <div v-if="supplierDescriptionUpdateError" class="text-danger">
                    {{ supplierDescriptionUpdateError }}
                </div>
                
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-danger btn-sm" data-bs-dismiss="modal">Cancel</button>
                <button type="button" class="btn btn-success btn-sm" @click="maintainSupplier">Update</button>
            </div>
            </div>
        </div>
        </div> -->

         <!-- Delete Customer -->
         <!-- <div id="deleteModal" class="modal fade">
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
                        <p hidden="true"> {{ deleteSupplier }}</p>
                        <p>Do you really want to delete these Supplier? This process cannot be undone.</p>
                    </div>
                    <div class="modal-footer justify-content-center">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancel</button>
                        <button type="button" class="btn btn-danger" @click="deleteSupplierById()">Delete</button>
                    </div>
                </div>
            </div>
        </div> -->

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

        
        //API Call Get All Customer
        axios
        .get("http://localhost:8080/order/all")
        .then((res)=>
        {
            this.orders = res.data;
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
          orders:[],
            // firstName: "",
            // surName: "",
            // address: "",
            // gender: "selected",
            // mobileNumber:"",
            appModel: {
                email: '',
                roleId: ''
            },
            isProccess: false,
            isProccessMaintain: false,
            isProccessDelete: false

            // firstNameUpdate: "",
            // surNameUpdate: "",
            // addressUpdate: "",
            // genderUpdate: " ",
            // mobileNumberUpdate:"",
            // deleteMobileNumber: ""
        }
    },
}
</script>

<style lang="css" scoped>
.home-page{
  background-color: #D3D3D3;
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

.btnModal{
    border: 0;
    width: 150px;
    height: 40px;
    color: white;
    border-radius: 20px;
    font-family: Poppins-Medium;
    font-size: 15px;
    text-decoration: none;
    background-color: #b721ff;
    float: left;
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

</style>

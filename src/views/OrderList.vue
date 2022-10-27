<template>
    <main class="home-page">
        <h1>Order List</h1>
        <p style="color:crimson">Manage Orders</p>
        <div v-if="appModel.roleId === 3">
            <button type="button" class="btn btn-primary btn-sm" data-bs-toggle="modal" data-bs-target="#exampleModal">
                Create Order
            </button>
        </div>
        <br />
        <div class="container">
            <table id="dtBasicExample" class="table table-striped table-dark table-hover table-sm">
                <thead>
                    <tr>
                        <th>Order Number</th>
                        <th>Customer</th>
                        <th>Order Status</th>
                        <th>Product </th>
                        <th>Action</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="order in orders" :key="order.orderId">
                        <td>{{order.orderId}}</td>
                        <td>{{order.customerId}}</td>
                        <td>{{order.orderStatusId}}</td>
                        <td>{{order.productId}}</td>
                        <td v-if="appModel.roleId ===  1 || appModel.roleId ===  2">
                            <button type="button" class="material-icons btn-sm bg-warning text-white"
                                data-bs-toggle="modal" data-bs-target="#editModal"
                                @click="editData(order.orderId)">create</button>
                            &nbsp;&nbsp;
                            <button type="button" class="material-icons  btn-sm bg-danger text-white" disabled
                                data-toggle="tooltip" data-placement="top" title="No Delete Action">
                                block
                            </button>
                        </td>
                        <td v-if="appModel.roleId ===  3">
                            <button type="button" class="material-icons btn-sm bg-warning text-white"
                                data-bs-toggle="modal" data-bs-target="#editModal"
                                @click="editData(order.orderId)">create</button>
                            &nbsp;&nbsp;
                            <button type="button" class="material-icons  btn-sm bg-danger text-white"
                                data-bs-toggle="modal" data-bs-target="#deleteModal"
                                @click="getDeleteSupplier(order.orderId)">delete_outline</button>
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
    mounted() {
        let appData = JSON.parse(localStorage.getItem("app.data"))
        this.appModel = appData;


        //API Call Get All Customer
        axios
            .get("http://localhost:8080/order/all")
            .then((res) => {
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
    data: function () {
        return {
            orders: [],
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
.home-page {
    background-color: #D3D3D3;
}
</style>

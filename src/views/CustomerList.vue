<template>
  <main class="home-page">
    <h1>CustomerList</h1>
    <p style="color: crimson">Manage Customer</p>
    <!-- <div>
            <button type="button" class="btnModal btn-lg"  @click="showModal">Add Customer</button>
        </div> -->
    <div>
      <button
        type="button"
        class="btn btn-primary btn-sm"
        data-bs-toggle="modal"
        data-bs-target="#exampleModal"
      >
        Add Customer
      </button>
    </div>
    <br />
    <div class="container">
      <table
        id="dtBasicExample"
        class="table table-striped table-dark table-hover table-sm"
      >
        <thead>
          <tr>
            <th>First Name</th>
            <th>Surname</th>
            <th>Address</th>
            <th>Gender</th>
            <th>Mobile Number</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="customer in customers" :key="customer.mobileNumber">
            <td>{{ customer.firstName }}</td>
            <td>{{ customer.surname }}</td>
            <td>{{ customer.address }}</td>
            <td>{{ customer.gender }}</td>
            <td>{{ customer.mobileNumber }}</td>
            <td v-if="appModel.roleId === 1">
              <button
                type="button"
                class="material-icons btn-sm bg-warning text-white"
                disabled
                data-toggle="tooltip"
                data-placement="top"
                title="No Edit Action"
              >
                block
              </button>
              &nbsp;&nbsp;
              <button
                type="button"
                class="material-icons btn-sm bg-danger text-white"
                disabled
                data-toggle="tooltip"
                data-placement="top"
                title="No Delete Action"
              >
                block
              </button>
            </td>
            <td v-if="appModel.roleId === 2">
              <button
                type="button"
                class="material-icons btn-sm bg-warning text-white"
                data-bs-toggle="modal"
                data-bs-target="#editModal"
                @click="editData(customer.mobileNumber)"
              >
                create
              </button>
              &nbsp;&nbsp;
              <button
                type="button"
                class="material-icons btn-sm bg-danger text-white"
                disabled
                data-toggle="tooltip"
                data-placement="top"
                title="No Delete Action"
              >
                block
              </button>
            </td>
            <td v-if="appModel.roleId === 3">
              <button
                type="button"
                class="material-icons btn-sm bg-warning text-white"
                data-bs-toggle="modal"
                data-bs-target="#editModal"
                @click="editData(customer.mobileNumber)"
              >
                create
              </button>
              &nbsp;&nbsp;
              <button
                type="button"
                class="material-icons btn-sm bg-danger text-white"
                data-bs-toggle="modal"
                data-bs-target="#deleteModal"
                @click="getDeleteMobileNumber(customer.mobileNumber)"
              >
                delete_outline
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <!-- Modal Add Customer -->
    <div
      class="modal fade"
      id="exampleModal"
      tabindex="-1"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog modal-lg">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">
              Add Customer
              <span class="label label-success">new</span>
            </h5>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
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
              <label for="firstName" class="form-label">First Name</label>
              <input
                type="text"
                class="form-control form-control-sm"
                id="txtFirstName"
                placeholder="Your First Name"
                v-model="firstName"
                required
              />
            </div>
            <div class="mb-3">
              <label for="Surname" class="form-label">Surname</label>
              <input
                type="txet"
                class="form-control form-control-sm"
                id="txtSurname"
                placeholder="Your Surname"
                v-model="surName"
                required
              />
            </div>
            <div class="mb-3">
              <label for="Surname" class="form-label">Gender</label>
              <select
                class="form-select form-select-sm"
                aria-label=".form-select-sm example"
                v-model="gender"
              >
                <option value="selected">Select Gender</option>
                <option value="male">Male</option>
                <option value="female">Female</option>
              </select>
            </div>
            <div class="mb-3">
              <label class="form-label" for="typePhone">Phone number</label>
              <input
                type="tel"
                id="typePhone"
                class="form-control form-control-sm"
                placeholder="Your Phone Number (+27.....)"
                v-model="mobileNumber"
                required
              />
            </div>
            <div class="mb-3">
              <label for="address" class="form-label">Address</label>
              <textarea
                class="form-control"
                id="txtAddress"
                rows="3"
                v-model="address"
                required
              >
              </textarea>
            </div>
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-danger btn-sm"
              data-bs-dismiss="modal"
            >
              Cancel
            </button>
            <button
              type="button"
              class="btn btn-success btn-sm"
              @click="addCustomer"
            >
              Save
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Edit  Customer -->
    <div
      class="modal fade"
      id="editModal"
      tabindex="-1"
      aria-labelledby="editModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog modal-lg">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="editModal">
              Maintain Customer
              <span class="label label-warning">Update</span>
            </h5>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
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
              <label for="firstName" class="form-label">First Name</label>
              <input
                type="text"
                class="form-control form-control-sm"
                id="txtFirstName"
                placeholder="Your First Name"
                v-model="firstNameUpdate"
                required
              />
            </div>
            <div class="mb-3">
              <label for="Surname" class="form-label">Surname</label>
              <input
                type="txet"
                class="form-control form-control-sm"
                id="txtSurname"
                placeholder="Your Surname"
                v-model="surNameUpdate"
                required
              />
            </div>
            <div class="mb-3">
              <label for="Surname" class="form-label">Gender</label>
              <select
                class="form-select form-select-sm"
                aria-label=".form-select-sm example"
                v-model="genderUpdate"
                disabled
              >
                <option>{{ genderUpdate }}</option>
              </select>
            </div>
            <div class="mb-3">
              <label class="form-label" for="typePhone">Phone number</label>
              <input
                type="tel"
                id="typePhone"
                class="form-control form-control-sm"
                placeholder="Your Phone Number (+27.....)"
                v-model="mobileNumberUpdate"
                disabled
              />
            </div>
            <div class="mb-3">
              <label for="address" class="form-label">Address</label>
              <textarea
                class="form-control"
                id="txtAddress"
                rows="3"
                v-model="addressUpdate"
                required
              >
              </textarea>
            </div>
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-danger btn-sm"
              data-bs-dismiss="modal"
            >
              Cancel
            </button>
            <button
              type="button"
              class="btn btn-success btn-sm"
              @click="MaintainCustomer"
            >
              Update
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Delete Customer -->
    <div id="deleteModal" class="modal fade">
      <div class="modal-dialog modal-confirm">
        <div class="modal-content">
          <div class="modal-header flex-column">
            <div class="icon-box">
              <i class="material-icons">&#xE5CD;</i>
            </div>
            <h4 class="modal-title w-100">Are you sure?</h4>
            <button
              type="button"
              class="close"
              data-bs-dismiss="modal"
              aria-hidden="true"
            >
              &times;
            </button>
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
            <p hidden="true">{{ deleteMobileNumber }}</p>
            <p>
              Do you really want to delete these Customer? This process cannot
              be undone.
            </p>
          </div>
          <div class="modal-footer justify-content-center">
            <button
              type="button"
              class="btn btn-secondary"
              data-bs-dismiss="modal"
            >
              Cancel
            </button>
            <button
              type="button"
              class="btn btn-danger"
              @click="deleteCustomer()"
            >
              Delete
            </button>
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
import "bootstrap/dist/css/bootstrap.min.css";
import "jquery/dist/jquery.min.js";
//Datatable Modules
import "datatables.net-dt/js/dataTables.dataTables";
import "datatables.net-dt/css/jquery.dataTables.min.css";
import $ from "jquery";

import axios from "axios";

export default {
  mounted() {
    let appData = JSON.parse(localStorage.getItem("app.data"));
    this.appModel = appData;

    //API Call Get All Customer
    axios.get("http://localhost:8080/customer/all").then((res) => {
      this.customers = res.data;
      $(document).ready(function () {
        $("#dtBasicExample").DataTable({
          lengthMenu: [
            [5, 10, 25, 50, -1],
            [5, 10, 25, 50, "All"],
          ],
        });
        $(".dataTables_length").addClass("bs-select");
      });
    });
  },
  data: function () {
    return {
      customers: [],
      firstName: "",
      surName: "",
      address: "",
      gender: "selected",
      mobileNumber: "",
      appModel: {
        email: "",
        roleId: "",
      },
      isProccess: false,
      isProccessMaintain: false,
      isProccessDelete: false,

      firstNameUpdate: "",
      surNameUpdate: "",
      addressUpdate: "",
      genderUpdate: " ",
      mobileNumberUpdate: "",
      deleteMobileNumber: "",
    };
  },
  methods: {
    addCustomer() {
      if (this.firstName === "") {
        alert("First name is required");
      } else if (this.surName === "") {
        alert("SurName is required");
      } else if (this.address === "") {
        alert("Address is required");
      } else if (this.gender === "selected") {
        alert("Gender is required");
      } else if (
        this.mobileNumber === "" ||
        !this.mobileNumber.startsWith("+27")
      ) {
        alert("Invalid PhoneNumber, Please follow the label");
      } else {
        const formData = {
          firstName: this.firstName,
          surname: this.surName,
          address: this.address,
          mobileNumber: this.mobileNumber,
          gender: this.gender,
          createdBy: this.appModel.email,
        };
        this.isProccess = true;
        setTimeout((_) => {
          axios
            .post("http://localhost:8080/customer/add", formData)
            .then((response) => {
              if (response.status === 201) {
                this.isProccess = false;
                alert("Customer Successfully add");
              } else {
                alert(response.data);
              }
            });
        }, 5000);
      }
    },

    editData(data) {
      this.mobileNumberUpdate = data;
      axios
        .get(`http://localhost:8080/customer/find/${data}`)
        .then((response) => {
          if (response.status === 200) {
            // set values
            this.firstNameUpdate = response.data.firstName;
            this.surNameUpdate = response.data.surname;
            this.genderUpdate = response.data.gender;
            this.addressUpdate = response.data.address;
          } else {
            console.log(response.data);
          }
        });
    },
    MaintainCustomer() {
      if (this.firstNameUpdate === "") {
        alert("First name is required");
      } else if (this.surNameUpdate === "") {
        alert("SurName is required");
      } else if (this.addressUpdate === "") {
        alert("Address is required");
      } else if (this.genderUpdate === "") {
        alert("Gender is required");
      } else if (
        this.mobileNumberUpdate === "" ||
        !this.mobileNumberUpdate.startsWith("+27")
      ) {
        alert("Invalid PhoneNumber, Please follow the label");
      } else {
        const formData = {
          firstName: this.firstNameUpdate,
          surname: this.surNameUpdate,
          address: this.addressUpdate,
          mobileNumber: this.mobileNumberUpdate,
          gender: this.genderUpdate,
          modifiedBy: this.appModel.email,
        };
        this.isProccessMaintain = true;
        setTimeout((_) => {
          axios
            .put(
              `http://localhost:8080/customer/maintain/${formData.mobileNumber}`,
              formData
            )
            .then((response) => {
              if (response.status === 200) {
                this.isProccessMaintain = false;
                alert("Customer Successfully Updated");
              } else {
                alert(response.data);
              }
            });
        }, 5000);
      }
    },
    getDeleteMobileNumber(data) {
      console.log(data);
      this.deleteMobileNumber = data;
      axios.get(`http://localhost:8080/customer/find/${data}`);
    },
    deleteCustomer() {
      console.log("deleteCustomer" + this.deleteMobileNumber);
      this.isProccessDelete = true;
      setTimeout((_) => {
        axios
          .delete(
            `http://localhost:8080/customer/delete/${this.deleteMobileNumber}`
          )
          .then((response) => {
            if (response.status === 200) {
              this.isProccessDelete = false;
              alert("Customer Successfully Deleted");
            } else {
              alert(response.data);
            }
          });
      }, 5000);
    },
  },
};
</script>
<style lang="css" scoped>
.home-page {
  background-color: #d3d3d3;
}

.btnModal {
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

.label-success {
  background-color: rgb(44, 241, 44);
  width: 60px;
  border-radius: 3px;
}

.label-warning {
  background-color: rgb(241, 139, 44);
  width: 60px;
  border-radius: 3px;
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
.modal-confirm .btn,
.modal-confirm .btn:active {
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
.modal-confirm .btn-secondary:hover,
.modal-confirm .btn-secondary:focus {
  background: #a8a8a8;
}
.modal-confirm .btn-danger {
  background: #f15e5e;
}
.modal-confirm .btn-danger:hover,
.modal-confirm .btn-danger:focus {
  background: #ee3535;
}
</style>

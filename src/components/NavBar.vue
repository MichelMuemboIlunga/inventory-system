<template>
  <!-- Navbar -->
  <nav class="navbar navbar-expand-lg navbar-light bg-dark">
    <!-- Container wrapper -->
    <div class="container">
      <!-- Navbar brand -->
      <a class="navbar-brand me-2" href="#">
        <img
          src="../assets/stockLogo.png"
          height="50"
          alt="MDB Logo"
          loading="lazy"
          style="margin-top: -1px"
        />
      </a>

      <!-- Toggle button -->
      <button
        class="navbar-toggler bg-light"
        type="button"
        data-mdb-toggle="collapse"
        data-mdb-target="#navbarButtonsExample"
        aria-controls="navbarButtonsExample"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <i class="fas fa-bars"></i>
      </button>

      <!-- Collapsible wrapper -->
      <div class="collapse navbar-collapse" id="navbarButtonsExample">
        <!-- Left links -->
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          <li class="nav-item">
            <a class="nav-link text-white" href="#" hidden="true"
              >Stock Management</a
            >
          </li>
        </ul>
        <!-- Left links -->

        <div class="d-flex align-items-center">
          <router-link to="/" class="btn px-3 me-2" id="btnLogin"
            >Login</router-link
          >
          <router-link to="/register" class="btn me-3" id="btnRegister"
            >Register</router-link
          >
        </div>
      </div>
      <!-- Collapsible wrapper -->
    </div>
    <!-- Container wrapper -->
  </nav>
  <!-- Navbar -->
</template>

<script>
export default {};
</script>

<style>
body {
  background-color: lightgrey;
}
#btnLogin {
  border: 0;
  color: white;
  border-radius: 20px;
  font-family: Poppins-Medium;
  font-size: 15px;
  text-decoration: none;
  background-color: #b721ff;
}
#btnRegister {
  border: 0;
  color: white;
  border-radius: 20px;
  font-family: Poppins-Medium;
  font-size: 15px;
  background-color: #b721ff;
  border: none;
}

#btnLogin:hover {
  background: rgb(92, 101, 235);
}
#btnRegister:hover {
  background: rgb(92, 101, 235);
}
</style>
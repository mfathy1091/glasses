<template>
<div>
        <!-- Main Sidebar Container -->
    <aside class="main-sidebar sidebar-dark-primary elevation-4">
        <!-- Brand Logo -->
        <a href="index3.html" class="brand-link">
            <!-- <img src="/img/logo.png" alt="pstic Logo" class="brand-image img-circle elevation-3" style="opacity: .8"> -->
            <span class="brand-text font-weight-light">PSTIC</span>
        </a>

        <!-- Sidebar -->
        <div class="sidebar">
            <!-- Sidebar user panel (optional) --> 
            <div class="user-panel mt-3 pb-3 mb-3 d-flex">
                <div class="image">
                    <!-- <img src="./img/profile.png" class="img-circle elevation-2" alt="User Image"> -->
                </div>
                <div class="info">
                    <a href="#" class="d-block">{{ currentUser.full_name }}</a>
                </div>
            </div>

            <!-- SidebarSearch Form -->
            <!-- <div class="form-inline">
                <div class="input-group" data-widget="sidebar-search">
                    <input class="form-control form-control-sidebar" type="search" placeholder="Search" aria-label="Search">
                    <div class="input-group-append">
                        <button class="btn btn-sidebar">
                            <i class="fas fa-search fa-fw"></i>
                        </button>
                    </div>
                </div>
            </div> -->

            <!-- Sidebar Menu -->
            <nav class="mt-2">
                <ul class="nav nav-pills nav-sidebar flex-column" data-widget="treeview" role="menu" data-accordion="false">
                    <!-- Add icons to the links using the .nav-icon class
                            with font-awesome or any other icon font library -->
                        <!-- <li class="nav-item">
                            <router-link
                            to='/add-beneficiaries' class="nav-link">
                                <i class="nav-icon fas fa-home"></i>
                                <p>Add Beneficiaries</p>
                            </router-link>
                        </li> -->
                        <li class="nav-item">
                            <router-link
                            to='/cases' class="nav-link">
                                <i class="nav-icon fas fa-home"></i>
                                <p>Cases</p>
                            </router-link>
                        </li>
                        <li class="nav-item">
                            <router-link
                            to='/beneficiaries' class="nav-link">
                                <i class="nav-icon fas fa-home"></i>
                                <p>Beneficiaries</p>
                            </router-link>
                        </li>
                        <li class="nav-item">
                            <router-link :to="{ name: 'addCase' }" class="nav-link">
                                <i class="nav-icon fas fa-plus-circle"></i>
                                <p>Add Case</p>
                            </router-link>
                        </li>
                        <li class="nav-item">
                            <router-link :to="{ name: 'pss' }" class="nav-link">
                                <i class="nav-icon fas fa-file-alt"></i>
                                <p>PSS Intake</p>
                            </router-link>
                        </li>
                        <li class="nav-header">My Pages</li>

                        <li class="nav-item">
                            <router-link :to="{name: 'workerPssLog'}" class="nav-link">
                                <i class="nav-icon fas fa-file-alt"></i>
                                <p>My January PSS Log</p>
                            </router-link>


                        <!-- <li class="nav-item">
                            <router-link to="/dashboard" class="nav-link">
                                <i class="nav-icon fas fa-tachometer-alt"></i>
                                <p>Dashboard</p>
                            </router-link>
                        </li> -->


                    <li class="nav-header">Settings</li>
                        <li class="nav-item">
                            <router-link :to="{ name: 'userSettings'}" class="nav-link">
                                <i class="nav-icon fas fa-users"></i>
                                <p>Users</p>
                            </router-link>
                        </li>
                        <li class="nav-item">
                            <router-link :to="{ name: 'settings'}" class="nav-link">
                                <i class="nav-icon fas fa-cog"></i>
                                <p>Settings</p>
                            </router-link>
                        </li>
                        <li class="nav-item">
                            <router-link :to="{ name: 'profile'}" class="nav-link">
                                <i class="nav-icon fas fa-user"></i>
                                <p>Profile</p>
                            </router-link>
                        </li>
                    <!-- <li class="nav-item">
                        <a href="#" class="nav-link">
                            <i class="nav-icon fas fa-cog"></i>
                            <p>
                                Management
                                <i class="right fas fa-angle-left"></i>
                            </p>
                        </a>
                        
                        <ul class="nav nav-treeview">
                            <li class="nav-item">
                                <router-link :to="{ name: 'userSettings'}" class="nav-link">
                                    <i class="fas fa-minus nav-icon"></i>
                                    <p>Users / Roles</p>
                                </router-link>
                            </li>
                        </ul>
                        <ul class="nav nav-treeview">
                            <li class="nav-item">
                                <router-link :to="{ name: 'settings'}" class="nav-link">
                                    <i class="fas fa-minus nav-icon"></i>
                                    <p>Settings</p>
                                </router-link>
                            </li>
                        </ul>
                    </li> -->
                    
                    <!-- @can('pss-menu') -->
                    <!-- <li class="nav-item">
                        <a href="#" class="nav-link">
                            <i class="nav-icon fas fa-th"></i>
                            <p>
                                PSS<i class="right fas fa-angle-left"></i>
                            </p>
                        </a>
                        
                        <ul class="nav nav-treeview">
                            <li class="nav-item">
                                <router-link to="/beneficiaries/check" class="nav-link">
                                    <i class="fas fa-minus nav-icon"></i>
                                    <p>Add Referral</p>
                                </router-link>
                            </li>
                        </ul>
                    </li> -->
                    <!-- @endcan -->

                    <li class="nav-item">
                        <a class="nav-link" @click="logout" href="#">
                            <i class="nav-icon fas fa-power-off red"></i>
                            <p>Logout</p>
                        </a>
                    </li>

                </ul>
            </nav>
            <!-- /.sidebar-menu -->
        </div>
        <!-- /.sidebar -->
    </aside>
</div>
</template>

<script>
export default {
    methods: {
        logout(){
			this.$store.dispatch('currentUser/logoutUser');
        }
    },
    computed:{
        currentUser: {
            get(){
                return this.$store.state.currentUser.user;
            }
        }
    },
}
</script>
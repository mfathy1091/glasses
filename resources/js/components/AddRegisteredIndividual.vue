<template>
    <div>
		<h1>1- Adding File and its Beneficiaries</h1>
		<!-- File Number Field -->
		<div class="col-4">
			<ValidationObserver v-slot="{ handleSubmit }">
				<form @submit.prevent="handleSubmit(getCasee)">
					<hr>
					<ValidationProvider name="File Number" rules="required|length:12" v-slot="{ errors }">
					<div class="form-group">
						<label for="number" class="form-label">File Number</label>
						<input v-model="caseeForm.file_number" type="text" :placeholder="mask" class="form-control">
						<span class="text-danger">{{ errors[0] }}</span>
					</div>
					</ValidationProvider>
					<button  type="submit" class="btn btn-primary">Create File</button>
				</form>
			</ValidationObserver>
		</div>

		<!-- Case Section -->
		<div v-if="showRegisterByFileNumberSection">
			
			<div class="card mt-3">
				<div class="card-header">
					<h5 class="m-0">
						File Number: {{ this.casee.file_number }}
					</h5>
				</div>
				<div class="card-body">
					<!-- Case beneficiaries -->
					<div class="card mt-3">
				<div class="card-header">
					<h3 class="card-title">
						beneficiaries under {{ this.casee.file_number }}
					</h3>

					<div class="card-tools">
						<button class="btn btn-success" @click="showCreateIndividualModal">
							Add New
						</button>
					</div>
					
				</div>

				<div class="card-body">
					<div class="card-body table-responsive p-0">
						<table class="table table-hover text-nowrap">
							<thead>
								<tr>
									<th>Name</th>
									<th>Individual ID</th>
									<th>Passport #</th>                     
									<th>Relationship</th>
									<th>Age</th>
									<th>Gender</th>
									<th>Nationality</th>
									<th>Current Phone #</th>
									<th>Modify</th>
								</tr>
							</thead>
							<tbody>
									<tr v-for="individual in caseebeneficiaries" :key="individual.id">
										<td>{{ individual.name }}</td>
										<td>{{ individual.beneficiary_id }}</td>
										<td>{{ individual.passport_number }}</td>
										<td>{{ individual.relationship.name}}</td>
										<td>{{ individual.age }}</td>
										<td>{{ individual.gender.name}}</td>
										<td>{{ individual.nationality.name}}</td>
										<td>{{ individual.current_phone_number }}</td>
										<td>
											<a href="#" @click="goToIndividualPage(individual)">
                                            	<i class="fas fa-eye"></i>
                                        	</a>
											<a href="#" @click="showEditIndividualModal(individual)">
												<i class="fa fa-edit blue"></i>
											</a>
											
											<a href="#" @click="deleteIndividual(individual.id)">
												<i class="fa fa-trash red"></i>
											</a>
										</td>
									</tr>
							</tbody>
						</table> 
					</div>
				</div>
			</div>


		</div>
	</div>

			<div class="card card-solid">
				<div class="card-header">
					<h3 class="card-title">
						beneficiaries Linked to {{ this.casee.file_number }}
					</h3>

					<div class="card-tools">
						<button class="btn btn-primary" @click="showCreateIndividualModal">
							<i class="fas fa-link"></i> Link beneficiaries
						</button>
					</div>
					
				</div>
				<div class="card-body pb-0">
					<div class="row">
						
						<div v-for="individual in caseebeneficiaries" :key="individual.id"
						class="col-12 col-lg-4 col-sm-6 col-md-4 d-flex align-items-stretch flex-column">
							<div class="card bg-light d-flex flex-fill">
								<div class="card-header text-dark border-bottom-0">
									<h6 class="lead">{{ individual.name }}</h6>
								</div>
								<div class="card-body pt-2">
									<div class="row">
										<div class="col-12">
											
											<span class="text-muted text-sm"><b>Personal Details</b></span>
											<ul class="mb-0 fa-ul text-muted">
												<li><span class="fa-li"></span><b>Passport #:</b> {{ individual.passport_number }}</li>
												<li><span class="fa-li"></span><b>Age:</b> {{ individual.age }}</li>
												<li><span class="fa-li"></span><b>Gender:</b> {{ individual.gender.name }}</li>
												<li><span class="fa-li"></span><b>Nationality:</b> {{ individual.nationality.name }}</li>
												<li><span class="fa-li"></span><b>Phone #:</b> {{ individual.current_phone_number}}</li>
											</ul>
											<hr>
											<span class="text-muted text-sm"><b>File Related Details</b></span>
											<span class="clickable text-red ml-2" @click="showCreateIndividualModal">
												<i class="fas fa-unlink"></i> Unlink
											</span>

											<ul class="mb-0 fa-ul text-muted">
												<li><span class="fa-li"></span><b>Individual ID:</b> {{ individual.beneficiary_id }}</li>
												<li><span class="fa-li"></span><b>Relationship:</b> {{ individual.relationship.name}}</li>
											</ul>

										</div>
									</div>
								</div>
								<div class="card-footer">
										<button class="btn btn-sm btn-primary" @click="goToIndividualPage(individual)">
											<i class="fas fa-eye"></i> View Page
										</button>
								</div>
							</div>
						</div>

					</div>
				</div>
			</div>

			<button type="submit" class="btn btn-primary" @click="goToCreateReferralPage">Next</button>

			<!-- Choose Direct Section -->
			<!-- <ValidationObserver v-slot="{ handleSubmit }">
				<form @submit.prevent="handleSubmit(goToCreateReferralPage)" v-if="caseebeneficiaries.length">		
					<ValidationProvider name="directbeneficiary_id" rules="required" v-slot="{ errors }">
					<div class="form-group">
						<label for="directbeneficiary_id" class="form-label">Choose Direct</label>
						<select name="directbeneficiary_id" v-model="directbeneficiary_id" id="directbeneficiary_id" class="form-control" aria-placeholder="fdgfdg">
							<option v-for='beneficiary in caseebeneficiaries' :value='beneficiary.id' :key="individual.id">{{ individual.name }}</option>
						</select>
						<span class="text-danger">{{ errors[0] }}</span>
					</div>
					</ValidationProvider>

					<br>
					<button type="submit" class="btn btn-primary">Next</button>
				</form>
			</ValidationObserver> -->
			<br>
			<br>
		</div>

		
		<!-- Registered Beneficiary Modal -->
		<div class="modal fade" id="individualModal" tabindex="-1" aria-labelledby="individualModalLabel" aria-hidden="true">
			<div class="modal-dialog modal-dialog-centered">
				<div class="modal-content">
					<div class="modal-header">
						<h5 v-show="!individualEditMode" class="modal-title" id="individualModalLabel">Create New Individual</h5>
						<h5 v-show="individualEditMode" class="modal-title" id="individualModalLabel">Edit Individual</h5>
						<button type="button" class="close" data-dismiss="modal" aria-label="Close">
							<span aria-hidden="true">&times;</span>
						</button>
					</div>
					<form @submit.prevent="individualEditMode ? updateIndividual() : createIndividual()">
						<div class="modal-body">

							<div class="form-group">
								<label for="relationship_id" class="form-label">Relationship to PA</label>
								<select name="relationship_id" v-model="individualForm.relationship_id" id="relationship_id" class="form-control" :class="{ 'is-invalid': individualForm.errors.has('relationship_id') }">
									<option value='0' disabled>Choose...</option>
									<option v-for='relationship in relationships' :value='relationship.id' :key="relationship.id">{{ relationship.name }}</option>
								</select>
								<HasError :form="individualForm" field="relationship_id" />
							</div>
							
							<div class="form-group">
								<label for="beneficiary_id" class="form-label">File Beneficiary ID</label>
								<input id="beneficiary_id" v-model="individualForm.beneficiary_id" type="text" name="beneficiary_id" class="form-control">
								<HasError :form="individualForm" field="beneficiary_id" />
							</div>

							<hr class="col-8 mt-5 mb-5">
							
							<div class="form-group">
								<label for="name" class="form-label">Name</label>
								<input id="name" v-model="individualForm.name" type="text" name="name" class="form-control">
								<HasError :form="individualForm" field="name" />
							</div>

							<div class="form-group">
								<label for="passport_number" class="form-label">Passport Number</label>
								<input id="passport_number" v-model="individualForm.passport_number" type="text" name="passport_number" class="form-control">
								<HasError :form="individualForm" field="passport_number" />
							</div>

							<div class="form-group">
								<label for="age" class="form-label">Age</label>
								<input id="age" v-model="individualForm.age" type="number" name="age" class="form-control" onwheel="return false;">
								<HasError :form="individualForm" field="age" />
							</div>

							<div class="form-group">
								<label for="gender_id" class="form-label">Gender</label>
								<select name="gender_id" v-model="individualForm.gender_id" id="gender_id" class="form-control" :class="{ 'is-invalid': individualForm.errors.has('gender_id') }">
									<option value='0' disabled>Choose...</option>
									<option value="1">Male</option>
									<option value="2">Female</option>
								</select>
								<has-error :form="individualForm" field="gender_id"></has-error>
							</div>

							<div class="form-group">
								<label for="nationality_id" class="form-label">Nationality</label>
								<select name="nationality_id" v-model="individualForm.nationality_id" id="nationality_id" class="form-control" :class="{ 'is-invalid': individualForm.errors.has('nationality_id') }">
									<option value='0' disabled>Choose...</option>
									<option v-for='nationaity in nationalities' :value='nationaity.id' :key="nationaity.id">{{ nationaity.name }}</option>
								</select>
								<has-error :form="individualForm" field="nationality_id"></has-error>
							</div>

							<div class="form-group">
								<label for="current_phone_number" class="form-label">Current Phone Number</label>
								<input id="current_phone_number" v-model="individualForm.current_phone_number" type="text" name="current_phone_number" class="form-control">
								<HasError :form="individualForm" field="current_phone_number" />
							</div>


						</div>
						<div class="modal-footer">
							<button type="button" class="btn btn-danger" data-dismiss="modal">Close</button>
							<button v-show="!individualEditMode" type="submit" class="btn btn-success">Create</button>
							<button v-show="individualEditMode" type="submit" class="btn btn-primary">Update</button>
						</div>

					</form>
				</div>
			</div>
		</div>

		<!-- Case Modal -->
		<div class="modal fade" id="caseeModal" tabindex="-1" aria-labelledby="caseeModalLabel" aria-hidden="true">
			<div class="modal-dialog modal-dialog-centered">
				<div class="modal-content">
					<div class="modal-header">
						<h5 v-show="!caseeEditMode" class="modal-title" id="caseeModalLabel">Create New File</h5>
						<h5 v-show="caseeEditMode" class="modal-title" id="caseeModalLabel">Edit File</h5>
						<button type="button" class="close" data-dismiss="modal" aria-label="Close">
							<span aria-hidden="true">&times;</span>
						</button>
					</div>
					<form @submit.prevent="caseeEditMode ? updateCasee() : createCasee()">
						<div class="modal-body">
							
							<div class="form-group">
								<label for="number" class="form-label">Enter the File Number</label>
								<input id="number" v-model="caseeForm.file_number" type="text" name="number" class="form-control">
								<HasError :form="caseeForm" field="number" />
							</div>

						</div>
						<div class="modal-footer">
							<button type="button" class="btn btn-danger" data-dismiss="modal">Close</button>
							<button v-show="!caseeEditMode" type="submit" class="btn btn-success">Create</button>
							<button v-show="caseeEditMode" type="submit" class="btn btn-primary">Update</button>
						</div>

					</form>
				</div>
			</div>
		</div>
    </div>
</template>
<script>
import Form from 'vform'
import router from '../router'
import {mapActions, mapGetters} from 'vuex'
//import { required, email, integer, between, regex } from 'vee-validate/dist/rules';


import AddReferral from './AddReferral.vue'


export default {
	// name: 'CheckIndividual',
    components: {
        AddReferral,
    },
    data(){
        return {
			
			showRegisterByFileNumberSection: false,
			showAddFileNumberSection: false,
			individualEditMode: false,
			caseeEditMode: false,
            casee: '',
			isNewCasee: '',
            caseebeneficiaries: [],
			directbeneficiary_id: '',
			relationships: [],
            // nationalities: {},
			
            
            caseeForm: new Form({
				id: '',
				file_number: '',
			}),
            format: '',
            regex: '^',
            mask: 'XXX-XXCXXXXX',

			individualForm : new Form({
				id: '',
                number: '',
                passport_number: '',
                name: '',
                age: '',
                is_registered: '',
                casee_id: '1',
                beneficiary_id: '',
                gender_id: '0',
                nationality_id: '0',
                relationship_id: '0',
                current_phone_number: '',
            }),
        }
    },

    computed: {
        ...mapGetters({
            'counter': 'getCounter',
            'nationalities': 'getNationalitiesgetter',
        })
    },

    mounted() {
        let x = 1;
        this.format = this.mask.replace(/X+/g, () => '$' + x++)
        this.mask.match(/X+/g).forEach((char, key) => {
            // console.log(char.length);
            // console.log(key);

            this.regex += '(\\d{' + char.length + '})?'
            console.log(this.regex);
        });

        this.$store.dispatch('getNationalities')
    },

    watch: {
        'caseeForm.file_number'(next, prev) {
            if (next.length > prev.length) {
                this.caseeForm.file_number = this.caseeForm.file_number.replace(/[^0-9]/g, '')
            
            .replace(new RegExp(this.regex, 'g'), this.format)
            .substr(0, this.mask.length);
            }
        },
    },

    methods:{
		getNationalities(){
			this.$Progress.start();
			axios.get('/api/nationalities').then(({data}) => (this.nationalities = data.data));
			this.$Progress.finish();
		},
        geCaseebeneficiaries(){
			
			if(this.casee){
				this.$Progress.start();
				axios.get('/api/casees/'+this.casee.id+'/beneficiaries')
				.then(({data}) => {
					this.caseebeneficiaries = data.data
				});
				this.$Progress.finish();
			}else{
				this.caseebeneficiaries = []
			}
		},
		getRelationships(){
			this.$Progress.start();
			axios.get('/api/relationships/')
            .then(({data}) => {
                this.relationships = data.data
			});
			this.$Progress.finish();
		},
        showCreateIndividualModal(){
			this.individualEditMode = false;
			this.individualForm.reset()
			$('#individualModal').modal('show')
		},
		showEditIndividualModal(individual){
			this.individualEditMode = true;
			this.individualForm.reset()
			$('#individualModal').modal('show')
			this.individualForm.fill(individual)
		},
		createIndividual() {
			this.$Progress.start();
			this.individualForm.post('/api/beneficiaries')
			.then(() => {
				// success
				Fire.$emit('caseebeneficiariesChanged');
				$('#individualModal').modal('hide')
				Toast.fire({
					icon: 'success',
					title: 'Added successfully'
				})
				
				this.$Progress.finish();
			})
			.catch(() => {
				// error
				this.$Progress.fail();
			})
		},
		updateIndividual(){
			this.$Progress.start();
			this.individualForm.put('/api/beneficiaries/'+this.individualForm.id)
			.then(() => {
				// success
				Fire.$emit('caseebeneficiariesChanged');
				$('#individualModal').modal('hide')
				Swal.fire(
					'Updated!',
					'It has been updated.',
					'success'
				)
				this.$Progress.finish();
			})
			.catch(() => {
				// error
				this.$Progress.fail();
			})
		},
		deleteIndividual(id){
			Swal.fire({
				title: 'Are you sure?',
				text: "You won't be able to revert this!",
				icon: 'warning',
				showCancelButton: true,
				confirmButtonColor: '#3085d6',
				cancelButtonColor: '#d33',
				confirmButtonText: 'Yes, delete it!'
			})
			.then((result) => {
				if (result.isConfirmed) {
					this.$Progress.start();
					this.individualForm.delete('/api/beneficiaries/'+id)
					.then(() => {
						// success
						Fire.$emit('caseebeneficiariesChanged');
						Swal.fire(
							'Deleted!',
							'It has been deleted.',
							'success'
						)
						this.$Progress.finish();
					})
					.catch(() => {
						// error
						Swal("Failed!", "There was something wrong.", "warning");
					});
				}
			})
		},
		// File Methods
		


		showCreateCaseeModal(){
			this.caseeEditMode = false;
			this.caseeForm.reset()
			$('#caseeModal').modal('show')
		},
		showEditCaseeModal(casee){
			this.caseeEditMode = true;
			this.caseeForm.reset()
			$('#caseeModal').modal('show')
			this.caseeForm.fill(casee)
		},

		getCasee(){
			this.$Progress.start();
			this.caseeForm.get('/api/casees/create_or_get/')
            .then(({data}) => {
					this.casee = data.data
					Fire.$emit('caseeChanged');
					Fire.$emit('caseebeneficiariesChanged');
					this.$Progress.finish();
					this.isNewCasee = data.isNewCasee
					if(this.isNewCasee){
						Toast.fire({
							icon: 'success',
							title: 'File created successfully'
						})
					}else{
						Swal.fire(
							'File Already Exists!',
							'We fetch it to you',
							'info'
						)
					}
				if(this.casee){
					this.showRegisterByFileNumberSection = true	
				}
            })
			.catch(() => {
				// error
				this.$Progress.fail();
			})
		},

		updateCasee(){
			this.$Progress.start();
			this.caseeForm.put('/api/casees/'+this.caseeForm.id)
			.then(() => {
				// success
				Fire.$emit('caseeChanged');
				$('#caseeModal').modal('hide')
				Swal.fire(
					'Updated!',
					'It has been updated.',
					'success'
				)
				this.$Progress.finish();
			})
			.catch(() => {
				// error
				this.$Progress.fail();
			})
		},
		deleteCasee(id){
			Swal.fire({
				title: 'Are you sure?',
				text: "You won't be able to revert this!",
				icon: 'warning',
				showCancelButton: true,
				confirmButtonColor: '#3085d6',
				cancelButtonColor: '#d33',
				confirmButtonText: 'Yes, delete it!'
			})
			.then((result) => {
				if (result.isConfirmed) {
					this.$Progress.start();
					this.caseeForm.delete('/api/casees/'+id)
					.then(() => {
						// success
						Fire.$emit('caseeChanged');
						Swal.fire(
							'Deleted!',
							'It has been deleted.',
							'success'
						)
						this.$Progress.finish();
					})
					.catch(() => {
						// error
						Swal("Failed!", "There was something wrong.", "warning");
					});
				}
			})
		},
		// goToIndividualPage(){
		// 	router.push({ path: '/beneficiaries/'+this.directbeneficiary_id })
		// },

		goToIndividualPage(individual){
			router.push({ path: '/beneficiaries/'+individual.id })
		},

		goToCreateReferralPage(){
			router.push({ path: `/casees/${this.casee.id}/referrals/create/` })
		},
    },
	created(){
		Fire.$on('caseeChanged', () => {
			this.geCaseebeneficiaries();
		});
		Fire.$on('caseebeneficiariesChanged', () => {
			this.geCaseebeneficiaries();
		});
		this.getRelationships();
		// this.getNationalities();
	}
}
</script>
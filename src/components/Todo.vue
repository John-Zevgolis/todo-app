<template>
	<div class="container">
		<div class="row">
			<div class="col-12">
				<h2 class="text-center mt-5">My Vue Todo App</h2>

				<!-- Input -->
				<div class="d-flex">
					<input type="text" placeholder="Enter task" class="form-control" v-model="task">
					<button class="btn btn-warning rounded-0" @click="submitTask">SUBMIT</button>
				</div>

				<!-- Task Table -->

				<table class="table table-bordered mt-5">
				  <thead>
				    <tr>
				      <th scope="col">Task</th>
				      <th scope="col">Status</th>
				      <th scope="col" class="text-center">Edit</th>
				      <th scope="col" class="text-center">Delete</th>
				    </tr>
				  </thead>
				  <tbody>
				    <tr v-for="(task, index) in tasks" :key="`task-${index}`">
				      <td>
				      	<span :class="{finished: task.status === 'finished'}">{{task.name}}</span>
				      </td>
				      <td style="width: 120px;">
				      	<span 
				      		class="pointer" 
				      		:class="{'text-danger': task.status === 'to-do',
				      		'text-warning': task.status === 'in-progress',
				      		'text-success': task.status === 'finished'}"
				      		@click="changeStatus(index)">{{firstCharUpper(task.status)}}
				      	</span>
				      </td>
				      <td>
				      	<div class="text-center">
					      	<button class="icon-btn" >
					      		<font-awesome-icon icon="pen" @click="editTask(index)" />
					      	</button>
					    </div>
				      </td>
				      <td>
				      	<div class="text-center">
					      	<button class="icon-btn" @click="deleteTask(index)">
					      		<font-awesome-icon icon="trash" />
					      	</button>
					    </div>
				      </td>
				    </tr>
				  </tbody>
				</table>
			</div>
		</div>
	</div>
</template>

<script>
	export default {
		data() {
	    	return {
	    		task: '',
	    		editedTask: null,
	    		availableStatuses: [
	    			'to-do',
	    			'in-progress',
	    			'finished'
	    		],
	    		tasks: [
	    			{
	    				name: 'Steal bananas from the store.',
	    				status: 'to-do'
	    			},
	    			{
	    				name: 'Eat 1kg chocolate in 1 hour',
	    				status: 'in-progress'
	    			}
	    		]
	    	}
	    },
	    methods: {
	    	submitTask() {
	    		if(this.tasks.length === 0) return;

	    		if(this.editedTask === null) {
	    			this.tasks.push({
	    				name: this.task,
	    				status: 'to-do'
	    			});
	    		} else {
	    			this.tasks[this.editedTask].name = this.task;
	    			this.editedTask = null;
	    		}
    			
    			this.task = '';
	    	},
	    	deleteTask(index) {
	    		this.tasks.splice(index, 1);
	    	},
	    	editTask(index) {
	    		this.task = this.tasks[index].name;
	    		this.editedTask = index;
	    	},
	    	changeStatus(index) {
	    		let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
	    		if(++newIndex > 2) newIndex = 0;
	    		this.tasks[index].status = this.availableStatuses[newIndex];
	    	},
	    	firstCharUpper(status) {
	    		return status.charAt(0).toUpperCase() + status.slice(1);
	    	}
	    }
	}
</script>

<style lang="scss" scoped>
	button {
		&.icon-btn {
			background: transparent;
			border: none;
		}
	}

	button,.form-control {
		&:focus,&:active,&:active:focus {
			box-shadow: none;
		}
	}

	.form-control {
		&:focus,&:active,&:active:focus {
			border-color: #ced4da;
		}
	}

	.pointer {
		cursor: pointer;
	}

	.finished {
		text-decoration: line-through;
	}
</style>
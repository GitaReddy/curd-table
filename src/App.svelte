<script>
	import { createEventDispatcher } from 'svelte';
  
	// Sample data
	let students = [
	  { id: 1, name: 'John Doe', age: 20 },
	  { id: 2, name: 'Jane Smith', age: 22 }
	];
  
	// Form fields
	let formName = '';
	let formAge = '';
	let selectedStudent = null;
  
	// Error messages
	let error = '';
  
	// Event dispatcher
	const dispatch = createEventDispatcher();
  
	function addStudent() {
	  if (formName && formAge) {
		const newStudent = {
		  id: students.length + 1,
		  name: formName,
		  age: formAge
		};
		students = [...students, newStudent];
		resetForm();
	  } else {
		error = 'Please enter both name and age.';
	  }
	}
  
	function editStudent() {
	  if (selectedStudent && formName && formAge) {
		const updatedStudents = students.map(student => {
		  if (student.id === selectedStudent.id) {
			return {
			  ...student,
			  name: formName,
			  age: formAge
			};
		  }
		  return student;
		});
		students = updatedStudents;
		resetForm();
	  } else {
		error = 'Please select a student and enter both name and age.';
	  }
	}
  
	function deleteStudent(id) {
	  students = students.filter(student => student.id !== id);
	  resetForm();
	}
  
	function selectStudent(student) {
	  selectedStudent = student;
	  formName = student.name;
	  formAge = student.age;
	  error = '';
	}
  
	function resetForm() {
	  selectedStudent = null;
	  formName = '';
	  formAge = '';
	  error = '';
	}
  </script>
  
  <main>
	<h2>Student Table</h2>
  
	<table>
	  <thead>
		<tr>
		  <th>ID</th>
		  <th>Name</th>
		  <th>Age</th>
		  <th>Actions</th>
		</tr>
	  </thead>
	  <tbody>
		{#each students as student}
		  <tr on:click={() => selectStudent(student)}>
			<td>{student.id}</td>
			<td>{student.name}</td>
			<td>{student.age}</td>
			<td><button on:click={() => deleteStudent(student.id)}>Delete</button></td>
			<td><button type="button" on:click={editStudent}>Edit</button></td>
		</tr>
		{/each}
	  </tbody>
	  
	</table>
  
	<form>
	  <h3>{selectedStudent ? 'Edit Student' : 'Add Student'}</h3>
	  <label>
		Name:
		<input type="text" bind:value={formName} />
	  </label>
	  <label>
		Age:
		<input type="number" bind:value={formAge} />
	  </label>
	  {#if error}
		<p>{error}</p>
	  {/if}
	  {#if selectedStudent}
		<button type="button" on:click={editStudent}>Edit</button>
	  {:else}
		<button type="button" on:click={addStudent}>Add</button>
	  {/if}
	  <button type="button" on:click={resetForm}>Cancel</button>
	</form>
  </main>
  
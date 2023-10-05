<template>
    <div :class="`panel-custom-form ${(width<30?'small':'large')} ${showHeader?'has-header':''}`">
		<form @submit.prevent="submitForm" v-if="!form_submitted">
			<div class="field">
				<label>Full Name</label>
				<input 
					v-model="form_name"
					type="text"
					placeholder="Enter your name" 
				/>
			</div>
			<div class="field">
				<label>Email</label>
				<input 
					v-model="form_email"
					type="email"
					placeholder="Enter your email" 
				/>
			</div>
			<div class="field">
				<label>Message</label>
				<textarea 
					v-model="form_message"
				></textarea>
			</div>
			<input type="submit" value="Submit"/>
		</form>
		<div v-if="form_submitted">
			<h3>Form Submitted</h3>
			<p><strong>Name:</strong> {{ form_name }}</p>
			<p><strong>Email:</strong> {{ form_email }}</p>
			<p><strong>Message:</strong><br/>{{ form_message }}</p>
			<button @click="reset">Reset Form</button>
		</div>
	</div>
</template>

<script>
import { ref } from 'vue';
export default {
	props: {
		showHeader: {
			type: Boolean,
			default: false,
		},
		width: String,
		height: String,
	},
	setup(){
		const form_name = ref('');
		const form_email = ref('');
		const form_message = ref('');
		const form_submitted = ref(false);

		function submitForm(){
			form_submitted.value = true;
		}

		function reset(){
			form_submitted.value = false;
			form_name.value = '';
			form_email.value = '';
			form_message.value = '';
		}

		return { form_name, form_email, form_message, form_submitted, submitForm, reset };
	},
};
</script>

<style scoped>
.panel-custom-form {
	padding: 12px;
}

.panel-custom-form.has-header {
	padding: 0 12px;
}

.panel-custom-form .field {
	margin-bottom: 1em;
}

.panel-custom-form h3 {
	font-weight: bold;
	font-size: 1.4em;
}
</style>

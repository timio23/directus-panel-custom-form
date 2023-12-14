<template>
    <div :class="`panel-custom-form ${showHeader?'has-header':''}`">
		<v-notice type="danger" icon="warning" v-if="form_error">{{  form_error  }}</v-notice>
		<div class="custom-form" v-if="!form_submitted">
			<div class="field">
				<label>Full Name</label>
				<v-input 
					v-model="form_name"
					type="text"
					placeholder="Enter your name" 
				/>
			</div>
			<div class="field">
				<label>Email</label>
				<v-input 
					v-model="form_email"
					type="text"
					placeholder="Enter your email" 
				/>
			</div>
			<div class="field">
				<label>Message</label>
				<v-textarea 
					v-model="form_message"
				/>
			</div>
			<v-button @click="submitForm()">Save</v-button>
		</div>
		<div v-if="form_submitted">
			<h3>Form Submitted</h3>
			<p><strong>Name:</strong> {{ form_name }}</p>
			<p><strong>Email:</strong> {{ form_email }}</p>
			<p><strong>Message:</strong><br/>{{ form_message }}</p>
			<v-button @click="reset">Reset Form</v-button>
		</div>
	</div>
</template>

<script>
import { ref } from 'vue';
import { useApi } from '@directus/extensions-sdk';
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
		const form_error = ref('');
		const api = useApi();

		function submitForm(){
			form_error.value = '';
			// Validation go here
			if(form_name.value == '' || form_email.value == '' || form_message.value == '') return;
			// Action goes here
			api.post('/items/myCollection', { field_1: form_name, field_2: form_email, field_3: form_message }).then((response) => {
				form_submitted.value = true;
				console.log(response);
			}).catch((error) => {
				console.log(error);
				form_error.value = error.message;
			});
		}

		function reset(){
			form_submitted.value = false;
			form_name.value = '';
			form_email.value = '';
			form_message.value = '';
			form_error.value = '';
		}

		return { form_name, form_email, form_message, form_submitted, form_error, submitForm, reset };
	},
};
</script>

<style scoped>
.panel-custom-form {
	padding: 12px;
	overflow-y: scroll;
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

<script>
/**
 * @fileoverview Modal component
 *
 * @param {string} size - Size of the modal. small | medium | large
 * @param {string} heading - Heading for the modal
 * @param {string} icon - Icon to show in the button - will use icomoon icons
 * @param {string} submit_text - Text to show in the submit button
 * @param {string} cancel_text - Text to show in the cancel button
 * @param {boolean} complete - Check to see if all required fields are complete before enabling the submit button
 */

export default {
	props: {
		size: {
			type: String,
			default: '',
		},
		heading: {
			type: String,
			default: '',
		},
		icon: {
			type: String,
			default: '',
		},
		submit_text: {
			type: String,
			default: '',
		},
		cancel_text: {
			type: String,
			default: 'Cancel',
		},
		complete: {
			type: Boolean,
			default: false,
		}
	},
	data() {
		return {
			visible: false,
		};
	},
	methods: {
		showModal() {
			this.visible = true;
			this.$emit('show');
		},
		hideModal() {
			this.visible = false;
			this.$emit('hide');
		},
		submit() {
			this.hideModal();
			this.$emit('submit');
		},
		cancel() {
			this.hideModal();
			this.$emit('cancel');
		},
	},

};
</script>


<template>
	<div v-if="visible" class="modal-overlay">
		<div class="modal-container" :class="size">
			<div class="modal-header">
				<button
					type="button"
					@click="hideModal"
					class="close"
				>
					<i class="icon-times" />
				</button>
				<h1 class="modal-title">{{ heading }}</h1>
			</div>
			<div class="modal-body">
				<slot name="modal-content" />
			</div>
			<div class="modal-footer">
				<slot name="extra-button" />
				<button
					class="btn btn-transparent"
					@click="cancel"
				>
					{{ cancel_text }}
				</button>
				<button
					class="btn btn-primary"
					:class="{'disabled': !complete }"
					@click="submit"
				>
					<i v-if="icon" :class="icon" />
					<span class="btn-text">{{ submit_text }}</span>
				</button>
			</div>
		</div>
	</div>
</template>
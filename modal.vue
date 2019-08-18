<script>
/**
 * @fileoverview Modal component
 *
 * @param {string} heading - Heading for the modal
 * @param {string} description - Paragraph text for the modal
 * @param {boolean} close_button - Should there be a close button for the user to 'X' out of the modal? Defaults to false
 * @param {boolean} background_close - Should the user be able to click the background to close the modal? Defaults to false
 * @param {string} image - Image to show in the modal
 * @param {string} icon - Icon to show in the modal
 * @param {string} confirm_text - Text to show in the confirm button
 * @param {string} decline_text - Text to show in the decline button
 * @param {string} checkbox_text - Text to show by the checkbox
 * @param {boolean} checkbox_required - Is the checkbox required to confirm this modal? Defaults to false
 */

export default {
	props: {
		heading: {
			type: String
        },
        description: {
            type: String
        },
        close_button: {
			type: Boolean,
			default: false
        },
        background_close: {
            type: Boolean,
			default: false
        },
        image: {
			type: String
        },
        icon: {
			type: String
        },
        confirm_text: {
            type: String
        },
        decline_text: {
            type: String
        },
        checkbox_text: {
            type: String
        },
        checkbox_required: {
            type: Boolean,
            default: false
        }
	},
    methods: {
       showModal() {
            this.visible = true;
            this.$emit('show');
        },
        closeModal() {
            this.visible = false;
            this.$emit('close');
        },
        confirm() {
            this.closeModal();
            this.$emit('confirm', {checkbox: true});
        },
        decline() {
            this.closeModal();
            this.$emit('decline');
        },
        background_click() {
            if(this.background_close == true) {
                this.closeModal();
            }
        }
    },
    data() {
        return {
            visible: false,
            check: false
        }
    },
};
</script>


<template>
  <div v-if="visible" class="modal-overlay" @click.self="background_click">
    <div class="modal-container">
        <div v-if="close_button == true" class="close" @click="closeModal">
            <i class="icon-times"></i>
        </div>
        <div v-if="image" class="mb20">
            <img :src='image' />
        </div>
        <div class="icon mb20" v-if="icon">
            <i :class='icon' />
        </div>
        <h1 class="mb20">{{heading}}</h1>
        <p>{{description}}</p>
        <label v-if="checkbox_text" class="radio-check mt10">
            <input type="checkbox" v-model="check" name="check"/> {{checkbox_text}}
        </label>
        <div v-if="decline_text || confirm_text" class="buttons mt20">
            <button v-if="decline_text" class="btn btn-primary btn-primary--gray" @click="decline">{{decline_text}}</button>
            <button v-if="confirm_text && !checkbox_required" class="btn btn-primary" @click="confirm">{{confirm_text}} </button>
            <button v-if="confirm_text && checkbox_required" class="btn btn-primary" @click="confirm" :class="{disabled: !check}">{{confirm_text}}</button>
        </div>
    </div>
  </div>
</template>
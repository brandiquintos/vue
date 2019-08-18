<script>
/**
 * @fileoverview Vue component for paginating through a list of items
 *
 * @param {number} total_items - The total number of items to be paginated through
 * @param {number} items_per_page - The number of items to show per page
 * @param {number} max_visible_buttons - The maximum number of buttons to show at a time
 */

export default {
	props: {
		total_items: {
			type: Number,
			default: 0
		},
		items_per_page: {
			type: Number,
			default: 20
		},
		max_visible_buttons: {
			type: Number,
			default: 10
		}
	},
	data() {
		return {
			page_number: 0,
		};
	},
	methods: {
		first_page() {
			this.page_number = 0;
		},
		next_page() {
			this.page_number++;
		},
		prev_page() {
			this.page_number--;
		},
		last_page() {
			this.page_number = this.total_pages -1;
		},
	},
	computed: {
		total_pages() {
			return Math.ceil(this.total_items / this.items_per_page);
		},
		pagination_range() {
			//calculate number of pages to show at a time
			let start;
			if (this.page_number - this.max_visible_buttons / 2 <= 0) {
				start = 1;
			}
			else {
				start = Math.ceil(this.page_number - this.max_visible_buttons / 2);
			}

			let range = [];

			for (let i = 0; i < this.max_visible_buttons && i < this.total_pages; i++) {
				range.push(start + i);
			}

			return range;
		},
		offset() {
			return this.page_number * this.items_per_page;
		}
	},
	watch: {
		page_number() {
			this.$emit('click', {limit: this.items_per_page, offset: this.offset});
		}
	}
};
</script>

<template>
	<div v-if="total_pages > 1" class="pagination-wrapper">
		<div class="pagination">
			<a class="first" @click="first_page" :class="{ disabled: page_number <= 0 }"> <i class="icon-angle-double-left" /> </a>
			<a class="prev" @click="prev_page" :class="{ disabled: page_number <= 0 }"> <i class="icon-angle-left" /> </a>
			<a
				v-for="page in pagination_range"
				:key="page"
				class="page"
				@click="page_number = page - 1"
				:class="{ current: page_number == page - 1}"
			>{{ page }}</a>
			<a class="next" @click="next_page" :class="{ disabled: page_number >= total_pages - 1 }"> <i class="icon-angle-right" /> </a>
			<a class="last" @click="last_page" :class="{ disabled: page_number >= total_pages - 1 }"> <i class="icon-angle-double-right" /> </a>
		</div>
	</div>
</template>
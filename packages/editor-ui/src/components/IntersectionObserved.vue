<template>
	<span ref="observed">
		<slot></slot>
	</span>
</template>

<script lang="ts">
import type { PropType } from 'vue';
import { defineComponent } from 'vue';
import type { EventBus } from 'n8n-design-system/utils';
import { createEventBus } from 'n8n-design-system/utils';

export default defineComponent({
	name: 'IntersectionObserved',
	props: {
		enabled: {
			type: Boolean,
			default: false,
		},
		eventBus: {
			type: Object as PropType<EventBus>,
			default: () => createEventBus(),
		},
	},
	mounted() {
		if (!this.enabled) {
			return;
		}

		this.$nextTick(() => {
			this.eventBus.emit('observe', this.$refs.observed);
		});
	},
	beforeDestroy() {
		if (this.enabled) {
			this.eventBus.emit('unobserve', this.$refs.observed);
		}
	},
});
</script>

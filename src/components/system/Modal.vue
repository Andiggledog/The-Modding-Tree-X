<template>
	<portal to="modal-root">
		<transition name="modal" @before-enter="setAnimating(true)" @after-leave="setAnimating(false)">
			<div class="modal-mask" v-show="show" v-on:pointerdown.self="$emit('close')">
				<div class="modal-wrapper">
					<div class="modal-container">
						<div class="modal-header">
							<slot name="header" :shown="isVisible">
								default header
							</slot>
						</div>
						<simplebar class="modal-body">
							<branches>
								<slot name="body" :shown="isVisible">
									default body
								</slot>
							</branches>
						</simplebar>
						<div class="modal-footer">
							<slot name="footer" :shown="isVisible">
								<div class="modal-default-footer">
									<div class="modal-default-flex-grow"></div>
									<button class="button modal-default-button" @click="$emit('close')">
										Close
									</button>
								</div>
							</slot>
						</div>
					</div>
				</div>
			</div>
		</transition>
	</portal>
</template>

<script>
export default {
	name: 'Modal',
	data() {
		return {
			isAnimating: false
		}
	},
	props: {
		show: Boolean
	},
	computed: {
		isVisible() {
			return this.show || this.isAnimating;
		}
	},
	methods: {
		setAnimating(isAnimating) {
			this.isAnimating = isAnimating;
		}
	}
}
</script>

<style scoped>
.modal-mask {
	position: fixed;
	z-index: 9998;
	top: 0;
	left: 0;
	bottom: 0;
	right: 0;
	background-color: rgba(0, 0, 0, 0.5);
	transition: opacity 0.3s ease;
}

.modal-wrapper {
	position: absolute;
	left: 50%;
	top: 50%;
	transform: translate(-50%, -50%);
}

.modal-container {
	width: 640px;
	max-width: 95vw;
    max-height: 95vh;
	background-color: var(--background);
	padding: 20px;
	border-radius: 5px;
	transition: all 0.3s ease;
	text-align: left;
	border: var(--modal-border);
    box-sizing: border-box;
    display: flex;
    flex-direction: column;
}

.modal-header {
    width: 100%;
}

.modal-body {
	display: flex;
	margin: 20px 0;
    width: 100%;
    overflow-y: auto;
    overflow-x: hidden;
}

.modal-footer {
    width: 100%;
}

.modal-default-footer {
	display: flex;
}

.modal-default-flex-grow {
	flex-grow: 1;
}

.modal-enter {
	opacity: 0;
}

.modal-leave-active {
	opacity: 0;
}

.modal-enter .modal-container,
.modal-leave-active .modal-container {
	-webkit-transform: scale(1.1);
	transform: scale(1.1);
}
</style>

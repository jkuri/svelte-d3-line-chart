<script lang="ts">
	import Hero from './components/Hero.svelte';
</script>

<Hero/>

<style lang="sass" global>
	@charset 'utf8'

	@import './styles/colours'
	@import './styles/variables'

	@import 'bulma/sass/base/minireset'
	@import 'bulma/sass/utilities/all'
	@import 'bulma/sass/elements/container'
	@import 'bulma/sass/grid/columns'

	@import './styles/typography'

	@import './styles/hero'

	body, html
		width: 100%
		height: 100%
		overflow-y: hidden
		position: relative
		background: $background
		color: $text-primary
		font-family: $font-family
		font-size: $font-size
		font-smoothing: antialiased
		text-rendering: optimizeLegibility
		-webkit-font-smoothing: antialiased
		-webkit-tap-highlight-color: transparent
		-moz-osx-font-smoothing: grayscale
		-webkit-font-smoothing: antialiased

	.line-chart
		display: inline-block
		width: 100%
		height: 400px
		border: 1px solid $border-box
		border-radius: $border-radius

		svg

			.line
				fill: none
				stroke-width: 3px
				shape-rendering: geometricPrecision

			.axis

				line, path
					stroke: $border-box
					stroke-width: 1px
					shape-rendering: crispEdges
					fill: none
					stroke-dasharray: 3 3

				path
					display: none

				text
					fill: $text-primary
					font-size: 10px
</style>

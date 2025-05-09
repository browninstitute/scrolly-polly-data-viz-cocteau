<script>// ===================================================================
// PANEL COMPONENT
// Represents a single content panel in the scrollytelling experience
// ===================================================================
import { onMount } from 'svelte';
import { children } from './actions.js';
// Component props
export let props;
// Extract properties from panel definition
const { align, panelClass, data, nodes = [], steps = [] } = props;
// Reference to the panel DOM element
let panelRef;
// Initialize panel when component mounts
onMount(() => {
    // Set scrolly data for intersection observer tracking
    panelRef.scrollyData = data;
    // Add this panel to the steps array for tracking
    steps.push(panelRef);
});
</script>

<div
	class={`st-panel ${align || ''} ${panelClass || ''}`}
	bind:this={panelRef}
	use:children={nodes}
/>

<style>.st-panel {
  box-sizing: border-box;
  padding-top: 2.25rem;
  padding-bottom: 2.25rem;
  margin-bottom: 40vh;
  margin-top: 40vh;
  margin-left: auto;
  margin-right: auto;
  width: 100%;
  position: relative;
  z-index: 1;
  pointer-events: none;
  /* Chrome fix for smoother animations */
  -webkit-transform: translate3d(0, 0, 0);
  transform: translate3d(0, 0, 0);
}
.st-panel.first {
  margin-top: 100vh;
}
.st-panel.last {
  margin-bottom: 100vh;
}
.st-panel::before {
  content: "";
  background-color: var(--color-panel-background, rgba(0, 0, 0, 0.6));
  border-radius: 2px;
  position: absolute;
  z-index: -1;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  width: calc(66.66667% + 3rem);
  height: 100%;
}
.st-panel::after {
  content: "";
  display: table;
  clear: both;
}
.st-panel :global(> *) {
  font-family: ABCSerif, Book Antiqua, Palatino Linotype, Palatino, serif;
  font-size: 1.375rem;
  line-height: 1.666666667;
  color: var(--color-panel-text, #fefefe);
  padding-left: 0.875rem;
  padding-right: 0.875rem;
  margin-top: 0;
  margin-left: auto !important;
  margin-right: auto !important;
  width: 66.666667%;
  pointer-events: all;
}
.st-panel :global(> *):last-child {
  margin-bottom: 0;
}
.st-panel > :global(img) {
  max-width: 66%;
  display: block;
  margin: auto;
  height: auto;
}

@media only screen and (min-width: 1023px) {
  :global(.right) {
    margin-right: calc(27.5% - 24.75rem) !important;
    width: 49.5rem !important;
  }
  :global(.left) {
    margin-left: calc(22.5% - 24.75rem) !important;
    width: 49.5rem !important;
  }
}
@media only screen and (max-width: 61.25rem) {
  .st-panel {
    padding-top: 1.5rem;
    padding-bottom: 1.5rem;
    margin-right: auto !important;
    margin-left: auto !important;
    width: 100% !important;
  }
  .st-panel::before {
    width: calc(83.3333% + 0.75rem);
  }
  .st-panel :global(p) {
    width: 83.333333%;
    font-size: 1.125rem;
    line-height: 1.555555556;
  }
}</style>
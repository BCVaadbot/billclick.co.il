<script lang="ts">
	import { onMount } from 'svelte';
	import { asset } from '$app/paths';

	const STORAGE_KEY = 'bc-notice';

	let visible = $state(false);

	onMount(() => {
		try {
			if (localStorage.getItem(STORAGE_KEY)) return;
		} catch {
			// Storage blocked — the notice could not be remembered, so skip it
			// rather than showing it on every page view.
			return;
		}

		visible = true;
	});

	function dismiss() {
		visible = false;

		try {
			localStorage.setItem(STORAGE_KEY, '1');
		} catch {
			// Storage blocked — the notice reappears on the next visit.
		}
	}
</script>

{#if visible}
	<div class="cookie-notice" role="region" aria-label="הודעה על קובצי Cookie">
		<p>
			אנו משתמשים בקובצי Cookie לניתוח השימוש באתר. מידע נוסף ב<a
				href={asset('/assets/vaadbot/legal/privacy.pdf')}
				target="_blank"
				rel="noopener">מדיניות הפרטיות</a
			>.
		</p>
		<button type="button" class="btn cookie-btn" onclick={dismiss}>הבנתי</button>
	</div>
{/if}

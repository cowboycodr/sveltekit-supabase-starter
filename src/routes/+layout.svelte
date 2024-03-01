<script lang="ts">
	import { invalidate } from '$app/navigation';
	import { onMount } from 'svelte';

	import moment from 'moment';

	import { Toaster } from 'svelte-sonner';

	import '../app.pcss';

	export let data;

	let { supabase, session } = data;
	$: ({ supabase, session } = data);

	onMount(() => {
		const {
			data: { subscription }
		} = supabase.auth.onAuthStateChange((event, _session) => {
			if (_session?.expires_at !== session?.expires_at) {
				invalidate('supabase:auth');
			}
		});

		return () => subscription.unsubscribe();
	});

	moment.updateLocale('relativeTime', {
		relativeTime: {
			future: 'in %s',
			past: '%s ago',
			s: 'secs',
			ss: '%ss',
			m: 'a min',
			mm: '%dm',
			h: 'an hour',
			hh: '%dh',
			d: 'a day',
			dd: '%dd',
			M: 'a month',
			MM: '%dM',
			y: 'a year',
			yy: '%dY'
		}
	});
</script>

<Toaster position="top-center" />

<slot />

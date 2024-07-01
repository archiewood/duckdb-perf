<script>
	import '@evidence-dev/tailwind/fonts.css';
	import '../app.css';
	import { EvidenceDefaultLayout } from '@evidence-dev/core-components';
	export let data;
</script>

<EvidenceDefaultLayout {data} hideSidebar fullWidth hideTOC githubRepo=https://github.com/archiewood/duckdb-perf>
	<slot slot="content" />
</EvidenceDefaultLayout>

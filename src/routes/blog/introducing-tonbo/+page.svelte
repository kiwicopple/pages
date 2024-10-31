<script>
	import { CodeBlock } from 'svhighlight';

	import 'highlight.js/styles/atom-one-dark.css';
	import Structured from './structured.svelte';
	import Aschronous from './aschronous.svelte';

	let code = `
#[tonbo_record]
pub struct User {
    #[primary_key]
    name: String,
    email: Option<String>,
    age: u8,
}
`;
</script>

<div class="bg-[#E7F1BE] font-code">
	<div
		class=" min-h-screen h-fit w-full md:w-[1000px] lg:w-[1200px] m-auto md:border-x-2 md:border-b-2 border-black"
	>
		<article
			class="w-full sm:w-[600px] md:w-[800px] mx-auto relative flex flex-col text-background-dark px-10 leading-[1.618] pb-[4rem] md:border-x-0 border-background-dark sm:px-0"
		>
			<div class="mt-10">
				<h1 class="text-[2.5rem] font-medium">Introducing Tonbo</h1>
				<p class="text-[0.75rem] mb-[1rem]">August 14, 2024 by Tzu Gwo</p>
			</div>
			<p>
				We’re excited to announce that <a href="https://github.com/tonbo-io/tonbo" class="underline"
					>Tonbo</a
				> is now open source in its preview version! Tonbo is an embedded persistent database written
				in Rust. It provides essential KV-like methods: insert, filter, and range scan, making it a foundation
				for data-intensive applications, including other types of databases. It also supports type-safe
				structured data storage. For Rust developers, we offer an ORM-like macro for ease of use:
			</p>
			<CodeBlock
				language="rust"
				{code}
				showHeader={false}
				showLineNumbers={true}
				background="bg-background-dark"
				codeTextClasses="font-code text-background-light text-[0.8rem]"
				lineNumberTextClasses="font-code text-background-light"
				rounded="rounded-none"
			/>
			<p />
			<p class="mt-[2rem]">
				Tonbo enables fast and convenient querying over type-safe structured data. For example,
				integrating a query engine takes just a few hours of coding: <a
					href="https://github.com/tonbo-io/tonbo/blob/main/examples/datafusion.rs"
					class="underline">datafusion example</a
				>
				(official support for DataFusion will be included in the next release). In our preliminary benchmarks,
				Tonbo outperforms RocksDB by 2.2x in data scan scenarios, even though it's still in its early
				stages. If you’re interested in Tonbo, please star the project on GitHub, follow us on
				<a href="https://x.com/tonboio" class="underline">Twitter</a>, or join the conversation on
				<a href="https://discord.gg/xa2KyKV5" class="underline">Discord</a>.
			</p>
			<h2 class="text-[1.2rem] font-medium mt-[1.2rem]">Why We Built Tonbo?</h2>
			<p>
				Analytical tools in the <a href="https://arrow.apache.org/" class="underline"
					>Apache Arrow</a
				>
				ecosystem (such as
				<a href="https://github.com/apache/arrow-datafusion" class="underline"
					>Apache Arrow DataFusion</a
				>) provide a nice foundation for data processing. They get a great balance between
				scalability, development efficiency, and execution performance, allowing developers to build
				efficient data analysis applications within days using DataFusion.
			</p>
			<p class="mt-[1rem]">
				However, most tools focus on the read path of databases. Every data-intensive
				application using DataFusion ends up spending significant time developing its own write path
				implementation. What if we could have a write path implementation as agile and performant as
				DataFusion?
			</p>
			<p class="mt-[1rem]">
				That’s the primary design goal of Tonbo: to serve as an embedded database offering a highly
				scalable data storage engine for the Arrow ecosystem. Additionally, Tonbo also has a
				long-term goal: <b class="font-medium"
					>provide offline-first distributed data storage capabilities, support rapid and flexible
					data storage across various environments—from embedded Linux and browsers to servers—and
					integrate with multiple storage systems such as file systems, OPFS, and S3.</b
				>
			</p>
			<div class="w-full max-w-full overflow-x-auto">
				<pre
					class="inline-block p-4 font-mono text-xs whitespace-pre sm:text-sm md:text-base min-w-max leading-[1.3">

			╔═tonbo═════════════════════════════════════════════════════╗
			║                                                           ║
			║    ┌─────────client memory─┐  ┌─────────client memory─┐   ║
			║    │ ┏━━━━━━━━━━━━┓        │  │ ┏━━━━━━━━━━━━┓        │   ║
			║    │ ┃  memtable  ┃        │  │ ┃  memtable  ┃        │   ║
			║    │ ┗━━━━┳━━━━━━━┛        │  │ ┗━━━━┳━━━━━━━┛        │   ║
			║    │ ┏━━━━▼━━━━━━━┓        │  │ ┏━━━━▼━━━━━━━┓        │   ║
			║    │ ┃  memtable  ┃        │  │ ┃  memtable  ┃        │   ║
			║    │ ┗━━━━┳━━━━━━━┛        │  │ ┗━━━━┳━━━━━━━┛        │   ║
			║    │ ┏━━━━▼━━━━━━━┓        │  │ ┏━━━━▼━━━━━━━┓        │   ║
			║    │ ┃  memtable  ┃        │  │ ┃  memtable  ┃        │   ║
			║    │ ┗━━━━┳━━━━━━━┛        │  │ ┗━━━━┳━━━━━━━┛        │   ║
			║    └──────╂────────────────┘  └──────╂────────────────┘   ║
			║    ┌──────╂─client storage─┐  ┌──────╂─client storage─┐   ║
			║    │ ┏━━━━▼━━━━┓           │  │ ┏━━━━▼━━━━┓           │   ║
			║    │ ┃ parquet ┃           │  │ ┃ parquet ┃           │   ║
			║    │ ┗━━━━┳━━━━┛           │  │ ┗━━━━┳━━━━┛           │   ║
			║    └──────╂────────────────┘  └──────╂────────────────┘   ║
			║           ┣━━━━━━━━━━━━━━━━━━━━━━━━━━┛                    ║
			║    ┌──────╂────────────────────────────────server ssd─┐   ║
			║    │      ┣━━━━━━━━━━━┓                               │   ║
			║    │ ┏━━━━▼━━━━┓ ┏━━━━▼━━━━┓                          │   ║
			║    │ ┃ parquet ┃ ┃ parquet ┃                          │   ║
			║    │ ┗━━━━┳━━━━┛ ┗━━━━┳━━━━┛                          │   ║
			║    └──────╂───────────╂───────────────────────────────┘   ║
			║    ┌──────╂───────────╂────────object storage service─┐   ║
			║    │      ┣━━━━━━━━━━━╋━━━━━━━━━━━┳━━━━━━━━━━━┓       │   ║
			║    │ ┏━━━━▼━━━━┓ ┏━━━━▼━━━━┓ ┏━━━━▼━━━━┓ ┏━━━━▼━━━━┓  │   ║
			║    │ ┃ parquet ┃ ┃ parquet ┃ ┃ parquet ┃ ┃ parquet ┃  │   ║
			║    │ ┗━━━━━━━━━┛ ┗━━━━━━━━━┛ ┗━━━━━━━━━┛ ┗━━━━━━━━━┛  │   ║
			║    └──────────────────────────────────────────────────┘   ║
			║                                                           ║
			╚═══════════════════════════════════════════════════════════╝
  </pre>
			</div>
			<h2 class="text-[1.2rem] font-medium mt-[1.2rem]">How is Tonbo Designed?</h2>
			<h3 class="text-[1.1rem] font-medium mt-[1.1rem]

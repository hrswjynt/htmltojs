<script>
	let codeText = '';
	let codeInJS = '';

	function elementToJS(key, parent, element) {
		let current = parent + '_' + element.tagName + key;
		codeInJS += `${current} = document.createElement("${element.tagName.toLowerCase()}")\n`;
		for (let key in element.attributes) {
			if (!isNaN(key)) {
				codeInJS += `attr("${element.attributes[key].nodeName}", "${element.attributes[key].value}")\n`;
			}
		}

		codeInJS += ' \n';

		// hadChildren
		if (element.childNodes.length > 0) {
			for (let key in element.childNodes) {
				if (!isNaN(key)) {
					if (element.childNodes[key].nodeName == '#text') {
						codeInJS += `${current}.append("${element.childNodes[key].nodeValue}")\n`;
					} else {
						elementToJS(key, current, element.childNodes[key]);
					}
				}
			}
		}

		codeInJS += `${parent}.append("${current}")\n`;
	}
	function stringToJS(string) {
		codeInJS = '';
		let c = document.createElement('div');
		c.innerHTML = string;
		let elem = c.firstChild;
		let current = elem.tagName;
		codeInJS += `${current} = document.createElement("${elem.tagName.toLowerCase()}")\n`;
		for (let key in elem.attributes) {
			if (!isNaN(key)) {
				codeInJS += `attr("${elem.attributes[key].nodeName}", "${elem.attributes[key].value}")\n`;
			}
		}

		codeInJS += ' \n';

		// hadChildren
		if (elem.childNodes.length > 0) {
			for (let key in elem.childNodes) {
				if (!isNaN(key)) {
					if (elem.childNodes[key].nodeName === '#text') {
						codeInJS += `${current}.append("${elem.childNodes[key].nodeValue}")\n`;
					} else {
						elementToJS(key, elem.tagName, elem.childNodes[key]);
					}
				}
			}
		}
	}
	function copyText() {
		navigator.clipboard.writeText(codeElement.value);
		alert('Script copied!');
	}
</script>

<div class="w-screen h-screen flex justify-center items-center">
	<div class="w-1/2 border border-black rounded-md p-4 flex flex-col gap-2">
		<h1 class="font-semibold">HTML String to JS</h1>
		<p>Enter your html code:</p>
		<textarea id="code" rows="5" class="w-full text-sm resize-none" bind:value={codeText} />
		<button
			on:click={() => stringToJS(codeText)}
			class="px-2 py-1 bg-gray-300 rounded-md drop-shadow-md">Generate</button
		>
		<p>Result:</p>
		<div class="relative">
			<textarea
				id="codeElement"
				rows="8"
				class="w-full text-sm resize-none"
				bind:value={codeInJS}
			/>
			<button
				class="absolute px-2 py-1 bg-gray-300 top-3 right-3 rounded-md drop-shadow-md"
				on:click={copyText}>Copy</button
			>
		</div>
	</div>
</div>

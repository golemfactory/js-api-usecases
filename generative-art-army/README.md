# Generative Art Army

By: TheNFTBillboard a.k.a. mike.sylphdapps.eth

Email: social@the-nft-billboard.com

## Summary

Deterministic generative art is produced by feeding an input string to an algorithm that outputs an image, interactive 3D model, or any other form of digital art. The algorithm has a special property: when given the same input string on repeated runs, the algorithm will output an identical result each time. The amount of time it takes for a generative art algorithm to run is bounded by what the artist is trying to achieve; these algorithms can run in under a second or they can take many minutes.

Generative Art Army uses the Golem Network to speed up an artist's workflow by distributing their work-in-progress algorithm alongside randomly generated input to providers. The outputs are displayed in-browser to the artist, allowing them access to possibly thousands of outputs in the same amount of time it might take to generate a handful of outputs locally.

## Detailed Flow
1. The artist visits the Generative Art Army website and is presented with a code editor.
2. The artist writes their algorithm into the editor using JavaScript.
3. When they reach a point where they want to test the algorithm, they enter the number of desired outputs into an input box and click a "Submit to network" button.
4. The browser generates payloads that include randomly generated input strings and the algorithm's source code.
5. The browser uses yajsapi to submit the payloads to the network as tasks.
6. As the results become available, yajsapi consumes them, rendering them to the page alongside the input string that was used to create each one.
7. The artist reviews the outputs and makes changes to their code as necessary to achieve their vision.
8. Return to step 3 and repeat until the artist is satisfied with the outputs.

## Pseudocode

```
function dispatchGenerativeArtArmy(numResults) {
	const tasks = [];
	for(let i = 0; i < numResults; i++) {
		const task = new Task({
			input: Math.random().toString(),
			algorithm: document.getElementById("editor").value
		});
		tasks.push(task);
	}

	const executor = new Executor({
		resultCallback: displayResult,
		/* Additional Executor configuration */
	});
	executor.execute(tasks);
}

function displayResult(task, result) {
	const canvas = document.createElement('canvas');
	const ctx = canvas.getContext("2d");
	ctx.drawImage(result, 0, 0, 500, 500);
	
	document.getElementById("results").append(task.input);
	document.getElementById("results").append(canvas);
}
```
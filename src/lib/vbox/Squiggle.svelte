<script>
	import { fade, scale } from 'svelte/transition';
	import { createEventDispatcher, onMount } from 'svelte';
	import P5 from 'p5-svelte';

	export let sketchWidth, sketchHeight;
	export let screenLocation;

	const sketchStyle = 'border-radius: 15px; border: 5px solid red';

	export let globalPoints;
	export let saveMode = false;

	const colors = ['#4d26db', '#05a59d', '#f6921e', '#ec1d23', '#ec008b'];
	const sprites = [
		'/anim/star01.gif',
		'/anim/newblob.gif',
		'/anim/head.gif',
		'/anim/sprite03.gif',
		'/anim/drops.gif',
		/* '/anim/empty.gif', */
	];
	const bases = [
		'/anim/base/circle.gif',
		'/anim/base/cloud.gif',
		'/anim/base/cube.gif',
		'/anim/base/heart.gif',
		'/anim/base/spiky.gif',
		/* '/anim/empty.gif', */
	];

	let saveGraphic;
	let saveGraphicPath = "/frame_stairwell.png";

	let images = [];
	let baseImages = [];

	const dispatch = createEventDispatcher();

	let maxPoints = 64;
	let noDraw = false;
	let saveFlag = false;

	if (screenLocation == 13 || screenLocation == 14 || screenLocation == 15) {
		noDraw = true;
	}

	let gest;
	let cnv;
	export let spriteChoice = sprites[-1];
	export let baseChoice = bases[-1];
	export let hex = colors[0];

	function squiggleDrawn() {
		globalPoints = gest.points;
		dispatch('squiggleDrawn', [globalPoints, spriteChoice, hex, baseChoice]);
	}

	const sketch = (p5) => {

		let inc = 0.05;
		class Gesture {
			constructor(
				seed,
				colorVar,
				girth,
				cap,
				join,
				x,
				y,
				speed,
				wiggle,
				smoothness
			) {
				this.seed = seed;
				this.points = [];
				this.color = colorVar;
				this.girth = girth;
				this.cap = cap;
				this.join = join;
				this.pos = p5.createVector(x, y);
				this.vel = p5.createVector(0, 0);
				this.acc = p5.createVector(0, 0);
				this.maxSpeed = speed;
				this.wiggle = wiggle;
				this.smooth = smoothness;
				this.scl = 50;
			}

			addPoint(x, y) {
				let newPoint = p5.createVector(x, y);
				this.points.push(newPoint);
			}

			render() {
				p5.stroke('#000000');
				p5.strokeWeight(2);
				p5.strokeCap(p5.ROUND);
				p5.strokeJoin(p5.ROUND);
				p5.noFill();
				p5.push();
				p5.translate(this.pos.x, this.pos.y);
				p5.beginShape();
				this.points.forEach((p) => {
					p5.vertex(p.x, p.y);
					p5.ellipse(p.x, p.y, 7);
				});
				p5.endShape();
				p5.pop();
			}

			drawSprites(spriteChoice, baseChoice) {
				p5.tint(hex);
				if (baseChoice == "/anim/empty.gif") {
					return;
				} else {
					let img = baseImages[bases.indexOf(baseChoice)];
					
					for (let i = 1; i < this.points.length-2; i+=3) {
						if(saveMode) {
							img.pause();
							img.setFrame(0);
							p5.image(img, this.points[i].x - 30, this.points[i].y - 30, 60, 60);
						} else {
							p5.image(img, this.points[i].x - 30, this.points[i].y - 30, 60, 60);
						}
					}
				}
				if (spriteChoice == "/anim/empty.gif") {
					return;
				} else {
					let img = images[sprites.indexOf(spriteChoice)];
					p5.tint(hex);
					for (let i = 0; i < this.points.length-3; i += 4) {
						if(saveMode) {
							img.pause();
							img.setFrame(0);
							p5.image(img, this.points[i].x - 25, this.points[i].y - 25, 50, 50);
						} else {
							p5.image(img, this.points[i].x - 25, this.points[i].y - 25, 50, 50);
						}
					}
				}
			}

			update(time) {
				let x = p5.floor(this.pos.x / this.scl);
				let y = p5.floor(this.pos.y / this.scl);
				p5.noiseSeed(this.seed);
				let angle = p5.noise(x * inc, y * inc, time) * p5.TWO_PI;
				let v = p5.Vector.fromAngle(angle);
				v.setMag(1);
				this.acc.add(v);

				this.vel.add(this.acc);
				this.vel.limit(this.maxSpeed);
				this.pos.add(this.vel);
				this.acc.mult(0);

				let stillIn = false;
				this.points.forEach((p) => {
					if (
						p.x > -p5.width / 2 &&
						p.x < p5.width / 2 &&
						p.y > -p5.height / 2 &&
						p.y < p5.height / 2
					) {
						stillIn = true;
					}
				});
				stillIn = false;
				if (this.pos.x > p5.width / 2 + 200 && !stillIn) {
					this.pos.x = -p5.width / 2 - 150;
				}
				if (this.pos.x < -p5.width / 2 - 200 && !stillIn) {
					this.pos.x = p5.width / 2 + 150;
				}
				if (this.pos.y > p5.height / 2 + 200 && !stillIn) {
					this.pos.y = -p5.height / 2 - 150;
				}
				if (this.pos.y < -p5.height / 2 - 200 && !stillIn) {
					this.pos.y = p5.height / 2 + 150;
				}
			}

			normalizePoints() {
				for (let i = 1; i < this.points.length; i++) {
					this.points[i].x -= this.points[0].x;
					this.points[i].y -= this.points[0].y;
					this.points[i].x = this.points[i].x * 0.5;
					this.points[i].y = this.points[i].y * 0.5;
				}
				this.points[0].x = 0;
				this.points[0].y = 0;
			}

			drawBezier(time) {
				this.color = hex;
				p5.stroke(this.color);
				p5.strokeWeight(this.girth);
				p5.strokeCap(p5.ROUND);
				p5.strokeJoin(p5.ROUND);
				p5.noFill();
				p5.push();
				p5.translate(this.pos.x, this.pos.y);
				p5.beginShape();
				//draw first point if points exist
				if (this.points.length > 0) {
					let offsetX0 = p5.map(
						p5.noise(p5.sin(time * 20), this.seed),
						0,
						1,
						-this.wiggle,
						this.wiggle
					);
					let offsetY0 = p5.map(
						p5.noise(p5.cos(time * 20), this.seed),
						0,
						1,
						-this.wiggle,
						this.wiggle
					);
					p5.vertex(this.points[0].x + offsetX0, this.points[0].y + offsetY0);
				}
				//iterate through the rest of the points and calc control points and add bezier
				for (let i = 1; i < this.points.length - 2; i += 2) {
					let offsetX = p5.map(
						p5.noise(p5.sin(time * 20 + i), this.seed),
						0,
						1,
						-this.wiggle,
						this.wiggle
					);
					let offsetY = p5.map(
						p5.noise(p5.cos(time * 20 + i), this.seed),
						0,
						1,
						-this.wiggle,
						this.wiggle
					);
					let x1 = (this.points[i].x + this.points[i + 1].x) / 2;
					let y1 = (this.points[i].y + this.points[i + 1].y) / 2;
					let x2 = (this.points[i + 1].x + this.points[i + 2].x) / 2;
					let y2 = (this.points[i + 1].y + this.points[i + 2].y) / 2;
					let x3 = this.points[i + 2].x;
					let y3 = this.points[i + 2].y;
					p5.bezierVertex(
						x1 + offsetX,
						y1 + offsetY,
						x2 + offsetX,
						y2 + offsetY,
						x3 + offsetX,
						y3 + offsetY
					);
				}
				p5.endShape();
				p5.pop();
			}
		}

		//setup of canvas and gesture
		p5.setup = () => {
			cnv = p5.createCanvas(sketchWidth, sketchHeight);
			p5.frameRate(48);
			// seed, colorVar, girth, cap, join, x, y, speed, wiggle, smoothness
			gest = new Gesture(
				100,
				p5.color(p5.hexToRgb(hex).r, p5.hexToRgb(hex).g, p5.hexToRgb(hex).b),
				15,
				'ROUND',
				'ROUND',
				0,
				0,
				5,
				7,
				0
			);
			gest.points = globalPoints;

			for (let i = 0; i < sprites.length; i++) {
				images[i] = p5.loadImage(sprites[i]);
			}
			
			for (let i = 0; i < bases.length; i++) {
				baseImages[i] = p5.loadImage(bases[i]);
			}
			saveGraphic = p5.loadImage(saveGraphicPath);
		};


		p5.draw = () => {
			p5.clear();
			p5.background(255,255,255,30);
			if (screenLocation == 14) {
				noDraw = true;
			}
			
			if(!saveMode && !noDraw) {
				if (p5.pmouseX < p5.width && p5.pmouseX > 0) {
					if (p5.pmouseY < p5.height && p5.pmouseY > 0) {
						if (p5.mouseIsPressed && gest.points.length < maxPoints) {
							gest.addPoint(p5.pmouseX, p5.pmouseY);
						}
					}
				}
				if (gest.points.length == 0) {
					p5.textSize(40);
					p5.noStroke();
					p5.fill(225);
					p5.text('Draw Here', 50, 150);
				}
			}
			if(saveMode) {
				p5.push();
				p5.scale(0.7);
				p5.translate(70,50);
			}
			gest.drawBezier(p5.frameCount * 0.001);
			gest.drawSprites(spriteChoice, baseChoice);

			if (saveMode) {
				p5.pop();
				p5.tint(255);
				p5.image(saveGraphic, -5, -5, sketchWidth+5, sketchHeight+10);
			}

			if(saveFlag) {
				saveFlag = false;
				p5.saveCanvas('mySquiggle', 'jpg');
			}
		};

		//handles reset
		p5.mousePressed = () => {
			if (!noDraw  && !saveMode) {
				if (p5.pmouseX < p5.width && p5.pmouseX > 0) {
					if (p5.pmouseY < p5.height && p5.pmouseY > 0) {
						gest.points = [p5.createVector(p5.pmouseX, p5.pmouseY)];
					}
				}
			}
		};

		p5.hexToRgb = (hex) => {
			var result = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(hex);
			return result
				? {
						r: parseInt(result[1], 16),
						g: parseInt(result[2], 16),
						b: parseInt(result[3], 16),
				  }
				: null;
		};
	};

	//makes sure drawing works on mobile
	onMount(() => {
		function detectDrawing(e) {
			// @ts-ignore
			if (e.target.className === 'p5Canvas') {
				e.preventDefault();
				if (e.type === 'touchend' || e.type === 'mouseup') {
					squiggleDrawn();
				}
			}
		}
		document.body.addEventListener('touchend', detectDrawing, {
			passive: false,
		});
		document.body.addEventListener('mouseup', detectDrawing, {
			passive: false,
		});
		document.body.addEventListener('touchmove', detectDrawing, {
			passive: false,
		});
		document.body.addEventListener('touchstart', detectDrawing, {
			passive: false,
		});
	});

	export const saveSketch = async () => {
    // Convert the Data URL to an ArrayBuffer
    const dataUrl = cnv.canvas.toDataURL();
    const response = await fetch(dataUrl);
    const blob = await response.blob();


    // Create a File object from the blob
    const file = new File([blob], "squiggle.png", { type: "image/png" });

    const data = {
        text: 'Check out my squiggle!',
        files: [file],
        title: "StairWELL"
    };

    if (navigator.share && typeof navigator.share === 'function') {
        try {
            await navigator.share(data);
        } catch (error) {
            console.error('Sharing failed:', error);
        }
    } else {
		saveFlag = true;
    }
};
</script>

<main in:fade>
	{#if screenLocation === 13}
		<div>
			{#each sprites as sprite}
				<input
					bind:group={spriteChoice}
					on:change={squiggleDrawn}
					name="spriteChoice"
					type="radio"
					value={sprite}
					style="background-image: url({sprite}); background-color: white;"
				/>
			{/each}
		</div>
		<div>
			{#each bases as base}
				<input
					bind:group={baseChoice}
					on:change={squiggleDrawn}
					name="baseChoice"
					type="radio"
					value={base}
					style="background-image: url({base}); background-color: white;"
				/>
			{/each}
		</div>
		<div>
			{#each colors as color}
				<input
					name="hex"
					type="radio"
					value={color}
					style="background-color: {color};"
					bind:group={hex}
					on:change={squiggleDrawn}
				/>
			{/each}
		</div>
	{/if}
	<P5 {sketch} />
</main>

<style>
	main {
		margin: 0 auto;
		aspect-ratio: 1;
		display: flex;
		flex-flow: column;
		justify-content: center;
		align-items: center;
		width: 95%;
	}

	main > div {
		display: grid;
		place-items: center;
		grid-template-columns: repeat(5, 1fr);
		gap: 0.5rem;
		grid-template-rows: 1fr;
		padding: 0.5rem;
		width: 100%;
	}

	input[type='radio'] {
		width: 3rem;
		height: 3rem;
		appearance: none;
		aspect-ratio: 1;
		outline: none;
		border: none;
		border-radius: 5px;
		background-position: center;
		background-size: contain;
		background-repeat: no-repeat;
	}

	input[type='radio']:checked {
		outline: solid 3px white;
		outline-offset: 2px;
	}

	:global(.p5Canvas) {
		border-radius: 15px;
	}
</style>

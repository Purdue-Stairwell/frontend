

<script>
import { fade } from 'svelte/transition';
import { createEventDispatcher, onMount } from 'svelte';
import P5 from 'p5-svelte';

export let sketchWidth, sketchHeight;

export let globalPoints;

const dispatch = createEventDispatcher();

let maxPoints = 24;
let noDraw = false;

let gest;
let cnv;
let hex;

function squiggleDrawn() {
    globalPoints = gest.points;
    dispatch('squiggleDrawn', globalPoints);
}


const sketch = (p5) => {
    let inc = 0.05;

    class Gesture {
        constructor(seed, colorVar, girth, cap, join, x, y, speed, wiggle, smoothness) {
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
            p5.stroke(this.color);
            p5.strokeWeight(this.girth);
            p5.strokeCap(p5.ROUND);
            p5.strokeJoin(p5.ROUND);
            p5.noFill();
            p5.push();
            p5.translate(this.pos.x, this.pos.y);
            p5.beginShape();
            this.points.forEach((p) => {
                p5.vertex(p.x, p.y);
            });
            p5.endShape();
            p5.pop();
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
            if (p.x > -p5.width / 2 && p.x < p5.width / 2 && p.y > -p5.height / 2 && p.y < p5.height / 2) {
                stillIn = true;
            }
            });
            if (!stillIn) {
            console.log(stillIn);
            }
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
        p5.frameRate(24);

        hex = "#FF0000";
        // seed, colorVar, girth, cap, join, x, y, speed, wiggle, smoothness
        gest = new Gesture(
            100,
            p5.color(p5.hexToRgb(hex).r, p5.hexToRgb(hex).g, p5.hexToRgb(hex).b),
            15,
            "ROUND",
            "ROUND",
            0,
            0,
            5,
            5,
            5
        );
        gest.points = globalPoints;
    }

    let index = 0;

    p5.draw = () => {
        p5.background(255);
            if (p5.pmouseX < p5.width && p5.pmouseX > 0) {
                if (p5.pmouseY < p5.height && p5.pmouseY > 0) {
                    if (p5.mouseIsPressed && gest.points.length < maxPoints) {
                        gest.addPoint(p5.pmouseX, p5.pmouseY);
                        index++;
                        if (index > maxPoints) {
                            index = 0;
                        }
                    }
                }
            }
            gest.drawBezier(p5.frameCount * 0.001, 0);
    }

    p5.updateColor = () => {
        hex = "#FF0000";
        gest.color = p5.color(p5.hexToRgb(hex).r, p5.hexToRgb(hex).g, p5.hexToRgb(hex).b);
    }

    //handles reset
    p5.mousePressed = () => {
        if (!noDraw) {
            if (p5.pmouseX < p5.width && p5.pmouseX > 0) {
                if (p5.pmouseY < p5.height && p5.pmouseY > 0) {
                    gest.points = [];
                }
            }
        }
    }



    p5.hexToRgb = (hex) => {
        var result = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(hex);
        return result
            ? {
                    r: parseInt(result[1], 16),
                    g: parseInt(result[2], 16),
                    b: parseInt(result[3], 16),
            }
            : null;
    }
}

//makes sure drawing works on mobile
  onMount(() => {
        document.body.addEventListener(
          "touchstart",
          (e) => {
            // @ts-ignore
            if (e.target.className === 'p5Canvas') {
                e.preventDefault();
            }
          },
          { passive: false }
        );
        document.body.addEventListener(
          "touchend",
          (e) => {
            // @ts-ignore
            if (e.target.className === 'p5Canvas') {
                e.preventDefault();
                squiggleDrawn();
            }
          },
          { passive: false }
        );
        document.body.addEventListener(
          "touchmove",
          (e) => {
            // @ts-ignore
            if (e.target.className === 'p5Canvas') {
                e.preventDefault();
            }

          },
          { passive: false }
        );
  });
</script>

<main in:fade>
    <P5 {sketch} />
</main>

<style>
    main {
        margin: 25px;
        aspect-ratio: 1;
        border-radius: 15px;
        background-color: rgb(199, 138, 171);
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 24pt;
        box-shadow: 3px 3px 2px #000015
    }
</style>
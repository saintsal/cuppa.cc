---
hide:

---
# 

<canvas id='canvas'></canvas>

<div class="content">

</div>
<div id="intro">
<img src="logo.svg" class="logo">
<p>Cuppa is a protocol for decentralised collaboration.
</div>

<div id="outro">
<img src="logo.svg" class="logo">
<p>With Cuppa, we can focus on doing, not coordinating.
</div>
<script type="text/javascript">

// Drawing with text. Ported from Generative Design book by the amazing Tim Holman - http://www.generative-gestaltung.de - Original licence: http://www.apache.org/licenses/LICENSE-2.0

// Application variables
var position = {x: 0, y: window.innerHeight/2};
var counter = 0;
var minFontSize = 40;
var angleDistortion = 0;
var letters = "Decentralised collaboration is ephemeral.  People connect on common goals. Or just contribute where they add value.  Cuppa let's the work take it's own structure. Easy as making tea. We're boiling the kettle. Anyone fancy a cuppa?";

// Drawing variables
var canvas;
var context;
var mouse = {x: 0, y: 0, down: false}

function init() {
canvas = document.getElementById( 'canvas' );
context = canvas.getContext( '2d' );
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;
//canvas.height = document.body.clientHeight;


canvas.addEventListener('mousemove', mouseMove, false);
canvas.addEventListener('touchmove', mouseMove, false);
canvas.addEventListener('mousedown', mouseDown, false);
canvas.addEventListener('touchstart', mouseDown, false);
canvas.addEventListener('mouseup',   mouseUp,   false);
canvas.addEventListener('touchend',   mouseUp,   false);
canvas.addEventListener('mouseout',  mouseUp,  false);  
canvas.addEventListener('dblclick', doubleClick, false);

window.onresize = function(event) {
//canvas.width = window.innerWidth;
//canvas.height = window.innerHeight;
}
}

function mouseMove ( event ){
mouse.x = event.pageX || event.changedTouches[0].pageX;
mouse.y = event.pageY || event.changedTouches[0].pageY;
draw();
}

function draw() {
if ( mouse.down ) {
var d = distance( position, mouse );
var fontSize = minFontSize + d/4;
var letter = letters[counter];
var stepSize = textWidth( letter, fontSize );

if (d > stepSize) {
var angle = Math.atan2(mouse.y-position.y, mouse.x-position.x);
var color = `hsl(
${255 - Math.floor((1 * counter) % 255)},
50%, 30%
)`;
/*
"#" +(counter % 16 * 10).toString(16) + 
+(counter % 7 * 10).toString(16) + 
"00";
*/

console.log(color);
context.font = fontSize + "px Georgia";

context.save();
context.translate( position.x, position.y);
context.fillStyle = color;
context.rotate( angle );
context.fillText(letter,0,0);
context.restore();

counter++;
if (counter > 0) {
document.getElementById('intro').classList.add("fade");
}
if (counter > letters.length-1) {
//counter = 0;
document.getElementById('intro').classList.add("faded");
document.getElementById('canvas').classList.add("faded");
document.getElementById('outro').classList.add("fadein");
document.getElementById('outro').classList.add("highlight");

}

//console.log (position.x + Math.cos( angle ) * stepSize)
position.x = position.x + Math.cos(angle) * stepSize;
position.y = position.y + Math.sin(angle) * stepSize;

}
}     
}

function distance( pt, pt2 ){

var xs = 0;
var ys = 0;

xs = pt2.x - pt.x;
xs = xs * xs;

ys = pt2.y - pt.y;
ys = ys * ys;

return Math.sqrt( xs + ys );
}

function mouseDown( event ){
mouse.down = true;
position.x = event.pageX || event.changedTouches[0].pageX;
position.y = event.pageY || event.changedTouches[0].pageY;

document.getElementById('help').style.display = 'none';
}

function mouseUp( event ){
mouse.down = false;
}

function doubleClick( event ) {
canvas.width = canvas.width; 
counter = 0;
}

function textWidth( string, size ) {
context.font = size + "px Georgia";

if ( context.fillText ) {
return context.measureText( string ).width;
} else if ( context.mozDrawText) {
return context.mozMeasureText( string );
}

};

init();
</script>


<style>

.help{
opacity: 0.5;
}

.fadein {
opacity: 0;
animation-name: fadeIn;
-webkit-animation-name: fadeIn; 
animation-duration: 6s;   
-webkit-animation-duration: 6s;
animation-timing-function: ease-in-out; 
-webkit-animation-timing-function: ease-in-out;     
visibility: visible !important; 
}

.fade {
opacity: 0.15;
transition: opacity 1s linear;
}

.faded {
opacity: 0;
transition: opacity 2s linear;
transition-delay: 2s;
}

@keyframes fadeIn {
0% {
opacity: 0.0;
}
50% {
opacity: 0;
}
70% {
opacity: 0.1;
}
100% {
opacity: 1;
}
}

@-webkit-keyframes fadeIn {
0% {
opacity: 0.0;
}
50% {
opacity: 0;
}
70% {
opacity: 0.1;
}
100% {
opacity: 1;
}
}

#intro, #outro {
position: absolute;
top: 0;
bottom: 0;
left: 0;
right: 0;
width: 90vw;
max-width: 400px;
height: 400px;
margin: auto;
text-align: center;
}


#intro{
z-index: -1;
}

.md-sidebar--primary  {
z-index: 10;
}

#outro {
z-index: 1;
display: none;
opacity: 0;
-webkit-touch-callout: none; /* iOS Safari */
-webkit-user-select: none; /* Safari */
-khtml-user-select: none; /* Konqueror HTML */
-moz-user-select: none; /* Old versions of Firefox */
-ms-user-select: none; /* Internet Explorer/Edge */
user-select: none; /* Non-prefixed version, currently
supported by Chrome, Edge, Opera and Firefox */

}

#outro.highlight {
opacity: 1;
display: block;

}

.logo {
width: 100px;
}

a#cta {
z-index: 1;
font-size: 2em;
background: #fff;
padding: 30px 30px 60px 30px;
background: linear-gradient(-45deg, #52ee7710, #e73c7e18, #cad52310, #5223d510);
background-size: 400% 400%;
animation: gradient 30s ease infinite;
}
a#cta.highlight {
background-color: #ffd18d7d;
}

canvas {
cursor: crosshair;
z-index: 0;
position: absolute;
top: 0;
left: 0;
right: 0;
bottom: 0;
width: 100%;
height: 100%; 
}


@keyframes gradient {
0% {
background-position: 0% 50%;
}
50% {
background-position: 100% 50%;
}
100% {
background-position: 0% 50%;
}
}
</style>

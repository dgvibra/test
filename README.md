<h1>Applicant Technical Test</h1>

<p>Una vez resulto el test, se debe enviar la dirección del repositorio público con el código fuente a la siguiente dirección: dario.gimenez@vibragaming.com.</p>
<p><a href="http://games.spieldev.com/applicant-test/" target="_blank">Ver demo</a></p>

<h2>Objetivo</h2>
<p>El objetivo del presente test de aptitud es comprobar la manera en la que el aspirante al puesto de desarrollador resuelve el problema planteado. Se tendrán en cuenta diversos aspectos técnicos al momento de evaluar el desarrollo. A saber:</p>

<ul>
	<li>Comprensión de la consigna.</li>
	<li>Nivel técnico.</li>
	<li>Calidad del código escrito.</li>
	<li>Bugs y/o excepciones.</li>
	<li>Tiempos de entrega.</li>
</ul>

<h2>Consigna</h2>
<p>Se debe desarrollar un juego de slot que cumpla con los siguientes requisitos:</p>
<ul>
	<li>El juego debe ser jugable desde un browser desktop o mobile.</li>
	<li>El juego debe desarrollarse en HTML5 Canvas. Preferentemente, pero no excluyente, utilizando alguno de los siguientes frameworks: PixiJS, Phaser, CreateJS.</li>
	<li>El juego debe mostrar un preloader de los assets del mismo.</li>
	<li>Una vez cargados los assets del juego, el usuario debe poder realizar un spin, y ver el resultado de la jugada.</li>
	<li>El usuario debe poder volver a jugar luego de mostrarse el resultado de la jugada.</li>
</ul>

<h2>Flow del juego</h2>
<ol>
	<li>Se lanza el juego en un browser.</li>
	<li>Se piden los reels a la api.</li>
	<li>Se piden las paylines a la api.</li>
	<li>Se pide el paytable a la api.</li>
	<li>Se realiza un spin (giro de reels) presionando el botón spin. El spin debe durar en promedio 3 segundos desde que comenzaron a girar los reels hasta que se detienen.</li>
	<li>Se detienen los reels mostrando el layout sorteado.</li>
	<li>En el caso de haber premios, se muestran las líneas ganadoras sobre los reels y el total ganado en la ventana correspondiente.</li>
	<li>Si hubieran líneas ganadoras, quedan resaltadas hasta el siguiente spin. Al volver a clickear el botón spin, deben desmarcarse.</li>
	<li>Se deben poder repetir los pasos desde el punto 5.</li>
</ol>


<h2>API. Descripción y modo de uso</h2>
<p> La API necesaria para este desarrollo es un archivo javascript que debe incluirse en el html que lanza el juego. La API se auto-inicializa, por lo cual basta con sólo incluir el .js en el documento html. Por ejemplo:</p>
<p><i>&lt;script type="text/javascript" src="lib/api.js"&gt;&lt;/script&gt;</i></p>
<p>La API cuenta con una serie de métodos mediante los cuales se simula la interacción con una plataforma de casino.</p>
<p>Para hacer referencia a la API debe invocarse el objeto wrapper. Por ejemplo:</p>
<p><i>wrapper.getReels();<br>
wrapper.spin();<br></i></p>

<h3>Métodos requeridos</h3>
<p><i>getReels():Array&lt;Array&lt;number&gt;&gt;</i></p>
<p>El método getReels devuelve una matriz con los 3 reels (columnas de símbolos) que componen el juego. Estos símbolos son los que deben utilizarse en cada reel al momento de iniciar una jugada; es decir, no se puede mostrar un símbolo que no se haya definido para ese reel.</p>

<p>getPaylines():Array&lt;Array&lt;number&gt;&gt;</p>
<p>El método getPaylines devuelve una matriz con las 5 paylines que contempla el juego. Cada payline está compuesta por una celda de cada reel. Si hubiera una cantidad mínima de símbolos iguales en esas celdas (en este caso 3), se considera que es una línea ganadora.</p>
<p>Las celdas se numeran de la siguiente manera:</p>
<p>0 1 2<br>
3 4 5<br>
6 7 8</p>

<p>Líneas de pago:</p>
<ul>
	<li>Línea 1 (horizontal medio): celdas 3, 4 y 5.</li>
	<li>Línea 2 (horizontal superior): celdas 0, 1 y 2.</li>
	<li>Línea 3 (horizontal inferior): celdas 3, 4 y 5.</li>
	<li>Línea 4 (diagonal descendente): celdas 0, 4 y 8.</li>
	<li>Línea 5 (diagonal ascendente): celdas 6, 4 y 2.</li>
</ul>

<p><i>getPaytable():Array&lt;Object&gt;</i></p>
<p>El método getPaytable devuelve un listado de objetos que define cuánto paga cada combinación de símbolos. En este caso la cantidad mínima de símbolos necesarios es 3.
Cada elemento del listado contiene los siguientes datos:</p>
<ul>
	<li>symbol: string; es el identificador del símbolo.</li>
	<li>prize: number; es el premio que paga el símbolo con 3 ocurrencias.</li>
</ul>
<p><b>Nota:</b> No es estrictamente necesario llamar a este método, ya que la API se encarga de calcular las líneas ganadoras.</p>

<p><i>spin():Object</i></p>
<p>El método spin debe llamarse al momento de generar una jugada nueva. El usuario presiona el botón spin, que da comienzo al giro de los reels, e inmediatamente debe llamarse a éste método.</p>
<p>El método spin devuelve un objeto con los siguientes datos:</p>
<ul>
	<li>stopPoints: Array&lt;number&gt;; son las posiciones en que debe detenerse cada reel.</number>
	<li>layout: Array&lt;string&gt;; es el layout sorteado. Se muestran todos los símbolos sorteados de corrido, siendo el primero el correspondiente a la celda 0 y el último el correspondiente a la celda 8.</li>
	<li>reelsLayout: Array&lt;Array&lt;number&gt;&gt;; es el layout sorteado. A diferencia del método layout se recibe separado por reels (columnas). Sirve para comprobar de manera rápida cómo debe quedar conformado el layout en pantalla. 
	<li>prizes: Array&lt;Object&gt;; son las líneas ganadoras. El objeto de cada elemento del array contiene los siguientes datos:
		<ul>
			<li>lineId: number; es el id de la línea que se está pagando.</li>
			<li>symId: string; es el identificador del símbolo que se está pagando.</li>
			<li>winnings: number; es el premio pagado para esa línea.</li>
		</ul>
	</li>
	<li>winnings: number; es la suma de los pagos de todas las líneas ganadoras.</li>

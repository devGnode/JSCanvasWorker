# Code page 437 IBM speed Tuto de 2014 :fr:
#### Convertir image cp437 en sprite puis en hexa

<center><img src="https://github.com/devGnode/JSCanvasWorker/blob/master/js/cp437/cp43788.png?raw=true"></center>

### :one: Principe de la police matricielle ou Bitmap 
Prenons exemple sur l'image ci-desus CP437 8x8 dont chaque caractere peut-être représenter dans un tableau matricielle de 8*8 soit 64 case vide qui représente 64 pixels comme ci-desous, lorsque la case est vide sa correspondance et équal à 0 et lorsque une case et cocher sa correspondance est de 1

<center><img src="https://github.com/devGnode/JSCanvasWorker/blob/master/js/cp437/bitmap88.png?raw=true"></center>

ainsi pour le deuxième caractère nous aurions un tableau matricielle semblable à celui-ci
<center><img src="https://github.com/devGnode/JSCanvasWorker/blob/master/js/cp437/head.png?raw=true"></center>

ce qui donne en tableau :

[

0,1,1,1,1,1,1,0,<br>
1,0,0,0,0,0,0,1,<br>
1,0,1,0,0,1,0,1,<br>
1,0,0,0,0,0,0,1,<br>
1,0,1,1,1,1,0,1,<br>
1,0,0,1,1,0,0,1,<br>
1,0,0,0,0,0,0,1,<br>
0,1,1,1,1,1,1,0,<br>

]

### :two: bin to hex

[

0,1,1,1,1,1,1,0, = 01111110 = 0x7E<br>
1,0,0,0,0,0,0,1, = 10000001 = 0x81<br>
1,0,1,0,0,1,0,1, = 10100101 = 0xA5<br>
1,0,0,0,0,0,0,1, = 10000001 = 0x81<br>
1,0,1,1,1,1,0,1, = 10111101 = 0xBD<br>
1,0,0,1,1,0,0,1, = 10011001 = 0x99<br>
1,0,0,0,0,0,0,1, = 10000001 = 0x81<br>
0,1,1,1,1,1,1,0, = 01111110 = 0x7E<br>

]

[ 0x7E,0x81,0xA5,0x81,0xBD,0x99,0x81,0x7E ]

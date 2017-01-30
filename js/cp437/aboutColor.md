# **TUTO PALETTE DE COULEURS** :fr:

* Monochrome
  * 1-bit Grayscale
  * 2-bit Grayscale
  * 4-bit Grayscale
  * 8-bit Grayscale 
* Regular Palette
  * 3-bit rgb
  * 6-bit RGBrgb
* Non-Regular Palettes
  * 4-bit RGBI
  * 3-3-2-bit RGB
  
Aujourd'hui nous utilisons les couleurs 24-bit RGB qui est representer sous forme de 3 octects de 8-bits ( R G B ) dont chacun des ces bytes corresponde 
à l'intensiter de celle-ci, nous avons un byte pour le <span style="color:blue"><b>Red</b></span>, un second pour le <b> Green </b>, 
et pour finir un pour le <span style="color:blue"><b>Blue</b></span>. il peut être aussi réprésenter sous forme hexadécimal # FF FF FF &rarr; # R G B
que  nous pouvons croiser beaucoup dans le developpement web. Dans sont fonctionnement 
plus ce byte ( r, g ou  b ) se raproche de 255 et plus sa couleur deviendra  intense par example pour #FF0000 nous avons une forte intensiter pour la couleur <b>Rouge</b>,
est une intensite null pour le <u>green</u> et le <u>blue</u> la couleur rendue sera <b>Rouge</b>

Ici le but est de générer les palettes de couleurs Monochrome 1-bit.. , Regular and Non-Regular 3-bit.... en couleur 24-bit  

## :one: Monochrome palettes
  
1 bits = 2 <sup> 1 </sup> = 2 couleurs &rarr; Monochrome Display Adapter ( MDA ), CGA mod text, IBM PC...

2 bits = 2 <sup> 2 </sup> = 4 couleurs  &rarr; Original Game Boy, NeXt, Commodore Amiga...

4 bits = 2 <sup> 4 </sup> = 16 couleurs &rarr; CGA, MCGA, Amstrad CPC...

8 bits = 2 <sup> 8 </sup> = 256 couleurs Format JPEG, TIFF .... 

## :two: Regular palettes
### 3-bit 

| interger      | Binary        | Hex    |
| ------------- |:-------------:| -----: |
| 0             | 000           | #000000|
| 1             | 001           | #0000FF|
| 2             | 010           | #00FF00|
| 3             | 011           | #00FFFF|
| 4             | 100           | #FF0000|
| 5             | 101           | #FF00FF|
| 6             | 110           | #FFFF00|
| 7             | 111           | #FFFFFF|

On peut remarquer que lorsque le bit est à 1 sont intensité est à sont maximum, donc pour réaliser la palette 3-bit RGB
nous allons donc multiplier par son maximum une valeur qui est à 1.

```javascript
function rgb3( r,g,b ){
	return  ( r * 0xff ) << 0x10 | // R
            ( g * 0xff ) << 0x08 | // G
			   b * 0xff;           // B
	}
  
  var clr = [], tmp,
			i = 0;
			for(; i<0x08; i++ ){
				tmp = checkBinary( base.decbin( i ), 3 );
				clr.push( rgb3(
					tmp[0], // r
					tmp[1], // g
					tmp[2]  // b
				));
			}
```
<center><img src="https://github.com/devGnode/JSCanvasWorker/blob/master/js/cp437/3bit.png"></center>

## :three: Non-Regular palettes
### 4-bit RGBi

| interger  | Binary | comment   |
|:----------|:------:|:---------:|
| 8         | 1000   | intensité |
| 4         | 0100   | low red   |
| 12        | 1100   | high red  |
| 2         | 0010   | low green |
| 10        | 1010   | high green|
| 1         | 0001   | low blue  |
| 9         | 1001   | high blue |

Dans le tableaux ci-dessus on peut voir que nous utilisons 3 bits pour la couleur <u>rgb</u> est un quatrième bit qui correspont à sont l'intensité.
Ci-dessous le tableau de la palette 4-bit avec une première déduction 256 / 3 = 85 &rarr; 0x55, qui sera notre chiffre clef.

Bien retenir que <b>0x55 + 0x55 = 0xAA + 0x55 = 0xFF </b>, 

| interger      | Binary        | Hex    |
| ------------- |:-------------:| -----: |
| 0             | 0000          | #000000|
| 1             | 0001          | #0000AA|
| 2             | 0010          | #00AA00|
| 3             | 0011          | #00AAAA|
| 4             | 0100          | #AA0000|
| 5             | 0101          | #AA00AA|
| 6 (exception) | 0110          | #AA5500|
| 7             | 0111          | #AAAAAA|
| 8             | 1000          | #555555|
| 9             | 1001          | #5555FF|
| 10            | 1010          | #55FF55|
| 11            | 1011          | #55FFFF|
| 12            | 1100          | #FF5555|
| 13            | 1101          | #FF55FF|
| 14            | 1110          | #FFFF55|
| 15            | 1111          | #FFFFFF|

### La speudo table de vériter

| bit           | Intensity     | equal | Output |
| ------------- |:-------------:|:-----:| -----: |
| 0             | 0             | = 0   | 0x00   |
| 0             | 1             | = 1   | 0x55   |
| 1             | 0             | = 2   | 0xAA   |
| 1             | 1             | = 3   | 0xFF   |

on suivera donc cette logique avec une exception : 

 ( ( x << 1 ) | intensity ) * 0x55
 
 quand i sera égal à 6 alors on divisera juste cette somme par deux pour le <b>Green</b>:
 
  ( ( ( x << 1 ) | intensity ) * 0x55 ) / 2
 
```javascript
function rgbi( bpp, i, r,g,b, itr ){
	var add = Math.floor( 0x100/bpp );
	return ( ( r << 1 | i ) * add ) << 0x10 |
		   ( ( ( g << 1 | i ) * add )/( !i && itr == 6 ? 2 : 1 ) ) << 0x08 |
		   ( b << 1 | i ) * add;
	}
  
  
var clr = [],
		tmp  = i = 0;
		
	for(; i< 16; i++ ){
		tmp = checkBinary( base.decbin( i ), 4 );
		clr.push( rgbi( 3, parseInt( tmp[0] ), // i
					   parseInt( tmp[1] ), // r
					  parseInt( tmp[2] ), // g
						parseInt( tmp[3] ), // b
					i ) );
	}
```

<img src="https://github.com/devGnode/JSCanvasWorker/blob/master/js/cp437/4rgbi.png">

# lien externes WIKI :

&rarr; [list of color palettes]( https://en.wikipedia.org/wiki/List_of_color_palettes )

&rarr; [  list monochrome and RGB palette ](https://en.wikipedia.org/wiki/List_of_monochrome_and_RGB_palettes)

&rarr; [EGA color palettes](https://fr.wikipedia.org/wiki/Color_Graphics_Adapter)

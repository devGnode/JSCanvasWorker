#graphicalUserInterface

```javascript
  var guid = graphicalUserInterface({
    monitor: DOMHtml canvas,
    width: uint x,
    height: uint y,
    });
```
**Global :**

>###uint screen_x 
>###uint screen_y

**methods :**

>###drawImage

  parameters : DOMImage node
  <br>@return void
  
>###resetScreen 

  parameters : int color || str color
  <br>@return void

>###resize 

  parameters : uint x, uint y
  <br>@return void

>###getRawPixel

  parameters : uint offset
  <br>@return **uint** colorRGB

>###setRawPixel

  parameters : uint offset, uint color
  <br>@return void

>###getPixel

  parameters : uint x, uint y
  <br>@return **uint** colorRGB

>###setPixel

  parameters : uint x, uint y, uint color
  <br>@return void

>###getLine

  parameters : uint y, bool key
  <br>@return Array ret[ addr1, addr2,... ] if key is false
  <br>@return Array ret[ 1,2,3,.... ] if key is true
  
>###refresh

  parameters : void
  <br>@return void
  
>###intToRgb

  parameters : uint color
  <br>@return JSON { r: uint, g: uint g, b: uint b } 

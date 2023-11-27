## Laboratorio No. 5 - Robótica de Desarrollo, Cinemática Inversa.
### Integrantes: 
- Victor Manuel Dávila Castañeda.
- Manuel Felipe Carranza Montenegro.
  
## Cinemática Inversa.

En la estrategia para definir una muñeca, empleamos el método geométrico, haciendo uso de la siguiente vista isométrica:

<div>
<p style = 'text-align:center;' align="center">
<img width="704" alt="Screenshot 2023-11-26 at 18 55 45" src="https://github.com/victordavila2311/LAB5Robotica_Manuel_Victor/assets/82252851/c4e7eb38-3a63-4a7e-9c90-abce50774851">
</p>
</div>

Como se observa, el cálculo de la primera articulación se puede realizar de la siguiente manera:

$$q_{1} = atan2(x, y)$$

Posteriormente, avanzamos con la tercera articulación. Siguiendo el método, proyectamos el vector hasta la punta en el plano $x_{2}$ - $y_{2}$. El resultado obtenido es:

<div>
<p style = 'text-align:center;' align="center">
<img width="405" alt="Screenshot 2023-11-26 at 19 07 04" src="https://github.com/victordavila2311/LAB5Robotica_Manuel_Victor/assets/82252851/013c8e19-ba09-496b-8f7b-94149ebde7d6">
</p>
</div>

Primero, observamos que:

$$L = \sqrt{ \left( z_{c} - L_{1} \right)^2 + x_{c}^{2} + y_{c}^{2}}$$

Construimos un triángulo con los lados $L_{1}$, $L_{2}$ y $L_{3}$. A partir de esto, podemos aplicar el teorema del coseno y obtener:

$$L_{2} = L_{2}^{2} + L_{3}^{2} - 2 \cdot L_{2} \cdot L_{3} \cdot Cos(180 - q_{3})$$

Resolvemos la ecuación para $Cos(q_{3})$ y finalmente obtenemos el siguiente sistema de ecuaciones:

$$Cos(q_{3}) = \frac{L^{2} - \left( L_{2}^{2} + L_{3}^{2} \right)}{2 \cdot L_{2} \cdot L_{3}}$$

$$Sin(q_{3} = \sqrt{1 - Cos(q_{3})}$$

$$q_{3} = atan2(Sin(q_{3}), Cos(q_{3}))$$

Luego, examinamos la segunda y cuarta articulación. Para ello, empleamos la siguiente proyección:

<div>
<p style = 'text-align:center;' align="center">
<img width="417" alt="Screenshot 2023-11-26 at 19 32 42" src="https://github.com/victordavila2311/LAB5Robotica_Manuel_Victor/assets/82252851/72c642aa-f832-402f-8293-984e6c130abd">
</p>
</div>

De la imagen anterior, se obtiene:

$$90 = q_{2} + \alpha + \gamma \rightarrow q_{2} = \alpha + \gamma - 90$$

## Descripción de la Solución Planteada.

### dibujo espacio de trabajo
<div>
<p style = 'text-align:center;' align="center">
<a href="https://www.youtube.com/watch?v=Exu-weSjR9Y" target="_blank"><img src="https://github.com/victordavila2311/LAB5Robotica_Manuel_Victor/blob/main/imagenes%20lab%205/escritura%20completa.png" 
alt="IMAGE ALT TEXT HERE" width="260" height="319.5" border="10" /></a>
</p>
</div>

### dibujo final
<div>
<p style = 'text-align:center;' align="center">
<a href="https://youtu.be/Ou4hDWupNw4" target="_blank"><img src="https://github.com/victordavila2311/LAB5Robotica_Manuel_Victor/blob/main/imagenes%20lab%205/escritura%20completa.png" 
alt="IMAGE ALT TEXT HERE" width="260" height="319.5" border="10" /></a>
</p>
</div>

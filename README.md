## Laboratorio No. 5 - Robótica de Desarrollo, Cinemática Inversa.
### Integrantes: 
- Victor Manuel Dávila Castañeda.
- Manuel Felipe Carranza Montenegro.
- 
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


## Descripción de la Solución Planteada.


<div>
<p style = 'text-align:center;' align="center">
<a href="https://youtu.be/Ou4hDWupNw4" target="_blank"><img src="https://github.com/victordavila2311/LAB1Robotica_Manuel_Victor/blob/main/imagenes_simulacion/imagen%20implementacion.png" 
alt="IMAGE ALT TEXT HERE" width="260" height="319.5" border="10" /></a>
</p>
</div>

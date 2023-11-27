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

$$q_{1} = atan2(x, \space y)$$

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

$$q_{3} = atan2(Sin(q_{3}), \space Cos(q_{3}))$$

Luego, examinamos la segunda y cuarta articulación. Para ello, empleamos la siguiente proyección:

<div>
<p style = 'text-align:center;' align="center">
<img width="417" alt="Screenshot 2023-11-26 at 19 32 42" src="https://github.com/victordavila2311/LAB5Robotica_Manuel_Victor/assets/82252851/72c642aa-f832-402f-8293-984e6c130abd">
</p>
</div>

De la imagen anterior, se obtiene:

$$90 = q_{2} + \alpha + \gamma$$ 

$$q_{2} = \alpha + \gamma - 90$$

Para $\alpha$, construimos un triángulo rectángulo y utilizamos la función $atan2$:

$$\alpha = atan2 \left( z_{c} - L_{1}, \space \sqrt{ \left( x^{2} + y^{2} \right)} \right)$$

Igual que anteriormente, empleamos otro triángulo rectángulo para $\gamma$:

$$\gamma = atan2 \left( L_{3} \cdot Sin_{q_{3}}, \space L_{2} + L_{3} \cdot Cos(q_{3}) \right)$$

Finalmente, para la última articulación, se sabe que la suma de cada ángulo de la articulación es la orientación deseada:

$$\theta = 90 + q_{2} + q_{3} + q_{4}$$

Y al resolver para $q_{4}$, se obtiene:

$$q_{4} = \theta - \left( 90 + q_{2} + q_{3} \right)$$

## Implementación en Matlab.

El siguiente bloque de código correspondiente a la cinemática inversa se encarga de calcular los ángulos de articulación $qs$ necesarios para posicionar el extremo del robot PhantomX Pincher en ubicaciones específicas del espacio tridimensional.

```matlab
ROBOT.plot([0 0 0 0], 'notiles', 'noname');

V = [210 50 l_1;
     160 0 l_1;
     210 -50 l_1;
     200 -50 l_1+5;
    ];
M = [150 50 l_1+5;
     150 50 l_1;
     V(1,:);
     V(2,:);
     V(3,:);
     150 -50 l_1
    ];
d1=deg2rad(-120)
d2=deg2rad(-80)
R1 = [cos(d1) -sin(d1) 0;
     sin(d1) cos(d1) 0;
     0 0 1
    ];
R2 = [cos(d2) -sin(d2) 0;
     sin(d2) cos(d2) 0;
     0 0 1
    ];
V = V * R1;
M = M * R2;
T=[1 0 0 100;
    0 1 0 100;
    0 0 1 200;
    0 0 0 1
    ];

plot(V(:,1),V(:,2))
hold on
plot(M(:,1),M(:,2))
xlim([-400 400])
ylim([0 400])

% Cinemeatica Inversa
qs= [0 0 0 0];
for i = 1:length(V(:,1))+length(M(:,1))
    qs(i,:)=[0 0 0 0];
end
%calculamos todas las cinematicas inversas


for i = 1:length(V(:,1))
    x = V(i, 1);
    y = V(i, 2);
    th=atan2(y,x);
    x= x-l_3*cos(th);
    y=y-l_3*sin(th);
    z = V(i, 3);
    L = sqrt(x^2 + y^2);
    c = sqrt(L^2 + z^2);
    c3 = (c^2 - l_2^2 - l_3^2)/(2 * l_2 * l_3);
    s3 = sqrt(1 - c3^2);
    k1 = l_2 + l_3 * c3;
    k2 = l_3 * s3;
    theta = atan2(z, L) - atan2(k2, k2);
    T(1:3,4) = V(i,:)';
    qs(i+1, 1) = pi/2 - atan2(y, x);
    qs(i+1, 2) = pi/2 - theta;
    qs(i+1, 3) = atan2(sqrt(1 - c3^2), c3);
    qs(i+1, 4) = -((pi/4) - qs(i, 2) - qs(i, 3));
end

for i = length(V(:,1))+1:length(V(:,1))+length(M(:,1))
    x = M(i-length(V(:,1)), 1);
    y = M(i-length(V(:,1)), 2);
    th=atan2(y,x);
    x= x-l_3*cos(th);
    y=y-l_3*sin(th);
    z = M(i-length(V(:,1)), 3);
    L = sqrt(x^2 + y^2);
    c = sqrt(L^2 + z^2);
    c3 = (c^2 - l_2^2 - l_3^2)/(2 * l_2 * l_3);
    s3 = sqrt(1 - c3^2);
    k1 = l_2 + l_3 * c3;
    k2 = l_3 * s3;
    theta = atan2(z, L) - atan2(k2, k2);
    qs(i+1, 1) = pi/2 - atan2(y, x);
    qs(i+1, 2) = pi/2 - theta;
    qs(i+1, 3) = atan2(sqrt(1 - c3^2), c3);
    qs(i+1, 4) = -((pi/4) - qs(i, 2) - qs(i, 3));
end
```

### velocidades
Para definir las velocidades que queremos que se muevan los motores va ser igual en todos y va a ser igual a la mas lenta de cualquiera de los motores y esa misma se la vamos a asignar a todos los motores para que se muevan de la manera mas sincronizada posible entre cada punto
```matlab
for i = 2:length(q(:,1))
    vel=(q(i,:)-q(i-1,:))/t;
    vel=min(abs(vel));
    vel=mapvel(app,vel);
    sol=mapeo(app,deg2rad(q(i)))
    movermotoresbits(app, sol, vel)
    pause(5)
end
```
La funcion map vel mapea el valor de velocidad que obtenemos en rad/s y la pasamos a un valor de bits teniendo en cuenta que el fabricante nos dice que un valor de 0 en memoria es aproximadamente igual a 0 rpm y 1023 es igual a 114 rpm, el codigo es el siguiente para la funcion.

```matlab
function velbit= mapvel(app, vel)
    velrpm=vel*(60/1)*(1/(2*pi));
    velbit=round(1023*velrpm/app.maxVel);

end
```

## Descripción de la Solución Planteada.

### Dibujo Espacio de Trabajo.
<div>
<p style = 'text-align:center;' align="center">
<a href="https://www.youtube.com/watch?v=Exu-weSjR9Y" target="_blank"><img src="https://github.com/victordavila2311/LAB5Robotica_Manuel_Victor/blob/main/imagenes%20lab%205/espacio%20trabajo.png" 
alt="IMAGE ALT TEXT HERE" width="520" height="319.5" border="10" /></a>
</p>
</div>

### Dibujo Final.
<div>
<p style = 'text-align:center;' align="center">
<a href="https://youtu.be/Ou4hDWupNw4" target="_blank"><img src="https://github.com/victordavila2311/LAB5Robotica_Manuel_Victor/blob/main/imagenes%20lab%205/escritura%20completa.png" 
alt="IMAGE ALT TEXT HERE" width="400" height="319.5" border="10" /></a>
</p>
</div>

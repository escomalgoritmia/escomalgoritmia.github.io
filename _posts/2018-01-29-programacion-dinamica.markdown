---
layout: post
title:  "Introducción a la programación dinámica"
date:   2018-01-29 22:44:12 -0800
categories: dynamic_programming
---

## ¿Qué es la programación dinámica?

La programación dinámica es una técnica de diseño de algoritmos que consiste en: descomponer un problema en subproblemas de forma recursiva, posteriormente utiliza las soluciones óptimas a estos subproblemas para encontrar la solución óptima al problema original. Es importante remarcar que los problemas deben cumplir con una propiedad sumamente importante para ser resueltos mediante la programación dinámica, esta propiedad se conoce como **problemas superpuestos**. La programación dinámica aprovecha esta propiedad al guardar en una tabla la solución a cada subproblema, haciendo que cada uno de estos tenga que ser calculado solo una vez, reduciendo la complejidad dramáticamente.
{: .text-justify}

## ¿Qué problemas puede resolver esta técnica?

La técnica de programación dinámica puede resolver cualquier problema que cuente con las siguientes propiedades:
{: .text-justify}

**Subestructura óptima:** en términos simples esta propiedad consiste en que podemos construir la solución óptima a un problema a través de la solución óptima a sus subproblemas.
{: .text-justify}

**Problemas superpuestos:** esta propiedad consiste en que cierto subproblema ocurre en múltiples ocasiones al resolver el problema.
{: .text-justify}

## ¿Como utilizar esta técnica?

Para resolver un problema que utilizando la programación dinámica utilizaremos los siguientes pasos:
{: .text-justify}

1. Caracterizar la estructura de una solución óptima
2. Descomponer un problema en subproblemas de forma recursiva
3. Definir la solución óptima como una combinación de las soluciones óptimas a los subproblemas

#### Paso 1

Se debe definir un conjunto de variables que proporcionen la información necesaria para representar al problema y a sus subproblemas. Es importante que tanto el problema como sus subproblemas puedan ser representados por el mismo conjunto de variables, esto nos permitirá descomponer recursivamente en problemas cada vez mas simples.
{: .text-justify}

#### Paso 2

En este paso descomponemos cierta instancia del problema en subproblemas(problemas mas simples) de forma recursiva, usualmente se intenta descomponer el problema en todas las formas posibles. Al descomponer los problemas eventualmente llegaremos a problemas tan simples que la respuesta es trivial, a estos problemas tan simples se les conoce como casos base, y se deben tener en cuenta al diseñar cualquier algoritmo de programación dinámica.
{: .text-justify}

#### Paso 3

Una vez que el problema ha sido descompuesto en subproblemas se combinan las soluciones óptimas a estos subproblemas para encontrar la solución óptima al problema original. Una vez calculada la solución óptima esta debe ser almacenada en una tabla, esto nos permite ahorrar tiempo en caso de que necesitemos el resultado para ese problema nuevamente.
{: .text-justify}

## Problema de práctica

`Chomps es un estudiante modelo, pero sus calificaciones han empeorado gracias a que siempre llega tarde por culpa del ineficiente servicio de metro que hay en la cuidad, es por eso que te ha pedido a ti su mejor estudiante del club de algoritmia que le ayudes a encontrar la ruta de metro mas rápida. La red del metro tiene N estaciones numeradas del 1 al N, sabiendo que Chomps empieza su viaje en la estacion 1 y termina en la estacion N ¿Cual es el tiempo minimo en el que puede realizarlo?`
{: .text-justify}

#### Entrada
`La primera linea contiene el numero de estaciones de metro N, las siguientes N lineas contienen N números cada una. El j-ésimo numero de la i-ésima fila representa la cantidad de tiempo que nos toma viajar entre la i-ésima estación y la j-ésima estación, en caso de que no sea posible viajar entre dos estaciones este número sera -1.`
{: .text-justify}

#### Salida
`El tiempo minimo en que Chomps puede realizar su viaje`
{: .text-justify}

#### Ejemplo

{% highlight c++ %}

 6
 0  1  3  3 -1 -1
-1  0 -1 -1  8 -1
-1 -1  0 -1  5 -1
-1 -1 -1  0  7 -1
-1 -1 -1 -1  0  3
-1 -1 -1 -1 -1  0

{% endhighlight %}

### ¿Este problema cumple con las propiedades?

Para saber si este problema cumple con las propiedades es suficiente con observar el comportamiento al tratar de resolverlo de la forma mas fácil posible, utilizando una **búsqueda completa**, esto es tratando todas las formas posibles de llegar desde la estacion 1 a la estacion N. En el caso de ejemplo podemos representar la red del metro de la siguiente forma:
{: .text-justify}

![Red del metro]({{ "/assets/example01.png"}})
{: .image-center}

#### **Problemas superpuestos**

Para llegar desde la estación 1 hasta la estación 6 se pueden encontrar las siguientes rutas:

`1-2-5-6`  
`1-3-5-6`  
`1-4-5-6`  

Podemos observar que el subproblema de encontrar la ruta mas corta desde la estación 5 a la estación 6 tiene que ser resuelto en múltiples ocasiones, esto convierte a nuestro problema en un problema con **problemas superpuestos**.
{: .text-justify}

#### **Subestructura óptima**

Dado que hemos encontrado la forma óptima de llegar desde la estación 2 a la estación 6, desde la estación 3 a la estación 6 y desde la estación 4 a la 6, podemos calcular la solución óptima de viajar desde la estación 1 a la estación 6, de la siguiente forma:
{: .text-justify}

`f(1) = min(1 + f(2), 3 + f(3), 3 + f(4))`

Donde la función `f(i)` nos dice el tiempo mínimo de llegar desde la estación i hasta la estación N. Ahora reemplazando los subproblemas por su solución óptima tenemos el siguiente resultado:
{: .text-justify}

`f(1) = min(1 + 11, 3 + 8, 3 + 10)`

Entonces al combinar estos resultados sabemos que la solución óptima para llegar desde la estación 1 a la estación N es:
{: .text-justify}

`f(1) = min(12, 11, 13) = 11`

En conclusión decimos que el problema cuenta con la propiedad de **subestructura óptima**, ya que es posible calcular la solución óptima a través de las soluciones óptimas a sus subproblemas.
{: .text-justify}

### ¿Como diseñar un algoritmo para este problema?

Antes de comenzar a diseñar nuestra solución para este problema, supondremos que ya se ha procesado la entrada, y los datos han sido guardados en las siguientes variables:
{: .text-justify}

{% highlight c++ %}

// Numero de estaciones
int numEstaciones;

// tiempo[i][j] es el tiempo necesario para viajar entre las estaciones i y j
// si no es posible viajar entre estas estaciones tiempo[i][j] sera -1
vector<vector<int>> tiempo(numEstaciones, vector<int>(numEstaciones));

{% endhighlight %}

**Paso 1**: en este paso seleccionamos las variables que representan a nuestro problema, para este problema en particular basta con tener un entero que nos indique la estación en la que nos encontramos actualmente. El conjunto de variables que seleccionemos siempre sera traducirá a los parámetros de nuestra función recursiva. De esta manera tendremos una función con un solo parámetro que como resultado regresara el tiempo mínimo para viajar desde una estación dada a la estación N.
{: .text-justify}

{% highlight c++ %}

int tiempoMinimo(int estacion);

{% endhighlight %}

**Paso 2**: en este paso se debe analizar como dividir el problema en problemas mas simples hasta que eventualmente lleguemos a un caso trivial(caso base) al que podamos dar solución fácilmente  Comencemos por pensar en el **caso base**, en este problema el caso base es la estación N, dado a que el tiempo requerido para viajar desde la estación N a la estación N es cero.
{: .text-justify}

Ahora suponiendo que no estemos en el caso base, trataremos de llegar a la estación N(caso base) desde la estación actual utilizando todas las formas posibles, es decir, visitaremos recursivamente cada estación directamente conectada con la estación actual, para eventualmente llegar al caso base.
{: .text-justify}

**Paso 3**:

{% highlight c++ %}

int tiempoMinimo(int estacion) {

  // Caso base
  if (estacion == numEstaciones)
    return 0;

  int mejorTiempo = INT_MAX;

  // Dividir el problema en subproblemas
  for (int i = 1; i <= numEstaciones; i++) {    
    if (tiempo[estacion - 1][i - 1] >= 0 && estacion != i) {
      // Combinar solución optimas
      int tiempoActual = tiempo[estacion - 1][i - 1] + tiempoMinimo(i);
      if (tiempoActual < mejorTiempo) {
        mejorTiempo = tiempoActual;
      }
    }
  }

  return mejorTiempo;
}

{% endhighlight %}

Finalmente aprovecharemos la propiedad de **problemas superpuestos** para almacenar en una tabla la solución a cada subproblema, de esta forma no se tendrá que calcular el mismo subproblema mas de una vez ahorrando tiempo de cómputo. Esto de logra al tener una tabla `memo` que es inicializada en un valor que nos indica que ese subproblema no ha sido calculado, en este caso nuestro indicador sera -1, una vez calculada la solución optima al subproblema se guardara este valor y cada que necesitemos la solucion optima utilizaremos el valor guardado en la tabla en lugar de calcularlo nuevamente.
{: .text-justify}

{% highlight c++ %}

vector<int> memo(numEstaciones, -1);

int tiempoMinimo(int estacion) {

  // Caso base
  if (estacion == numEstaciones)
    return 0;

  // Si se ha calculado previamente regresamos la solucion
  if (memo[estacion - 1] >= 0)
    return memo[estacion - 1];

  int mejorTiempo = INT_MAX;

  // Dividir el problema en subproblemas
  for (int i = 1; i <= numEstaciones; i++) {    
    if (tiempo[estacion - 1][i - 1] >= 0 && estacion != i) {
      // Combinar solución optimas
      int tiempoActual = tiempo[estacion - 1][i - 1] + tiempoMinimo(i);
      if (tiempoActual < mejorTiempo) {
        mejorTiempo = tiempoActual;
      }
    }
  }

  // Almacenamos la respuesta optima en la tabla
  return memo[estacion - 1] = mejorTiempo;
}

{% endhighlight %}

### ¿Como calcular la complejidad?

#### **Memoria**

La complejidad en memoria de un algoritmo de programación dinámica esta dado por el tamano de la tabla que utilizamos para almacenar las soluciones a los subproblemas, para obtener el numero de subproblemas distintos basta con multiplicar entre si las variables que representan al problema en la funcion recursiva. Dado que tenemos `N` estaciones del metro, la complejidad en memoria es `O(N)`
{: .text-justify}

#### **Tiempo**

La complejidad en tiempo se calcula **multiplicando el numero de subproblemas distintos por la complejidad de calcular un subproblema**. Para la solucion propuesta el numero de subproblemas distintos es `N` el numero de estaciones del metro. Para saber cuanto cuesta calcular cada subproblema tenemos que analizar la funcion recursiva, en el caso de nuestra funcion en cada llamada recursiva utilizamos un ciclo que recorre cada una de las `N` estaciones, por lo tanto la complejidad de calcular un subproblema es `O(N)`. Ahora multiplicando la complejidad de calcular un subproblema por el numero de subprolemas distintos tenemos que la complejidad de nuestro algoritmo es `O(N * N)`.
{: .text-justify}



### ¿Cual fue la mejor ruta?

Hasta ahora nuestro algoritmo solo es capaz de decirnos cual es el tiempo mínimo en que podemos llegar desde la estación 1 hasta la estación N, pero resulta que también podemos saber cual fue la ruta óptima de manera sencilla, esto solo requiere guardar información adicional en otra tabla, esta tabla nos dirá cual es la estación que debemos visitar después de tal forma que el resultado sea óptimo.
{: .text-justify}

{% highlight c++ %}

vector<int> memo(numEstaciones, -1);
vector<int> siguienteEstacion(numEstaciones, -1);

int tiempoMinimo(int estacion) {

  // Caso base
  if (estacion == numEstaciones)
    return 0;

  // Si se ha calculado previamente regresamos la solucion
  if (memo[estacion - 1] >= 0)
    return memo[estacion - 1];

  int mejorTiempo = INT_MAX;

  // Dividir el problema en subproblemas
  for (int i = 1; i <= numEstaciones; i++) {    
    if (tiempo[estacion - 1][i - 1] >= 0 && estacion != i) {
      // Combinar solución optimas
      int tiempoActual = tiempo[estacion - 1][i - 1] + tiempoMinimo(i);
      if (tiempoActual < mejorTiempo) {
        mejorTiempo = tiempoActual;
        // Guardamos la estacion a la que debemos ir para minimizar el tiempo
        siguienteEstacion[estacion - 1] = i - 1;
      }
    }
  }

  // Almacenamos la respuesta optima en la tabla
  return memo[estacion - 1] = mejorTiempo;
}

{% endhighlight %}

Al ejecutar nuestro algoritmo con el caso de ejemplo la tabla siguienteEstacion quedara de la siguiente forma:
{: .text-justify}

`Para la estación 1: siguienteEstacion[0] =  2`  
`Para la estación 3: siguienteEstacion[2] =  4`  
`Para la estación 5: siguienteEstacion[4] =  5`  
`Para la estación 6: siguienteEstacion[5] = -1`  

Bastara con iterar los valores de `siguienteEstacion` hasta llegar a -1 para saber cual fue la ruta óptima. De forma que la mejor ruta es `1-3-5-6`.
{: .text-justify}

{% highlight c++ %}
stack<int> mejorRuta(int estacion) {
  int estacionActual = estacion - 1;
  stack<int> mejorRuta;

  mejorRuta.push(estacion);
  while (siguienteEstacion[estacionActual] != -1) {
    mejorRuta.push(siguienteEstacion[estacionActual] + 1);
    estacionActual = siguienteEstacion[estacionActual];
  }

  return mejorRuta;
}
{% endhighlight %}

En general en cualquier algoritmo de programación dinámica cuyo objetivo sea la maximizacion o minimizacion podra utilizar este metodo para saber cual es la respuesta optima, solo se tendra que proporcionar la informacion necesaria para saber cual es la desicion que garantiza el resultado optimo. 
{: .text-justify}

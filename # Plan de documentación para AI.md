# Crear versión del plan de ejecución para AI

Sobre este plan que elaboraste, genera una versión concisa, orientada especialmente para que una AI lea e interprete los requerimientos para automatizar la construcción del sistema.

## Objetivo

Primero arma el plan de construcción de forma abstracta, sin componentes concretos. Te daré una serie de instrucciones para que me brindes opciones para elegir que implementación selecionar.
Luego de evaluar el plan, te daré instrucciones para rearmar el plan con implementaciones concretas.

## Consideraciones

Con las siguientes son para segmentar cada requerimiento técnico de forma abstracta antes de que pasemos a un plan de construcción concreto.

### Componentes

- Deben ser de fácil configuración y uso, que permita una rápida curva de aprendizaje en caso de tener que intervenir con alguna corrección manual.
- La aplicación principal que orqueste el procesamiento debe ser en python.
- La construcción de las integraciones de cada componente debe ser pensada de forma que pueda reemplazarse un componente sin verse afectada la solución.
- Para cada componente, haz una lista por categoría de componentes, dame opciones posibles para cada componente reemplazable. La lista debe contener componentes que cuenten con opciones o cuotas de uso gratuito para esta primera etapa de prototipo. Si se necesita escalar se puede considerar invertir en costos.

### Hospedaje

El hospedaje de cada componente dependera de las opciones que me brindes, si cuentan con una versión gratis en la nube debe ser esa la primera opción, pero si tienen costo y existe una posibilidad gratis con docker, será esa la alternativa.

### Configuración

Crear el esquema de un archivo json, para indicar los parámetros de las conexiones a cada componente que debe integrar el sistema.

### Interfaz de usuario

- No se necesita una interfaz visual para que interactúe un humano.
- La interfaz debe ser una línea de comandos.

### Parámetros de entrada

- Para la ejecución, dime si es posible (considerando factores de riesgo) que usemos un archivo de instrucciones como plantilla, para indicar los parámetros de entrada que se usarán en la ejecución.

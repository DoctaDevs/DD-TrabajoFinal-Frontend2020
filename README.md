# DD-TrabajoFinal-Frontend2020
Trabajo final para el curso de Frontend 2020-2021, dictado por @joelalejandro.

## ¡Vamos a hacer a un juego!

El objetivo de este trabajo final es evaluar todo lo que has aprendido durante el curso de Frontend: HTML, CSS y JavaScript.

## ¿Qué debo presentar?

- Un fork de este repositorio, con la aplicación desarrollada
- Toda la documentación que quieras para complementar tu proceso de trabajo en una carpeta `docs`. Pueden ser diagramas, anotaciones, lo que quieras.

## El proyecto

### Las pantallas

![image](https://user-images.githubusercontent.com/118913/118710506-c4683900-b7f4-11eb-8a03-940c0269fa08.png)

El juego consta de 8 pantallas, con algunas variaciones:

- Pantalla de carga
- Selección de pokémon
- Ficha técnica de pokémon
- Anuncio de la batalla
- Ronda de batalla
- Resultado de la ronda
- Resultado del juego
- Puntajes

### Las acciones del juego

Deberás construir una aplicación **web mobile** que haga las siguientes cosas:

- Cargar pokemones al azar
- Listar pokemons elegidos
- Mostrar información del pokemon al clickearlo
- Elegir el pokemon a utilizar para el combate
- Empezar una serie de rondas contra los otros 5 pokemones
- Por cada ronda:
  - Aplicar un algoritmo de combate (descrito más abajo)
  - Decidir quién es el ganador de cada ronda
  - Asignar los puntos al jugador o al oponente
  - Avanzar a la siguiente ronda
- Al finalizar la serie de rondas:
  - Determinar si se ganó o se perdió, comparando la cantidad de rondas ganadas contra la cantidad de rondas perdidas.
  - Guardar el resultado del combate en una API que ya estará lista para que utilices.

### Las herramientas de desarrollo

Para desarrollar este trabajo, deberás **forkear** este repo y crear tu aplicación, utilizando HTML, CSS y JavaScript.

Puntualmente, los requirimientos fundamentales de uso del lenguaje serán:

- HTML semántico
- CSS organizado en módulos, usando variables, clases y demás selectores.
- JavaScript organizado en base al paradigma de Web Components, utilizando además clases y funciones auxiliares.

### Arquitectura

Tendrás que decidir si la aplicación se compone de varios archivos .html o de un sólo archivo .html que muestra y oculta distintas secciones de la página.
Cada mecanismo tiene sus diferencias:

| Aspecto | Multipágina | Una página |
| ------- | ----------- | ---------- |
| Comunicación | Deberás utilizar LocalStorage para guardar información entre pantallas | Podrás utilizar variables de estado para almacenar la información |
| Organización | Cada página podrá tener cargados los componentes que necesita solamente | Deberás cargar todos los componentes que desarrolles |

Por otra parte, deberás decidir cómo almacenar la información que el juego necesita:

- Pokemones elegidos al azar
- Pokemon elegido para combatir
- Pokemones oponentes
- Puntuación de la ronda
- Puntuación del juego
- Rondas a jugar
- Rondas jugadas
- Rondas faltantes

## Funcionalidades

### Cargar pokemones al azar

Para cargar una serie de pokemones al azar, podemos utilizar el endpoint `/random` de la API de Pokémon. Podrás realizar una request de tipo GET a `https://app.pokemon-api.xyz/pokemon/random` para obtener un pokemon al azar. Como necesitamos varios, podemos llamar a la API varias veces para obtener los que necesitemos y guardarlos, para poder utilizarlos después.

### Listar pokemones elegidos

Una vez que tenemos guardados nuestros pokemons, podemos crear las tarjetas de selección con los siguientes datos:

- Tipos de pokemon
- Número
- Nombre
- Imagen

Salvo la imagen, esta información está disponible en la respuesta de la API que guardamos en el paso anterior.

La **imagen** vamos a sacarla de la PokeAPI, utilizando esta ruta como base:

https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-v/black-white/animated/POKENUMERO.gif

reemplazando `POKENUMERO` con el número del pokemon. Si es por ejemplo, 25, buscaremos la imagen aquí:

https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-v/black-white/animated/25.gif

La idea es formar una estructura como esta:

![image](https://user-images.githubusercontent.com/118913/118710261-6dfafa80-b7f4-11eb-95ef-b831683dc6f4.png)

Cada tipo de pokemon tiene su ícono y su color de fondo. Estas definiciones están disponibles en el diseño de Figma:

![image](https://user-images.githubusercontent.com/118913/118715348-e87a4900-b7f9-11eb-9b6c-42e28fe20e95.png)

En caso de que un pokemon tenga más de un tipo, solo utilizaremos los colores representativos del primer tipo de pokemon. 

Entonces, cuando vemos varios pokemones, tenemos algo como esto:

![image](https://user-images.githubusercontent.com/118913/118710406-9daa0280-b7f4-11eb-9f28-888c7837aa17.png)

### Ver información de un pokémon

Al elegir un pokemon, podemos ver toda la información de este pokemon:

![image](https://user-images.githubusercontent.com/118913/118710485-ba463a80-b7f4-11eb-8376-1ca6b89d2552.png)

En esta pantalla vemos:

- Pokemon #...: es el número del pokémon
- El nombre del pokémon
- Su nombre en otros idiomas (siempre son los mismos idiomas)
- Win ratio: Este dato va a venir de la API del juego, reportando la relación entre cuántas veces ganó y perdió este pokémon.
- Imagen: Obtenida de la PokeAPI, como se describió más arriba, pero más grande. Dado el estilo pixelado de las imágenes, **está bien agrandar la imagen** para que cubra toda la pantalla, no importa si se pixela más. En este caso, hace a la estética.
- Especie y descripción
- Tipos
- Estadísticas: observar que el color de las barras cambia según el tipo del pokémon también!
- Peso y altura
- Inefectividad, efectividad y no-efectividad: son los tipos de pokemones contra los que este pokemon es debil, fuerte y neutro, respectivamente. Esta información ayudará a definir los resultados de cada ronda de combate.

Toda esta información no debería requerir requests adicionales, ya que tenemos toda esta información al elegir los pokemones al azar al comienzo del juego. Aquí entra en juego tu habilidad para poder transferir la información de una pantalla a otra.

### Inicio de combate

![image](https://user-images.githubusercontent.com/118913/118711399-f037ee80-b7f5-11eb-880f-edef3188f649.png)

Esta pantalla aparecerá por cada ronda que juguemos. En "Won" y "Lost" se muestran cuantas rondas la persona jugadora ganó y perdió, respectivamente. 

### Progreso del combate

![image](https://user-images.githubusercontent.com/118913/118711493-0ba2f980-b7f6-11eb-8185-f0986adeabd6.png)

Mientras el combate se desarrolla, los pokemones perderán puntos de vida. Aquí entra en juego el ⚔ Algoritmo de combate ⚔.

Cada pokémon tiene 6 estadísticas. Vamos a darle uso de la siguiente manera (para quienes hayan jugado Pokémon, esto *no* es el algoritmo de los juegos, es una versión inventada):

- Puntos de vida (HP): cuántos golpes puede aguantar sin desmayarse
- Ataque (Attack): cuánto daño hace el pokémon al golpear
- Defensa (Defense): cuánto daño absorbe el pokémon al ser golpeado
- Ataque especial (Special Attack): cuánto daño extra hace el pokémon en un golpe de suerte
- Defensa especial (Special Defense): cuánto daño extra absorbe el pokémon en un golpe de suerte
- Velocidad (Speed): qué tan rápido es el pokémon para atacar

Dependiendo de cuánto te animes a desarrollar, podrás elegir entre 3 versiones de este algoritmo:

#### Versión básica

- Cada 1 segundo, cada pokemon realiza un golpe. El daño de ese golpe es igual a `PokemonAtacando.Ataque - PokemonDefendiendo.Defensa`. Si el daño es menor a 0, el daño del golpe se calcula con `PokemonDefendiendo.Defensa * 0.05` (el 5% de la defensa del oponente).
- La ronda se termina cuando `PokemonAtacando.PuntosDeVida <= 0` o `PokemonDefendiendo.PuntosDeVida <= 0`.
- Tras 2-3 segundos, se avanza a la pantalla de resultado.

#### Versión intermedia

Incluye lo de la versión básica y:

- Antes de realizar el golpe, se obtiene un número al azar entre 0 y 1. Si el número obtenido es mayor o igual a 0.9:
  - Si el pokemon que obtiene este número es el atacante, la fórmula pasa a ser: `PokemonAtacando.Ataque + PokemonAtacando.AtaqueEspecial - PokemonDefendiendo.Defensa`.
  - Si el pokemon que obtiene este número es el defensor, la fórmula pasa a ser: `PokemonAtacando.Ataque - PokemonDefendiendo.Defensa - PokemonDefendiendo.DefensaEspecial`. 
- Si el tipo del pokemon atacante es efectivo contra el tipo del pokemon defensor, el daño se incrementa en un 50% (`totalDaño * 1.5`).
- Si el tipo del pokemon defensor es efectivo contra el tipo del pokemon atacante, el daño se reduce en un 50% (`totalDaño * 0.5`).
- Si el tipo del pokemon atacante es inefectivo contra el tipo del pokemon defensor, el daño se reduce en un 50% (`totalDaño * 0.5`).
- Si el tipo del pokemon defensor es inefectivo contra el tipo del pokemon atacante, el daño se incrementa en un 50% (`totalDaño * 1.5`).

#### Versión avanzada

Incluye lo de la versión básica e intermedia y:

- Antes de realizar el golpe, se obtiene un número al azar entre 0 y la velocidad del pokemon. Si `VelocidadPokemon - NúmeroAzar <= 10`:
  - Si el número fue obtenido por el pokemon atacante, se anula la defensa del pokemon defensor, y recibe el 100% del daño.
  - Si el número fue obtenido por el pokemon defensor, se anula el ataque del pokemon atacante, y recibe el 100% del daño.

### Resultado de la ronda

![image](https://user-images.githubusercontent.com/118913/118716367-1e6bfd00-b7fb-11eb-9272-02621bc6a21d.png)

El pokemon que haya permanecido con `PuntosDeVida > 0` es quien recibe el punto para esta ronda.

Tras unos segundos, se incrementa en 1 el contador de rondas y se vuelve a la pantalla de comienzo de ronda.

### Fin del juego

![image](https://user-images.githubusercontent.com/118913/118716407-27f56500-b7fb-11eb-8bf9-4a3605f84574.png)

Al finalizar el juego, se cuentan las partidas ganadas y perdidas y se muestra el resultado.

Tras unos segundos, se avanza a la pantalla de resultados.

### Resultados

![image](https://user-images.githubusercontent.com/118913/118716440-35125400-b7fb-11eb-90b7-32146756faaa.png)

Se guardan los resultados en la API del juego y se muestran los resultados de las últimas 10 partidas con este pokemon.

Para guardar los resultados, se debe enviar una petición de tipo `POST` a `https://pokemon-last-standing-api.vercel.app/results`, con el siguiente JSON.

```js
{
  "pokemons": {
    // El número de pokémon del jugador
    "player": 121,
    
    // Lista con los pokémones oponentes
    "opponents": [21, 22, 23, 24, 25],
  },
  "rounds": {
    // Los pokemones contra los que ganó
    "win": [21, 22, 23],
    "loss": [24, 25]
  },
  // Si el resultado del juego es que el jugador ganó, enviar "win".
  // Si no, enviar "loss".
  "outcome": "win"
}
```

Para obtener los resultados de las últimas 10 partidas, se debe enviar una petición de tipo `GET` a `https://pokemon-last-standing-api.vercel.app/results` con el parámetro `limit=10` y el parámetro `pokemon` con el número de pokemon del jugador. La API te responderá de la siguiente manera:

```js
{
  "pokemon": 121,
  "results": [
    {
      "opponent": 21,
      "outcome": "win",
    },
    {
      "opponent": 22,
      "outcome": "win",
    },
    {
      "opponent": 23,
      "outcome": "win",
    },
    {
      "opponent": 24,
      "outcome": "loss",
    },
    {
      "opponent": 25,
      "outcome": "loss",
    },
    ...
  ]
}
```

### ¿Algo más?

Este proyecto tiene muchas cosas donde se puede profundizar y poner en práctica todo lo aprendido. También podrás sumar todo lo que investigues y aprendas por tu cuenta, siempre y cuando sigas los requerimientos básicos planteados en este documento.

## Diseños a utilizar

[Link a Figma](https://www.figma.com/file/xTwP1JAOZgXo6WOqQiZjtS/TP-Frontend-Pok%C3%A9mon-Last-Standing?node-id=0%3A1)

## APIs a utilizar

Vamos a estar utilizando la [API Pokémon hecha por Pulkit Sambhavi Singh](https://app.pokemon-api.xyz/), cuya documentación está disponible [aquí](https://purukitto.github.io/pokemon-api/).

Para las imágenes, utilizaremos la [PokeAPI](https://pokeapi.co/).

Guardaremos la información en una API con una base de datos preparada para este trabajo final, que estará disponible en https://pokemon-last-standing-api.vercel.app/.

## ¡No sé por dónde empezar!

Tranqui, puede ser un poco abrumador leer todo esto.

- Date una pasada por el Notion y andá repasando conceptos.
- Hacé una lista de las cosas que tiene que hacer la app e intentá asociarlas a las cosas que conocés.
- Por cualquier cosa, podés consultar a joel@doctadevs.com o en el foro de discusión de este repositorio.

## Recursos

- [Crear un enrutador SPA (Single Page Application) utilizando JavaScript](https://dev.to/alexcamachogz/creando-un-router-con-vanilla-javascript-27pl): Esto puede servir para trabajar todo en una sola pantalla sin necesidad de tener múltiples archivos HTML.
  



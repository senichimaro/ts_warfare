# Weapon Manual
R0-GR combat droid module deploy attack based in protocols to select targets and/or actions over targets.

See requiremenst in [MANUAL.md](MANUAL.md).

* R0-GR combat droid module is located in `./modules/r0gr_module.ts`

> .env file is intentionally uploaded to the repo with the purpose of exposing a complete folders and files structure.

> Errors are thrown for testing purposes.

> Typescript compiler options are targeting ES5 because there were no explicit constraints about it. This version was selected beacause It is the most extended browser supported version of ECMAScript (Javascript v.2009 - the first major revision to JavaScript). ES5 is fully supported in all modern browsers. ES6 (alse known as ECMAScript 2015) was fully supported by Safari 10 and Edge 14 in 2016, Chrome 58 and Firefox 54 in 2017, and Opera 55 in 2018.


### Index
* [Installation](#installation)
* [v1 programming interface](#v1-programming-interface)
  + [code structure](#code-structure)
  + [module functiolaty](#module-functiolaty)
* [v0 The Pseudo Code](#v0-the-pseudo-code)
  + [Assumptions](#assumptions)
  + [Weapon response](#weapon-response)
    - [protocols like flags](#protocols-like-flags)
    - [closest or furthest enemies](#closest-or-furthest-enemies)
  + [evaluation](#evaluation)
    - [evaluation interfaces](#evaluation-interfaces)
    - [warfare data](#warfare-data)
  + [response](#response)
* [curl command](#curl-command)
* [External refences](#external-refences)


## Installation
Instructions to install "R0-GR combat droid module":

* npm packages
~~~
// packages installation
> npm i
~~~
* dev mode
~~~
// run live over Typescript
> npm run dev
~~~
* build
~~~
// run tsc compiler
> npm run build
~~~
* test mode
~~~
// run compiled files
> npm run test
~~~


# v1 programming interface
R0-GR combat droid module has an intentionally strict protocol priority without loosing flexibility to execute orders following the "covering or destroy" principle, prioriting human lifes (allies) | target urgency (mech) | enemy lines (distance).

## code structure
Conceptually, R0-GR combat droid module, was externally created to be a "plug & play" module easy to replace or to combine: think in a _black box_ that other parts of the software has no idea how to work but a clear idea what it returns. Internally, It's a class splitted into single functions and It acts in an strictly order to be easy to extend, easy to modify, to replace or remove tasks. For more info see [v0 : The Pseudo Code](#v0-the-pseudo-code)

### module functiolaty
To follow _good practices_ functionalities followed two main principles: isolation and single purpose. Requirements were split into smallest parts as possible to bring a simple and clear progression of tasks. The main design priority was simplicity, clarity and scalability at long term.


# v0 The Pseudo Code
The core weapon action is to attack and protocols acts over **coordinates** and **enemies/type**.


~~~
protocols:
- enemies (distance)
- mech
- allies

item => {
  evaluate item for allies : positive / negative
  evaluate item for mech : positive / negative
  results [ item / items ]
  evaluate item for enemies : positive / negative
  return item;
}

// protocol avaluator
scanner => case => enemies => execAlliesScan()
scanner => case => mech => execMechScan()
scanner => case => allies => execEnemisScan()

extended : simple attack (safety lever for no enemy reference)
attack_allowed : trigger_safety()
~~~


## Assumptions
Actions modifiers relay over **enemies/type** and **(optional)allies**.

* enemies/type : soldier/mech(modifier)
* (optional)allies : modifier
* multiple valid targets : attack the first valid target


## Weapon response
Each protocol require specific attack configuration that is isolated to elaborate a single answer to a single petition.


#### protocols like flags
Work with flags are pretty much like filter values to include or exclude them. (select "flaged positive" items from scan property)


#### closest or furthest enemies
Coordinates from enemies location are given by y/x axis and distance is calculated getting hypotenuse. Then, enemis distance items are compared to reveal "closest / furthest" and the item is returned.

Get **scan** property, consider valid **coordinates** object (a^2 + b^2 = h^2) range those under 100m farest, select closest / furthest enemies.


## evaluation
R0-GR combat droid module receive [warfare data](#warfare-data) that algorithmic processes to interact with [evaluation interfaces](#evaluation-interfaces) that orchrestate a response.


#### evaluation interfaces
* basic : closest-enemies / furthest-enemies
* mech (flag) : prioritize-mech / avoid-mech
* allies (flag) : assist-allies / avoid-crossfire
* [extended] simple attack : allow attacks without enemies references


#### warfare data
-	**módulo de visión** (información de entorno)
    -	*protocols* : Protocolo o lista de protocolos que han de ser usados para determinar cual de los siguientes puntos debe de atacarse primero.
    -	*scan* : Lista de puntos extraidos del módulo de visión, que es un listado de puntos con el número de objetivos de esa posición, y los siguientes subvalores:
        -	*coordinates* : Coordenadas x e y del punto.
        -	*enemies* : Tipo de enemigo type y número number . Los posibles valores de type serán: soldier y mech.
        -	(opcional) *allies* : Número de aliados que hay en dicha posición. Si no está presente este valor, significa que no hay aliados en la zona.


- **Protocolos disponibles:**
    -	**closest-enemies** : Se deberá priorizar el punto más cercano en el que haya enemigos.
    -	**furthest-enemies** : Se deberá priorizar el punto más lejano en el que haya enemigos.
    -	**assist-allies** : Deberan de priorizarse los puntos en los que exista algún aliado.
    -	**avoid-crossfire** : No debe de atacarse ningún punto en el que haya algún aliado.
    -	**prioritize-mech** : Debe de atacarse un mech si se encuentra. En caso negativo, cualquier otro tipo deobjetivo será válido.
    -	**avoid-mech** : No debe de atacarse ningún enemigo del tipo mech


## response
An action response is based in enemy distance and coordinates. If no distance requirements are given it decide to attack the first valid target.


# curl command
This is the command used with curl to test API results.
~~~
curl --silent -H "Content-Type: application/json" -X POST -d "@test.json" http://localhost:8888/radar
~~~



# External refences
* [klicks](https://es.wikipedia.org/wiki/Klick)
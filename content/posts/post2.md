---
title: NFTs Programmable
date: 2022-05-05
description: How to create yout first Programmable NFT
---

##indice

1. Introduccion
2. Creando una NFT sin programar
3. Descargemos Metamask
4. Creando un archivo IPFS
5. Creando una NFT Programable
5.2 Desplegando Smart Contract en Solidity usando Remix
5.3 Minteando NFT

##Es necesario un poco de:
-familiarizacion con la red o blockchain donde se desea postear su NFT, tambien con el precio de la misma (BNB, ETH, POLYGON, SOL y mas)
-NO es necesario saber todo solo precios de la moneda (la moneda oficial de Binance) https://www.binance.com/es-MX/trade/BNB_BUSD
-El costo del gas por "minteo" de NFTs en esta red 
-El explorador de la red nos ayuda en todo esto. Rastrear transacciones e informacion de costos, datos para desplegar tu NFT en tu wallet y mas. Te dejo el explorador de Binance https://bscscan.com pero todas las blockchain tienen uno.

##Herramientas:
-Remix IDE (VM de Ethereum, es un IDE para crear contratos en Solidity)
-Metamask (cartera de crypto activos)
-Visual Studio Code (para generar archivos formatos .JSON)
-app.pinata.cloud (aplicacion que utiliza IPFS necesaria para almacenar nuestra NFT)

¡Empezemos!

##Paso 1:
Pasemos a explicar un poco sobre que es una NFT, las NFT mas conocidas actualmente son las NO programables usualmente creadas bajo herramientas gratuitas ofrecidas en NFT Marketplaces para ser listados en su  catalogo de venta, estas NFT usualmente son "Arte" y dentro de su codigo solo tiene un link IPFS con direccion al Arte que el creador puso en Venta.

La red de estas NFT NO programables estan guardadas usualmente bajo un SmartContract Maestro de el Marketplace donde se origino el Mint (Creacion del Token ERC-721), haciendo asi posible su facil transferencia entre compradores y vendedores.

Por otro lado, una NFT programable, es cuando nosotros somos los que creamos ese SmartContract maestro el cual puede tener cualquier caracteristica especial que nosotros queramos en nuestras NFT y sobre este smartContract maestro generara el MINT de las Token ERC-721 como el supply que pongamos a disposicion a nuestra comunidad , usuarios o clientes u otras funciones.

Por dar un ejemplo de que se podria lograr con la programacion del SmartContract seria:
1.- Utilizar la NFT como un pase especial de Acceso a contenido "Premium".
2.- Utilizar la NFT para que los holders de ella obtengan ingresos pasivos con el tiempo.
3.- Utilizar la NFT para Gaming dandole a cada NFT el id de un objeto en el Juego.

Las NFT tienen la caracteristica principal de ser un identificador unico e inmutable, pero no solo se limita a esto ya dependera de el uso que queramos darle.

##Paso 2
Aqui te dejo un tutorial paso a paso para crear una NFT NO Programable: https://github.com/Hisahito/my_launchx_blog/blob/master/content/posts/post3.md


##Paso 3: 
Descargemos Metamask. https://metamask.io  Se trata de una simple cartera de crypto activos. Aqui es donde veremos nuestra NFT minteada y aqui sera donde la portaremos todo el tiempo,
A Metamask reciberemos nuestra NFT una vez la mintiemos y tambien podremos enviarla a otra cartera si lo deseamos, siempre y cuando pagemos la tarifa de envio(gas fee).

Es importante aclarar que es necesario saber perfectamene en que red estamos deployando nuestra NFT pues como veras en al parte de arriba de Matamask aparece que estamos conectado a la red de ETH. Podemos cambiar de red siempre y cuando agregemos los datos que Metmask nos pide.

En esta prueba  utilizaremos la red de Binance, para ello sera necesario agregar su red a metamask te dejo un paso a paso aqui: https://academy.binance.com/es/articles/connecting-metamask-to-binance-smart-chain


##Paso 4: IPFS Pinnata
Pinnata se trata de una herramienta para almacenar archivos en la nube. Con esto pinnata nos genera un link valido para utilizar en nuestro contrato en Solidity.
<img width="1440" alt="Captura de Pantalla 2022-05-05 a la(s) 12 55 55" src="https://user-images.githubusercontent.com/83984969/169598620-62ffdde8-7fbb-4fae-8c53-f62436ca8a3c.png">



Pinnata es muy intuitivo y facil de usar. Primer subamos nuestra NFT en el formato que imagen que tenemos o hicimos:
<img width="1440" alt="Captura de Pantalla 2022-05-05 a la(s) 13 19 14" src="https://user-images.githubusercontent.com/83984969/169598734-9cc5de8d-be3f-4e1d-b472-94fb6cf72b40.png">


Una vez pinnata nos muestra que se subio nuestro archivo es importante que guardemos el URL que nos genero pues lo utilizaremos despues.
<img width="1440" alt="Captura de Pantalla 2022-05-05 a la(s) 13 17 08" src="https://user-images.githubusercontent.com/83984969/169598764-ac452857-fd2d-46ac-8ff9-3e18a647ad03.png">



##Paso 5
Listo tenemos nuestra imagen arriba en "formato" IPFS ahora VSC nos ayudara a generar el tokenURI necesario en nuestro contrato inteligente. Abramos nuestro VSC y creemos un archivo .json
creemos un objeto con ciertas caracteristas descriptivas de nuestra NFT:
<img width="1440" alt="Captura de Pantalla 2022-05-05 a la(s) 13 41 56" src="https://user-images.githubusercontent.com/83984969/169598880-aa4cf1d9-91e4-47ca-b2ae-1b65ba6835e1.png">



```javascript
{
  "name": "nombre de tu NFT",
  "description": "esta es mi primera NFT en la red de Binance",
  "valor": 50BNB,
  "image": "aqui va la URL que nos dio pinnata"
}
```

Es importante mencionar que en este archivo podemos poner los valores que queramos, esto solo son descriptivos y ayudaran a que nuestra NFT tenga una historia que contar.
Ahora guardemos este archivo y volvamos a Pinnata para subir nuestro JSON. Guardemos este nuevo URL. Este nuevo URL es el URL final que ira en nuestro contrato inteligente.

##Paso 5.1.- Remix ETH Virtual Machine.

Ahora si adentremenos a solidity !! Usaremos el IDE de Remix https://remix.ethereum.org

<img width="1440" alt="Captura de Pantalla 2022-05-05 a la(s) 12 47 01" src="https://user-images.githubusercontent.com/83984969/166983105-56f97b0c-eec4-4687-9c5b-e78aeb3fb8a0.png">


Creamos una carpeta con el nombre que gustes, en mi caso le puse el nombre de NFT y creamos un archivo nuevo .sol, en mi caso lo nombre firstNFT.sol
el codigo que ves en la imagen esta a continuacion:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.0;
 
import "https://github.com/0xcert/ethereum-erc721/src/contracts/tokens/nf-token-metadata.sol";
import "https://github.com/0xcert/ethereum-erc721/src/contracts/ownership/ownable.sol";
 
contract nft1 is NFTokenMetadata, Ownable {
 uint256 public tokenCounter;
  constructor() {
    nftName = "SandersCall";
    nftSymbol = "THECALL";
    tokenCounter = 0;
  }
 
  function mint(address _to, string calldata _uri) external onlyOwner {
    super._mint(_to, tokenCounter);
    super._setTokenUri(tokenCounter, _uri);
    tokenCounter = tokenCounter + 1;
  }
 
}
```
Expliquemos un poco el codigo:
Pragma solidity es la version de solidity que estaremos utilizando para nuestro SmartContract.
-Import se utiliza para agregar un SmartContract y utilizar las funciones de estos, se puede importar localmente o por medio de URL solo en remix.

```solidity
contract nft1 is NFTokenMetadata, Ownable
```
xnombrex: es el nombre que tendra nuestro contrato.
xcaracteristicas del contratox: es de que tipo sera nuestro contrato o si podra llamar funciones de otro smartcontract , en este caso puede llamar funciones de NFTokenMetadata y Ownable contratos que estan dentro de las librerias de Github que descargamos.

```solidity
uint256 public tokenCounter;
```
se trata de la declaracion de nuestra primera variable tokenCounter de tipo uint256.

```solidity
Constructor()
```
Es una funcion que todos los SmartContract tienen y sirve para iniciar las variables con un valor en cuanto se haga el deploy.

```solidity
function mint(address _to, string calldata _uri) external onlyOwner
```
Esta es la funcion que genera las token ERC-721 , solicita la direccion de la cartera a la que deseas mandar la NFT y la url del Json , esta funcion solo puede ser ejecutada por la cartera Owner del SmartContract.


##Paso 5.2.- Compilamos el Codigo para que Remix descarge el repositorio de github donde se encuentran las funciones del protocolo ERC-721 de la libreria de github:

![30b636eb-17d8-43a0-a5a4-314b6ed954aa](https://user-images.githubusercontent.com/83984969/169591970-54cace50-875f-4240-97e8-e32f5ac732c2.jpg)


Recuerden poner el compilador en la version 0.8.0 justo como el pragma solidity indica , sino esto nos podria dar error.


##Paso 5.3.- Deploy

En la seccion de Deploy , cambiamos a inject Web3 para que remix se conecte con Metamask y sepa que utilizaremos la red de pruebas de Binance que se diferencia con el numero 97.
![unnamed5](https://user-images.githubusercontent.com/83984969/169592167-d879c2cb-c698-4db1-92cc-a7e57e9bd7a9.jpeg)

Verificamos que estamos haciendo deploy de nuestro contrato y con nuestra cartera.
Una vez listos ejecutamos el deploy:

Confirmamos la transacion en Metamask ![8b38e16b-c949-46fc-86e0-caafe6d5ac5a](https://user-images.githubusercontent.com/83984969/169592548-c4aa0b02-66db-4f51-a932-48fe8ff27ba5.jpg)


Se verificara que se ejecuto el deploy en la testnet ![unnamed4](https://user-images.githubusercontent.com/83984969/169592634-e32231d1-ad82-46fc-ae12-f027d4d1d839.jpeg)


Revisamos en remix la direccion de nuestro SmartContract y desde remix podemos interactuar con las funciones de esta.         ![unnamed2](https://user-images.githubusercontent.com/83984969/169592696-bb2c327a-2494-4d29-a76d-62abe3426707.jpeg)


Por ultimo podemos hacer Mint de nuestra primera Token ERC-721 utilizando la funcion Mint.
Aqui agregamos la direccion de nuestra cartera y la URL del JSON que hicimos en el paso 4 que contiene nuestra imagen.
![unnamed3](https://user-images.githubusercontent.com/83984969/169592801-3ef35313-aad5-410b-824d-88b0495f0a7e.jpeg)



Verificamos si la transacion se realizo con exito confirmandolo en el explorador de la red de Binance, este nos dara todos los daros importantes 

![d43e130a-7d56-41c6-88fa-3e9f36174b2a](https://user-images.githubusercontent.com/83984969/169596878-aa4052eb-54ca-47e1-b21a-9e8b9df89b57.jpg)

Felicidades! Haz programado tu primera NFT!!! 

Recuerda que con las funciones creadas podemos hacer llamados a la blockchain para recibir datos de nuestro reciente SmartContract todo desde el IDE de Remix.

Por ejemplo podemos observar la cantidad de tokens que a generado  nuestro smartContract con tokenCounter.
Tambien podemos ver quien es dueño de la NFT numero 1 con OwnerOf.
Podemos ver el nombre y quien es el dueño de este SmartContract con Owner y Name , asi como tambien la URL de cada Token.
Tambien podemos observar cuantas de nuestra NFT tiene cierta cartera , ingresando su direccion en BalanceOf.

Espero este tutorial haya sido de ayuda! no dudes en compartirlo.







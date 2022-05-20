---
title: NFTs in the Binance Chain Tutorial
date: 2022-05-05
description: How to create yout first NFT
---

## ESTE POST SE ENCUENTRA EN CONSTRUCCION TODAVIA. ESTOY EN LA FASE DE PRUEBA PARA CONFIRMAR SU FIABILIDAD.

Quiero partir desde la idea que conocemos que son las NFTs y de lo que se trata, con esto explicado me dedicare unicamente a explicar el paso a paso de como
crear una NFT en la red de Binance Chain.
Quiero aclarar tambien que se trata de mismo proceso si desea "Mintear" una NFT en la red de Ethereum, ya que utilizarremos las mismas herramientas utilizadas 
en ambas redes.

##Herramientas:
-Remix IDE (VM de Ethereum, es un IDE para crear contratos en Solidity)
-Metamask (cartera de crypto activos)
-Visual Studio Code (para generar archivos formatos .JSON)
-app.pinata.cloud (aplicacion que utiliza IPFS necesaria para almacenar nuestra NFT)


Paso 1:
-Un poco de familiarizacion con el precio de BNB (la moneda oficial de Binance) https://www.binance.com/es-MX/trade/BNB_BUSD
-El costo del gas por "minteo" de NFTs en esta red 
-El explorador de la red de Binance para rastrear transacciones e informacion importante para rastrear tu NFT y desplegarla en tu wallet

Pasemos a explicar un poco sobre que es una NFT, las NFT mas conocidas actualmente son las no programables usualmente creadas bajo herramientas gratuitas ofrecidas en NFT Marketplaces para ser listados en su  catalogo de venta, estas NFT usualmente son "Arte" y dentro de su codigo solo tiene un link IPFS con direccion al Arte que el creador puso en Venta.

La red de estas NFT no programables estan guardadas usualmente bajo un SmartContract Maestro de el Marketplace donde se origino el Mint (Creacion del Token ERC-721), haciendo asi posible su facil transferencia entre compradores y vendedores.

Las NFT programables, es cuando nosotros somos los que creamos ese SmartContract maestro el cual puede tener cualquier caracteristica especial que nosotros queramos en nuestras NFT y sobre este smartContract maestro se generara el MINT de las Token ERC-721 que sera el supply que pongamos a disposicion a nuestra comunidad , usuarios o clientes.

Por dar un ejemplo de que se podria lograr con la programacion del SmartContract seria:
1.- Utilizar la NFT como un pase especial de Acceso a contenido "Premium".
2.- Utilizar la NFT para que los holders de ella obtengan ingresos pasivos con el tiempo.
3.- Utilizar la NFT para Gaming dandole a cada NFT el id de un objeto en el Juego.

Las NFT tienen la caracteristica principal de ser un identificador unico e inmutable, pero no solo se limita a esto ya dependera de el uso que queramos darle.

En este tutorial enseñaremos la plantilla sencilla que toda NFT  bajo el protocolo ERC-721 utiliza. 



Paso 3: Descargemos Metamask. https://metamask.io  Se trata de una simple cartera de crypto activos. Aqui es donde veremos nuestra NFT minteada y aqui sera donde la portaremos todo el tiempo,
desde Metamask podemos enviarla a alguien mas o inclusive recibir mas. Como primer paso la necesitamos para recibir nuestra NFT desde REMIX una vez la mintiemos. 

Es importante aclarar que es necesario saber perfectamene en que red 
estamos creando nuestra NFT, cada red principal (Binance,ETH,Polygon,Avalanche) tambien tiene una red de pruebas donde el costo del token es gratis y es una buena alternativa a utilizar para hacer pruebas de nuestros SmartContracts antes de pasarlos a produccion.

En esta prueba  utilizaremos la Testnet de Binance, para ello sera necesario agregar la testnet a metamask: LINK A TUTO
y conseguir nuestros primeros BNB de la testnet gratis: LINK A FAUCET


Paso 4: IPFS Pinnata
Pinnata se trata de una herramienta para almacenar archivos en la nube. Con esto pinnata nos genera un link valido para utilizar en nuestro contrato en Solidity.
<img width="1440" alt="Captura de Pantalla 2022-05-05 a la(s) 12 55 55" src="https://user-images.githubusercontent.com/83984969/166987583-ecc9b6e2-9b21-4ccc-8d76-751bc6799d29.png">


Pinnata es muy intuitivo y facil de usar. Primer subamos nuestra NFT en el formato que imagen que tenemos o hicimos:
<img width="1440" alt="Captura de Pantalla 2022-05-05 a la(s) 13 19 14" src="https://user-images.githubusercontent.com/83984969/166987908-37c93dac-5302-4ea3-9d50-3e45b8530306.png">

Una vez pinnata nos muestra que se subio nuestro archivo es importante que guardemos el URL que nos genero pues lo utilizaremos despues.
<img width="1440" alt="Captura de Pantalla 2022-05-05 a la(s) 13 17 08" src="https://user-images.githubusercontent.com/83984969/166988440-e6e59a5d-0469-4876-98df-95beae17bd90.png">


Paso 5 Visual Studio Code
VSC nos ayudara a generar el tokenURI necesario en nuestro contrato inteligente. Abramos nuestro IDE y creemos un archivo .json
creemos un objeto con ciertas caracteristas descriptivas de nuestra NFT:
<img width="1440" alt="Captura de Pantalla 2022-05-05 a la(s) 13 41 56" src="https://user-images.githubusercontent.com/83984969/167000056-f41d9ea1-83a5-4ae2-b802-fc8746eaba01.png">


```javascript
{
  "name": "nombre de tu NFT",
  "description": "esta es mi primera NFT en la red de Binance",
  "valor": 50BNB,
  "image": "aqui va la URL que nos dio pinnata"
}
```

Es importante mencionar que en este archivo podemos poner los valores que queramos, esto solo son descriptivos y ayudaran a que nuestra NFT tenga una historia que contar.
ahora guardemos este archivo y subamoslo a Pinnata. Guardemos este nuevo URL. Este nuevo URL es el URL final que ira en nuestro contrato inteligente.

Paso 2: Remix Virtual Machine.

Ahora si adentremenos a solidity !!

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
-Contract xnombrex is xcaracteristicas del contrato 1x,xcaracteristicas del contrato 2x { } 
xnombrex: es el nombre que tendra nuestro contrato.
xcaracteristicas del contratox: es de que tipo sera nuestro contrato o si podra llamar funciones de otro smartcontract , en este caso puede llamar funciones de NFTokenMetadata y Ownable contratos que estan dentro de las librerias de Github que descargamos.

uint256 public tokenCounter; : es la declaracion de nuestra primera variable tokenCounter de tipo uint256.

Constructor() : Es una funcion que todos los SmartContract tienen y sirve para iniciar las variables con un valor en cuanto se haga el deploy.

function mint(address _to, string calldata _uri) external onlyOwner : Esta es la funcion que genera las token ERC-721 , solicita la direccion de la cartera a la que deseas mandar la NFT y la url del Json , esta funcion solo puede ser ejecutada por la cartera Owner del SmartContract.

Paso 2.1.- Compilamos el Codigo para que Remix descarge el repositorio de github donde se encuentran las funciones del protocolo ERC-721 de la libreria de github:

-img compile-

Recuerden poner el compilador en la version 0.8.0 justo como el pragma solidity indica , sino esto nos podria dar error.


Paso 2.2 .- Deploy

En la seccion de Deploy , cambiamos a inject Web3 para que remix se conecte con Metamask y sepa que utilizaremos la red de pruebas de Binance que se diferencia con el numero 97.
-img red y cartera-
Verificamos que estamos haciendo deploy de nuestro contrato y con nuestra cartera.
Una vez listos ejecutamos el deploy:

Confirmamos la transacion. -img confirm-

Se verificara que se ejecuto el deploy en la testnet -img de tx confirmada-

Revisamos en remix la direccion de nuestro SmartContract y desde remix podemos interactuar con las funciones de esta. -img funciones-

Por ultimo podemos hacer Mint de nuestra primera Token ERC-721 utilizando la funcion Mint. -img MINT-

Aqui agregamos la direccion de nuestra cartera y la URL del JSON que contiene nuestra imagen de portada.




Podemos hacer llamados a la blockchain para recibir datos de nuestro reciente SmartContract.

Por ejemplo podemos observar la cantidad de tokens que a generado  nuestro smartContract con tokenCounter.
Tambien podemos ver quien es dueño de la NFT numero 1 con OwnerOf.
Podemos ver el nombre y quien es el dueño de este SmartContract con Owner y Name , asi como tambien la URL de cada Token.
Tambien podemos observar cuantas de nuestra NFT tiene cierta cartera , ingresando su direccion en BalanceOf.






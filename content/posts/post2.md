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
-Remix IDE (VM de Ethereum, es un IDE para crear contrartos en Solidity)
-Metamask (cartera de crypto activos)
-Visual Studio Code (para generar archivos formatos .JSON)
-app.pinata.cloud (aplicacion que utiliza IPFS necesaria para almacenar nuestra NFT)


Paso 1:
-Un poco de familiarizacion con el precio de BNB (la moneda oficial de Binance) https://www.binance.com/es-MX/trade/BNB_BUSD
-El costo del gas por "minteo" de NFTs en esta red 
-El explorador de la red de Binance para rastrear transacciones e informacion importante para rastrear tu NFT y desplegarla en tu wallet

Paso 2: Remix Virtual Machine de ETH.
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

Paso 3: Descargemos Metamask. https://metamask.io  Se trata de una simple cartera de crypto activos. Aqui es donde veremos nuestra NFT minteada y aqui sera donde la portaremos todo el tiempo,
desde Metamask podemos enviarla a alguien mas o inclusive recibir mas. Como primer paso la necesitamos para recibir nuestra NFT desde REMIX una vez la mintiemos. Es importante aclarar que es necesario saber perfectamene en que red 
estamos creando nuestra NFT, pues en Metamask tambien podemos cambiar de red. Continuare el tutorial suponiendo que la red donde crearemos nuestra NFT es la red de Binance.
Dicho esto, es necesario entonces agregar la red de Binance a nuestra wallet de metamask, para ello sigue este tutorial: https://academy.binance.com/es/articles/connecting-metamask-to-binance-smart-chain

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



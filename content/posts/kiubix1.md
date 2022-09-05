---
title: Análisis de los Beneficios sobre el uso de Strapi + React vs Strapi + GraphQL + React
date: 2022-05-05
description: Pros y Contras de implementar Strapi + GraphQL + React.
---

**indice**


 1.¿Qué es GraphQL y como implementarlo?    
 2.Implementación de Strapi con GraphQL y React.  
 3.Beneficios y desventajas de Strapi + React. 
 4.Beneficios y desventajas de Strapi + GraphQL + React. 

**Introduccion**. 


Intentaremos darle una explicacion a la diferencia en utilizar tecnologias para el desarrollo de backend agil como Strapi, un CMS que nos ayuda a facilitar la creacion de contenido junto al servidor backend y endpoints personalizados para su consumo, de la mano de React del lado cliente o, por otro lado, los beneficios y contras de agregar la capa de GraphQL a estas tecnologias y si es conveniente o no para el desarrollo de proyectos empresariales de alta escalabilidad como los desarrollados en KIUBIX. Vamos al punto.

**GraphQL**. 
**¿Qué es GraphQL?**. 


GraphQL es un lenguaje de consultas para API, brinda una descripción completa y comprensible de los datos del API, también brinda al cliente la capacidad de solicitar exactamente los datos que necesita.

¿Cómo implementar GraphQL?(ejemplo práctico)
  1 Iniciar un proyecto de NodeJS. 
  ```javascript
 npm init -y
```
<img width="598" alt="Captura de Pantalla 2022-09-05 a la(s) 11 06 03" src="https://user-images.githubusercontent.com/83984969/188489203-d66ffaa0-0d85-4e85-bd83-5882f3d8b616.png"> 


Apollo Server es un servidor de GraphQL, es el más popular y el que tiene mejor documentación. El cliente solicita informacion con las sintaxis de GraphQL a apollo server y apolo server se conecta al REST API, microservicios o una BD, 
consulta la información, la procesa y la devuelve al cliente.

![unnamed-2](https://user-images.githubusercontent.com/83984969/188489228-35e20e18-121f-47fd-9db3-87d4c549604d.png) 

  2 Instalar apollo y graph.    
  ```javascript
 npm i apollo-server graphql
```

  3 Usaremos ECMAScript. para eso abrimos el proyecto y archivo package.json y añadimos la opción “type”: “module”, el archivo debería quedar así
![unnamed-3](https://user-images.githubusercontent.com/83984969/188489572-f9c8ea2a-fc5a-4f60-8362-b6bad7d4ecbc.png) 

  4 Crear el archivo index.js , importar graphql con Apollo-server con :
  ```javascript
import {ApoloServer, gql} from ‘apollo-server’
  ```
Para mayor practicidad, se usará un array de objetos a modo de base de datos

![unnamed-4](https://user-images.githubusercontent.com/83984969/188489716-2c04c57d-9869-4484-9fc6-3191d0471b53.png) 

  5 Es importante definir la estructura de los datos y peticiones
![unnamed-5](https://user-images.githubusercontent.com/83984969/188489805-5653a476-ab1f-4db9-af32-b8d721498971.png) 

  6 Definir las querys. 
  
  
  ![unnamed-6](https://user-images.githubusercontent.com/83984969/188489898-029a80ec-b1fc-49dc-a888-fc67eb8b0635.png) 

  7 crear y ejecutar el servidor. El constructor acepta un objeto con las definiciones de estructuras y las querys. Es muy parecido a express.  
  
  
  ![unnamed-7](https://user-images.githubusercontent.com/83984969/188490014-9f8f0833-98b3-4132-9a80-7825be79c008.png) 
  al ejecutar el proyecto, este nos arroja una base url y un puerto. 
  
  
  ```javascript
  $node index.js
  Servidor funcionando en https://localhost:4000/
  ```
  
  Al abrir esa url en el navegador veremos algo como esto:  
  
  
  ![unnamed-8](https://user-images.githubusercontent.com/83984969/188490231-35da2ce2-4358-45f2-9b75-503e38335fce.png) 
  
  
  si se presiona el botón “Query Your serve” veras una pagina en blanco conectada al servidor apollo donde puedes ejecutar tus querys

  
 ![unnamed-9](https://user-images.githubusercontent.com/83984969/188490888-a37f8ead-413a-4a69-9e74-5c4f3b676e2a.png)

  los llamados en Postman son practicamente iguales:
  
  ![unnamed-10](https://user-images.githubusercontent.com/83984969/188490981-f4ecd564-7770-4be9-ac01-1361b0e75ef9.png)

  Aunque es aconcejable usar el sistema propio de apollo-server debido a que genera un tipo de documentación automática respecto a los datos que devuelven los querys.
 
  Y eso es todo, así de simple puedes crear un servidor de GraphQL con sus querys. no obstante hay muchas otras cosas interesantes que se pueden hacer, veamos:
  
**Peticiones con parámetros**.  

Se debe hacer otro query y entre paréntesis (como si fuera una función con parámetros) se especifica el nombre del parámetro, el tipo de dato y lo que devuelve.
En este ejemplo el parámetro hace una búsqueda por nombre.
en args están todos los argumentos especificados en el query. En este ejemplo se desestructura el “nombre” en una constante aunque se puede hacer desde la definición del argumento.

![unnamed-11](https://user-images.githubusercontent.com/83984969/188491144-7187bb54-61ac-4faa-b204-9d2bfd090a4b.png)

Observa bien que en la llamada se especifican los parámetros con sus valores de esta manera y se especifica los campos que se requiere, en caso de no existir algún campo devolverá “null”

![unnamed-12](https://user-images.githubusercontent.com/83984969/188491220-d6cee745-34de-4942-aafb-7500ece8e1b5.png)
![unnamed-13](https://user-images.githubusercontent.com/83984969/188491288-44c29b3d-2c58-4a5f-8264-6cefe9e62f06.png)
![unnamed-14](https://user-images.githubusercontent.com/83984969/188491303-8ccea2c3-f1f3-4ea7-b47c-3070b7563f15.png)

**Resolver campos de manera dinámica**. 

Se pueden resolver campos con información de otros campos o con otra lógica resultando en un campo extra al de la información original. IMPORTANTE, el campo resultante se debe especificar en el modelo.
para este ejemplo añadiremos dos campos extra al modelo “nombreCompleto” y “mayorEdad”. 
Para resolver esos campos, se debe hacer una propiedad extra a lado de los Query con el mismo nombre del modelo y dentro el nombre de los campos con su solución.
```javascript
  const typeDefs = gql {
    type Person {
      id: ID!,
      nombre: String!,
      apellidos: String!,
      nombreCompleto: String!,
      edad: Int,
      mayorEdad: Boolean,
      telefono: String!,
    }
  }
  ```
  
  ```javascript
  const resolvers = {
    Query: {
      personCount: () => person.length,
      allpersons: () => persons,
      //args son los argumentos que se especifican en la query
      findPerson: (root, args) => {
        const {nombre} = args;
        return persons.find(person => person.nombre === nombre);
      }
    },
      Person: {
        nombreCompleto: (root) => ´${root.nombre} ${root.apellido} ´,
        mayorEdad: (root) => root.edad > 17
      }
  }
  ```
  
  ![unnamed-15](https://user-images.githubusercontent.com/83984969/188492335-2f7d3253-6f97-4ec1-be6c-c47543a81955.png)

**Tipos de datos personalizados**. 

Un tipo de dato personalizado en un modelo en realidad es un objeto. para usarlo, se debe declarar un modelo y usarlo como propiedad de otro modelo y como una propiedad común, se puede resolver y al hacer el llamado en la query, 
también se pueden especificar los datos que se requieren:

```javascript
  const typeDefs = gql {
    type nombreCompleto {
      nombre: String!,
      apellidos: String!
    },
    type Person {
      id: ID!,
      nombre: String!,
      apellidos: String!,
      nombreCompleto: nombreCompleto!,
      edad: Int,
      mayorEdad: Boolean,
      telefono: String!,
    }
  }
  ```
   ```javascript
      Person: {
        nombreCompleto: (root) => {
          return{
            nombre: root.nombre,
            apellido: root.apellido
          }
        },
        mayorEdad: (root) => root.edad > 17
  }
  ```
  
  ![unnamed-16](https://user-images.githubusercontent.com/83984969/188492951-2002aaaf-25b8-46f6-9f42-1c5a018f10a1.png)
  
  Como podemos observar, los datos que manejamos con GraphQL tiene una estructura completamente diferente a la bd, devolviendo campos que ni siquiera están en la bd original. De este modo podemos cambiar la manera en 
  la que se envían y reciben los datos en el cliente sin necesidad de cambiar el modelo o entidad de la BD, los datos simplemente se procesan y se adaptan.
  
**Mutaciones**. 

Las mutaciones son maneras para modificar información, en este caso las usaremos para añadir más personas a la bd. recordar que aunque estamos usando un array como bd, también podemos usar una de mongo o sql u otras API
Para hacer una mutación, primero y como todo en GraphQL, se debe definir en el modelo y el los resolvers
Para completar el ejemplo correctamente instalaremos, importamos y usaremos uuid con: 
```javascript
   npm i uuid
  ```

```javascript
      type Query {
        personcount: Int!,
        allPersons: [Person!],
        findPerson: (nombre: String!) Person
  }
  
    type Mutation {
      addPerson(
        nombre: String!,
        apellidos: String!,
        edad: Int,
        telefono: String
      ):Person
    }
  ```
  
  ```javascript
   Mutation: {
    addPerson: (root, args) => {
      const person = { ...args, id: uuid() }
      persons.push(person)
      return person;
    }
   },
    person: {
  ```
  
  Al momento de hacer la mutación, también se debe especificar los datos deseados de retorno

![unnamed-17](https://user-images.githubusercontent.com/83984969/188494354-f1f239ec-23d3-46c7-8d47-70d62916316f.png)


Como seguramente ya notaste, lo interesante está en los resolvers, desde esa seccion de codigo indicas cómo transformar la información
  
**Implementación de Strapi con GraphQL y React**. 

Primero que nada, debemos entender que el hecho de usar GraphQL no representa un cambio tal cual al momento de realizar la instalación de Strapi cuando vamos a crear el proyecto nuevo, ¿la razón? Usar GraphQL 
en Strapi es realizando la instalación de un plugin, que se hace de la siguiente manera:
```javascript
   strapi install graphql
  ```
  
  A continuación, debemos instalar dos paquetes muy importantes para el uso de GraphQL en nuestro proyecto:
  ```javascript
   npm install @apollo/client graphql
  ```
  
  Ya una vez con esto en nuestro proyecto de React, dentro del componente que queremos utilizar o queramos utilizar GraphQL. Vamos a importar useQuery y gql del paquete “@apollo/client”.
  ```javascript
   import { useQuery, gql } from @apollo/client
  ```
  
  La manera en que podemos pedir datos al backend utilizando GraphQL, es llamando a ”gql” y usando las backtick para generar un formato legible y poder realizar nuestra consulta, y debería verse algo así:
  ```javascript
   const BLOGS = gql ´
    {
      blogs{
        data{
          id,
          attributes{
            Title,
            Body,
            Author
          } 
        }
      }
    }
    ´
  ```
  
  Ahora ya tenemos nuestra query creada, y con esto podemos hacer una consulta para poder obtener la información que necesitamos. Vamos a usar ahora el useQuery, que importamos igual de “@apollo/client”, 
  para hacer la consulta y de esta misma, podemos aplicar “destructuring” para obtener 3 valores principales, el de loading, el de error, y el de data. Con estos podemos realizar una lógica en donde si la información 
  está cargando podemos devolver un componente como una pantalla de carga o bien, un mensaje solamente, ya que nos permite como el nombre lo indica, saber si está siendo cargada la información. El siguiente es error, 
  que es evidente el objetivo que tiene, ya que nos permite identificar qué salió mal y poder ya sea devolver un componente donde le digamos al usuario que ha ocurrido algo, o la información no fue encontrada, 
  o solamente devolverle a un componente diferente. Por último, pero igual de importante, tenemos el de data, en donde aquí vendrá la información que trajo nuestra consulta. A continuación, un ejemplo de esto mencionado:

  ```javascript
   const { loading, error, data } = useQuery(BLOGS);
   if (loading) return <p> loading...</p>
   if (error) return <p> "Error :(" </p>
  ```
  
  De data puedes acceder a la información de lo que solicitaste, por ejemplo, en los códigos mostrados, al haber pedido blogs, entonces en nuestra data, hay una propiedad llamada blogs en donde tiene a su vez su propia data, 
  que son los registros de nuestra información anteriormente solicitada. Se puede hacer la lógica que se requiera, como recorrer la data y que cada elemento contenido se muestre de manera determinada con HTML y CSS. Ejemplo:

  ![unnamed-18](https://user-images.githubusercontent.com/83984969/188498947-ed7e3b4b-8eb2-402a-8d3e-f41213331bc4.png)


  Como desarrolladores web, podremos querer recibir variables para poder realizar consultas a gusto y necesidad del usuario, haciendo uso de un id, nombre, email, entre otros atributos que podríamos usar como parámetros para realizar una consulta. 
  Afortunadamente, es sencillo poder hacer esto, ya que podemos decirle a gql, que queremos hacer uso de variables de la siguiente forma:

  ```javascript
   const BLOGS = gql ´
    query GetBlog( id: ID!){
      blog(id: $id){
        data{
          id,
          attributes{
            Title,
            Body,
            Author
          } 
        }
      }
    }
    ´
  ```
  
  Como se puede apreciar, estamos haciendo uso de $id, pero, la pregunta es cómo hacer para decirle a gql qué valor debe tomar ese $id. Lo hacemos pasándole como segundo parámetro a nuestra función de useQuery, un objeto que tiene una propiedad llamada 
  variables que igualmente a su vez es un objeto donde vamos a establecer las variables que va a hacer uso gql para realizar nuestra consulta. Ejemplo:

  ```javascript
   const { loading, error, data } = useQuery(BLOGS, {
      variable: { id: id }
  })
  ```
  
  Ahora, debemos inicializar primero nuestro Apollo Client en el App.js, y para ello vamos a crear un objeto, podemos llamarlo de la manera que se desee, pero, por cuestiones de ejemplificar de la mejor forma lo denominaremos “client”. 
  Pasaremos como parámetro un objeto que tendrá 2 propiedades, la uri y el cache que es dónde va a ser guardado el mismo. Importaremos 3 cosas para este paso, ApolloClient, InMemoryCache y ApolloProvider. Apollo Client es para poder inicializar nuestro cliente,
  InMemoryCache es para poder establecer que el caché va a ser guardado en memoria y el ApolloProvider tiene un funcionamiento similar al context que maneja React, y nos permite se establecer el cliente en el context para poder acceder desde cualquier componente.

  ![unnamed-19](https://user-images.githubusercontent.com/83984969/188499474-344fb430-d8ac-4bc0-9d15-6edc6b4e1891.png)

 **Beneficios y Desventajas de Implementar Strapi + React en conjunto**. 
 
 | Pros | Contras |
| ------------- | ------------- |
| -Javascript como lenguaje común por ende la sintaxis es lineal en todo el proyecto  | -Es necesario descargar “React Markdown Package” para editar el texto enriquecido.  |
| -De entrada usar la API de Strapi nos agrega un mundo de posibilidades de personalización en nuestras requests.  | -Strapi no deja de ser un “enlatado” es probable que si el proyecto escala mucho nos encontremos con limitaciones que sería necesario probar.  |
| -El panel administrativo de Strapi es super intuitivo de manejar, inclusive para usuarios no-técnicos, agregando una capa de “rapidez” al momento de querer realizar algo rápido.  | -Al momento de realizar cambios de permisos y estar conectado a varios ambientes, estos cambios sólo se reflejan en un solo ambiente. Es necesario entonces hacer los cambios manualmente para todos los ambientes.  |
| -A comparación de una API REST normal que tomaría más tiempo en desarrollar, Strapi nos facilita una implementación más rapida.  | -En el caso particular de Strapi, al estar basado 100% en Javascript puede ser complicado para otros usuarios su compatibilidad con otros lenguajes por ejemplo para Typescript es necesario usar un paquete externo, no hablemos de Python u otros.  |
| -Lo anterior dicho se podría resumir en 3 grandes beneficios: menos código, más agilidad en el desarrollo y gran potencial de escalabilidad en nuestro proyecto/idea.  | -Posibles alternativas: Contentful, Netlify CMS, FireBase, Directus o Parse-Server.  |



**Beneficios y Desventajas de Implementar Strapi, GraphQL & React en conjunto**. 

  | Pros | Contras |
| ------------- | ------------- |
| -Utilizar GraphQL + React le agrega una capa extra de seguridad pues se utiliza un solo endpoint POST para peticiones encriptadas.  | -Usuarios acostumbrados a trabajar en archivos .tsx tendrían que descargar paquetes externos para aplicarlo con Strapi.(BETA). |
| -La facilidad que agrega el usar Axios o Fetch en React para que el Front realice sus request una sola URL/endpoint.  | -Es virtualmente imposible tomar API que ya existen y reescribirlas con GraphQL. Se tendría que diseñar una nueva versión desde cero.  |
| -Tanto GraphQL como React son desarrollados y mantenidos por Facebook, mucha de su documentación e implementación pueden ayudar a la misma causa al ser los dos de tipo declarativos.  | -Es posible encontrarse con problemas si se requiere describir otro tipo de estructuras que no sean estructuras de objetos.  |
| -Si ya Strapi amplía nuestras posibilidades, la adición de GraphQL amplía aún más las chances de realizar querys específicas para solventar lo que exactamente necesita el front.  | -GraphQL puede presentar problemas cuando el cliente solicita demasiados datos de campos anidados a la vez. No existe un mecanismo de profundidades máxima de consulta.  |
| -A pesar de ser tecnologías nuevas, Strapi está muy bien complementado con GraphQL pues cuentan con un playground interno (localhost:1337/graphQL) para ingresar nuestros comandos.  | -Tanto Strapi y GraphQL son tecnologías relativamente nuevas por ello tienen constantes actualizaciones en donde su sintaxis suele cambiar.  |
| -Al existir un solo endpoint es muy facil de mantener y dar mantenimiento del lado del cliente.  | -GraphQL utiliza una sintaxis diferente que rompe el esquema de Strapi + React de puro JS. Es necesario aprenderla, tanto el tipado como el uso de variables. Esto puede tomar tiempo dependiendo de cada desarrollador.  |





###### **Escrito y analizado por Alfonso Mendiola, Carlos de la Cruz y Victor Morales para Kiubix.**

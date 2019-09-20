# DOM

## Intro

- Document Object Model
- Es un _modelo_ que representa la estructura y los elementos de un sitio web (documento) a través de un _árbol_
  - _nodos_ relacionados entre sí mediante _ejes/aristas/rama_
  - _raíz_: `document`
- Cuando cargamos un sitio web en el _browser_, este _parsea_ el contenido del documento HTML, construye un modelo (DOM) a partir de su estructura y contenido y finalmente _renderiza_ el resultado
- Podemos modificarlo _programáticamente_
  - Seleccionar un elemento por _clase_ o _id_
  - Recorrer el _árbol de nodos_
- **Cada _nodo_ del _árbol_ es un _objeto_** que representa un elemento del documento HTML
  - Podemos decir entonces que es una representación de los elementos del documento HTML a través de objetos
- En nuestro caso, siempre que hablemos de _documento_ ó `document`, vamos a estar haciendo referencia al HTML con el que estemos trabajando
- Es una _estructura viva_: cuando es modificada, la página que vemos en el browser se actuaiza para reflejar los cambios

![](https://www.kirupa.com/html5/images/DOM_js_72.png)

## DOM y JavaScript

- Cuando una página web se carga, el browser crea el DOM del sitio, que es un _objeto_ que representa el documento HTML y sirve como _interfaz_ entre JavaScript y el documento en si mismo
- El _browser_ _parsea_ el documento para determinar qué tiene que _renderizar_ y luego lo _renderiza_
- Recordemos que el _DOM_ es un _árbol de nodos_ que representan elementos del documento HTML y que estos _nodos_ son a su vez _objetos_, por lo tanto podemos interactuar con estos objetos usando JavaScript usando todo lo que ya conocemos (propiedades, métodos, etc)

![](https://bitsofco.de/content/images/2018/11/HTML-to-Render-Tree-to-Final.png)

- Para más detalles, ver
  - [Browser environment, specs](https://javascript.info/browser-environment)
  - [What, exactly, is the DOM?](https://bitsofco.de/what-exactly-is-the-dom/)
  - [DOM tree](https://javascript.info/dom-nodes)

## Nodos

- Todo lo que es accesible y modificable a través del DOM es un _nodo_

  - elementos HTML
  - atributos de un elemento HTML
  - contenido/texto dentro de un elemento

- Podemos utilizar la propiedad [`.childNodes`](https://developer.mozilla.org/en-US/docs/Web/API/Node/childNodes) para obtener una _lista_ de los nodos _descendientes_ de un nodo, indexados

```js
const ol = document.childNodes[1].childNodes[2].childNodes[1];
ol.parentElement;
ol.nextSibling.nextSibling;
ol.previousSibling;
```

### Tipos de nodos

- _Document_: el nodo _raíz_, `document`
- _Element_: un elemento HTML
- _Attr_: atributo de un elemento HTML
- _Text_: contenido de texto de un nodo de tipo `Element` ó `Attr`. **No tienen descendientes.**
- _Comment_: comentario HTML
- _DocumentType_: declaración del _Doctype_
- etc...

#### `nodeType`

- Con la propiedad [`nodeType`](https://developer.mozilla.org/en-US/docs/Web/API/Node/nodeType) podemos saber de qué tipo es un nodo
- `nodeType` retorna un entero que representa el tipo
- Podemos usar [`nodeName`](https://developer.mozilla.org/en-US/docs/Web/API/Node/nodeName) para obtener el nombre

## DOM API

- El _browser_ nos provee de una _API_ para interactuar con el DOM usando JavaScript
  - Esta _API_ nos permite
    - inspeccionar los elementos y la estructura del documento
    - modificar la _estructura_: agregar, modificar o eliminar elementos HTML ó atributos
    - modificar el _contenido_ del documento
    - modificar los _estilos_ (CSS)
    - agregar o eliminar eventos
    - reaccionar a determinados eventos
    - etc...

### `window`

- De los objetos que nos provee esta _API_, con los que más vamos a interactuar, son `document` y `window`
  - `document`: representa el documento; en nuestro caso el HTML que estamos viendo en el browser
  - `window`: representa la ventana que contiene al documento que estamos viendo en el browser
- :star: Las propiedades y métodos de `window` pueden ser utilizadas sin referenciar a `window` explícitamente, porque `window` (si estamos en el browser) representa al **objeto global**. Por lo tanto `window.document` puede escribirse como `document` directamente

- [Window - MDN](https://developer.mozilla.org/en-US/docs/Web/API/Window)

#### Algunas propiedades de `window`

- `document`: es una referencia al objeto `document` que estamos viendo en el browser
- `console`: es una referencia al objeto que representa la _consola de debugging del browser_
  - Es el mismo objeto que usamos para hacer el famoso `console.log`!

#### Algunos métodos de `window`

- `alert`: abre un _popup_ de alerta en el browser

### `document`

![](https://flaviocopes.com/dom/dom-body-head.png)  
_Parte del DOM que muestra los nodos `html`, `head` y `body`_

![](https://flaviocopes.com/dom/dom-head-title.png)  
_Parte del DOM que muestra los nodos hijos de `head`_

![](https://flaviocopes.com/dom/dom-body-a.png)
_Parte del DOM que muestra los nodos hijos de `body`_

#### Algunas propiedades de `document`

- `title`
- `URL`
- `documentElement`
- `head`
- `body`
- etc...

![](https://flaviocopes.com/dom/dom-nodes.png)

##### Ejemplo usando `document.body`

```js
document.body.style.background = 'red'; // make the background red

setTimeout(() => (document.body.style.background = ''), 3000); // return back
```

También podemos obtener una colección ([HTMLCollection](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCollection)) de nodos de un tipo particular, por ejemplo utilizando las siguientes propiedades de `document`

- `links`
- `images`
- `forms`
- etc...

- [Document - MDN](https://developer.mozilla.org/en-US/docs/Web/API/Document)

#### Algunos métodos de `document`

Los más utilizados forman parte de la [_Selectors API_](https://developer.mozilla.org/en-US/docs/Web/API/Document_object_model/Locating_DOM_elements_using_selectors): esta API nos provee de métodos que nos permiten obtener de forma fácil aquellos nodos del _DOM_ de tipo _Element_, que matcheen con un determinado _selector_ (de forma análoga a los selectores de CSS)

- `getElementById`
- `querySelector`
- `querySelectorAll`
- `getElementsByTagName`
- `getElementsByClassName`

## Recorriendo el DOM

El _DOM_ es un _arbol_ de elementos, con el nodo `document` en la raíz, que hace referencia al elemento `html` (tag), que a su vez contiene a sus elementos hijos `head` y `body`, etc.

Para cada uno de estos elementos, podemos navegar a través de la estructura del _DOM_ y movernos entre sus diferentes nodos.

### Obteniendo el nodo _parent_

Cada _nodo_ del _DOM_ tiene 1 _parent_. Para obtenerlo podemos usar

- `Node.parentNode`
- `Node.parentElement` (si el _parent_ es un _nodo_ de tipo _Element_)

Generalmente vamos a utilizar `parentNode`

### Obteniendo los _nodos children_

#### todos los descendientes

Para chequear si un _nodo_ tiene _descendientes_, podemos usar

- `<NODE>.hasChildNodes()`: retorna un valor _booleano_

Para obtener una lista de los _nodos descendientes_, de tipo _Element_ de un _nodo_, usamos

- `<NODE>.children`

El _DOM_ también nos provee de la propiedad `<NODE>.childNodes`. Esta incluye en el resultado, además de los nodos de tipo _Element_, los nodos de tipo _Text_ y espacios en blanco, por lo que generalmente vamos a preferir usar `children`

![](https://flaviocopes.com/dom/dom-get-children.png)

#### Primer y último descendiente

Para obtener el primer _child node_ de un nodo determinado, de tipo _Element_, usamos

- `<NODE>.firstElementChild`

Para obtener el último _child node_ de un nodo determinado, de tipo _Element_, usamos

- `<NODE>.lastElementChild`

![](https://flaviocopes.com/dom/dom-get-first-last-child.png)

### Obteniendo los _nodos hermanos_ (siblings)

- `<NODE>.previousElementSibling`
- `<NODE>.nextElementSibling`

### Editando el _DOM_

La _API_ del _DOM_ nos provee de varios métodos para editar los nodos y modificar el documento.

- `document.createElement()`: crea un nuevo nodo de tipo _Element_
- `document.createTextNode()`: crea un nuevo nodo de tipo _Text_

Podemos crear nuevos nodos y luego agregarlos a elementos del _DOM_ como _children_, utilizando

- `document.appendChild()`

```js
const div = document.createElement('div');
div.appendChild(document.createTextNode('Hello world!'));
```

#### Otros métodos para modificar el _DOM_

- [`first.removeChild(second)`](https://developer.mozilla.org/en-US/docs/Web/API/Node/removeChild): elimina el nodo _children_ `second` del nodo `first`
- [`document.insertBefore(newNode, existingNode)`](https://developer.mozilla.org/en-US/docs/Web/API/Node/insertBefore): inserta `newNode` como hermano de `existingNode`, ubicándolo antes que `existingNode` en el _DOM_
- [`element.appendChild(newChild)`](https://developer.mozilla.org/en-US/docs/Web/API/Node/appendChild): modifica el _árbol_ debajo de `element`, agregándole `newChild`, después de los demás nodos _children_
- [`element.prepend(newChild)`](https://developer.mozilla.org/en-US/docs/Web/API/ParentNode/prepend): modifica el _árbol_ debajo de `element`, agregándole el nodo `newChild`, antes que los demás nodos _children_
- [`element.replaceChild(newChild, existingChild)`](https://developer.mozilla.org/en-US/docs/Web/API/Node/replaceChild): modifica el _árbol_ debajo de `element`, reemplazando `existingChild` por el nodo `newChild`
- [`element.textContent = 'some text'`](https://developer.mozilla.org/en-US/docs/Web/API/Node/textContent): modifica el contenido de un nodo de tipo _Text_ a “some text”
- etc...

## Eventos

- Los nodos pueden tener _event handlers_ ligados a ellos. Cuando un evento se dispara, los _event handlers_ se ejecutan
- Podemos _reaccionar_ a _eventos_ generados, por ejemplo, por los usuarios, usando JavaScript y la funcionalidad que nos provee el _DOM_

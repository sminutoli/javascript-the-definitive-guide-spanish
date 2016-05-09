Si estuviste dando vueltas por el mundillo de Javascript lo suficiente, te habrás encontrado con un conjunto de recetas para trabajar orientado a objetos. La manera estándar, si es que puede haber una manera estándar, es esta.

Y esta es la receta para la herencia.

Este es el estándar. Pero ¿por qué esto? ¿Por qué este... lío? ¿Y por qué esto funciona?

Hola a todos. Mi nombre es James Shore y esto es Let’s Code: Test-Driven JavaScript. Vengo a ofrecerles algunas lecciones que aprendí sobre programación orientada a objetos en javascript. Estoy grabando este el 1ro de enero de 2013.

En el episodio de hoy, vamos a armar estas recetas desde el comienzo. Comenzaremos con un repaso de los fundamentos de objetos, luego veremos como las funciones trabajan en javascript. Seguido, cubriremos prototipos y herencia, luego polimorfismo y sobreescritura de métodos. Conversaremos sobre clases e instancias en el modelo de prototipos, luego veremos cómo se relacionan estos conceptos con el modelo clásico que la mayoría utiliza. Por último, nos meteremos en el funcionamiento interno del operador instanceof, antes de terminar con un vistazo de futuros caminos, una herramienta para explorar estos conceptos en profundidad y mis recomendaciones.

Es un gran tema y vamos a cubrir mucho terreno. Asegurate de aprovechar los botones de pausa y retroceso de tu reproductor de video. También puedes usar estas referencias para ir directo a las secciones de interés.

## 1. Fundamentos de Objetos
Comencemos con lo básico. Estos son los tipos comunes en JavaScript—son todo lo que realmente puedes tipear en tu código fuente. De cualquier forma, la mayoría de estos no son realmente tipos primitivos. Los únicos que son primitivos son los strings, números y booleanos; un par de maneras de decir “no puedo encontrarlo” y objetos. Todo lo demás—funciones, arrays y expresiones regulares, así como todos los objetos que utilizas en tus programas—son todas variantes de objetos.

Entonces, ¿qué es un objeto? Bueno, es un conjunto de pares de clave/valor. En otros lenguajes tal vez lo llames diccionario, hash, array asociativo, pero fundamentalmente son pares de clave/valor. Puedes usar cualquier número de claves con un nombre dado, tan largo como un string, y cada string estará asociado con un valor. Estos valores pueden ser de cualquier tipo: alguno de los primitivos básicos, así como cualquier tipo de objeto, incluyendo funciones y, por supuesto, objetos en sí mismos.

Algo importante a tener en cuenta sobre los objetos es que, si bien los primitivos se pasan por valor, los objetos se pasan por referencia. Veamos qué quiere decir esto.

O sea que si tenemos dos variables, llamadas number1 y number2, y asignamos un valor a number1—digamos 3.14 etcetera—y luego copiamos esta variable dentro de number2, estos dos valores no están conectados. Si cambiamos number2, number1 no es afectado.

Los objetos, por otro lado, son almacenados por referencia. Lo que quiere decir esto es que si asignas un objeto a una de estas variables y luego copiamos ese objeto en una nueva variable, no estamos copiando el objeto. Sigue habiendo un solo objeto. Lo que estamos copiando es la referencia. El puntero. El cursor. Si cambiamos object2, object1 se modifica también. object2.a es 42, así como, object1.a is 42.

Queda una cosa antes de terminar con lo básico.

Si buscas una propiedad que no se encuentra en un objeto, obtendrás undefined como resultado. Ahora bien, puedes asignar undefined a cualquier propiedad que quieras. Esto no elimina la propiedad. Si por alguna razón necesitas eliminar por completo la propiedad del objecto, debes utilizar el operador delete. Honestamente, en el día a día, esta distinción no es muy importante.

## 2. Funciones y Métodos
Anteriormente he dicho que las funciones son objetos particulares, y eso es verdad. Cuando defines una función, Javascript crea un objeto que tiene tres propiedades predefinidas: name, que es el nombre de la función; length, que es el número de argumentos; y prototype, que explicaré luego.

Como las funciones son sólo objetos, puedes hacer con ellas las mismas cosas que harías con un objeto común. Puedes asignarles propiedades; puedes asignarlas a variables; y cuando lo haces, ellas se pasan por referencia, entonces puedes ejecutarlas en su nueva localización simplemente diciendo el nombre de la variable y paréntesis.

Cuando pones una función dentro de un objeto, típicamente es llamada un “método”. Puedes ejecutar métodos como a cualquier función, diciendo object.methodname y luego paréntesis.

Cuando haces esto, JavaScript asignará la palabra clave this al objeto utilizado. Entonces si dices myObject.get(), this referencia a myObject. Luego de que la función retorne, this volverá a referenciar valor que tenía antes.

La palabra clave this es uno de los grandes temas en Javascript. Depende del objeto, no del contexto en donde la función fue definida, y eso puede causar muchos problemas.

Entonces, si tenemos myMethod que devuelve this.val, y llamamos object1.get(), obtendremos 42. Si llamamos a object2.get(), this será asignado a object2 y obtendremos 3.14159. Si llamamos directamente a myMethod(), this no será asignado a nada en particular. Puede ser undefined si estamos usando la directiva strict mode; puede ser el objeto global; es difícil asegurar su valor.

Tienes que ser realmente cuidadoso al usar this. Cuando sea posible, recomiendo utilizar el strict mode ya que esto forzará que this sea undefined. Esto no va a prevenir problemas relacionados  a this, pero te ayudará a identificarlos más rápido.

En caso de necesitar que this referencie a un valor en particular, puedes forzar su asignación utilizando call(), apply() o bind(). Entrar en detalle en el uso de estas funciones excede el alcance de este material, pero déjame mostrarte un ejemplo de uso.

Si decimos myMethod.call(), se ejecutará la función myMethod forzando el valor de this a lo que digamos. Entonces myMethod.call(object1) asignará la refencia this al object1.

## 3. Herencia de Prototipos
Bien, estas son las bases. Empecemos a meternos en temas los más complicados.

Es un poco raro que te pongas a definir todos tus objetos desde cero. Típicamente tendrás algún tipo de patrón repetido. En este caso, por ejemplo, object1, object2, y object3 utilizan la misma función. En vez de definirlas a todas por separado, lo que a la larga sería una pesadilla de mantenimiento, lo que puedes hacer es usar algo llamado “prototipos”.

Esto funciona así: tu defines un único objeto, y luego tienes otros objetos que heredan de éste, o lo “extienden”. La forma de hacer esto es llamando a Object.create().

Entonces si tengo un objeto base con una función y un valor, puedo crear un nuevo objeto (que he llamado hijo) diciendo Object.create(child). Puedes hacer con este objeto hijo lo que harías con cualquier objeto. Puedes agregarle valores o incluso extenderlo con otro hijo.

La parte interesante es cuando comenzamos a usar estos objetos.

El objeto base sigue siendo el mismo. Digamos que invocamos parent.get(). Bien, this será asignado al objeto parent, entonces JavaScript buscará por get en ese objeto. Cuando lo encuentre, llamará a esa función, que dice devolver this.val, esto causará que Javascript busque val en parent. Hasta acá todo es como esperamos.

Pero ahora se pone interesante. Si llamamos a child.get(), ahora this referencia a child. JavaScript buscará el método get en child, pero no lo encuentra. Entonces buscará en su prototipo—subirá un eslabón en la cadena de prototipos—examinando a parent, encontrando al método get allí. Cuando encuentre la función, tratará de devolver this.val, lo que significa que volverá a child, buscando val, encontrandolo. Entonces child.get() devolverá 3.14 en vez de 42, incluso cuando está utilizando la función definida en parent.

JavaScript atravesará toda la cadena de prototipos si es necesario para encontrar una propiedad. Si decimos grandchild.get(), Javascript buscará get en el objeto grandchild. Como no puede encontrarlo, irá al prototype buscando get en child. No lo encontrará aquí, entonces irá a parent, busca y encuentra a get allí. Llamará a la función, intentando devolver this.val, entonces nuevamente buscará en grandchild la propiedad val. No la encontrará, entonces irá a child, busca val, la encuentra, y devuelve 3.14159.

Estos son los fundamentos de la herencia en JavaScript. Ahora, si has visto otras maneras de hablar sobre objetos en JavaScript, seguramente estarán pensando en clases u otra cosa primero. Pero lo que acabamos de ver—esta herencia basada en prototipos—es la forma en la que JavaScript funciona. Actualmente, JavaScript no tiene otra forma de herencia más que la prototípica que estoy mostrando aquí.

Hay una cuestión más que me gustaría compartir, y es que cada objeto tiene un prototipo, excepto el objeto base y los objetos que son creados explícitamente sin prototipo. Así es como se ven.

Por defecto, los objetos que creas tienen a Object.prototype como su prototipo, y las funciones tienen a Function.prototype como su prototipo. Puedes notar que de aquí provienen los métodos call(), apply(), y bind() que mencioné hace poco.

Ahora, mostrar todo esto en las visualizaciones sería hacerlas demasiado detalladas, por eso voy a utilizar [[Object]] y [[Function]] para referir a esos prototipos de ahora en adelante.

## 4. Polimorfismo & Sobreescritura de métodos

Una vez que tienes objetos en una cadena de prototipos, te encontrarás necesitando hijos que se comporten distinto a sus padres, incluso cuando el mismo método es invocado. Llamamos a esto “polimorfismo”. Polimorfismo significa, “el mismo nombre, diferente comportamiento”. Ahora, técnicamente, puedes obtener polimorfismo sin herencia, pero no vamos a hablar de eso ahora.

En JavaScript es fácil generar polimorfismo; utilizas el mismo nombre para la propiedad, asignándole otro método. Entonces si queremos tener un objeto firmAnswer que responda algo distinto—o de otra manera—podríamos simplemente decir firmAnswer.get y asignar la función. En este caso, lo que vamos a hacer es devolver el mismo valor agregando “!!” al final.

Entonces si decimos answer.get(), esto devolverá “42”, pero si decimos firmAnswer.get(), esto devolverá “42!!”. Supongo que captas la idea.

Esta relación es un poco más fácil de ver si no tenemos las visualizaciones de la función aquí, por lo que voy a dejar de mostrarlas por ahora.

Ahora podrás notar que nuestra fn2 busca su this.val y nuestra fn1 busca this.val. Aquí hay un poco de código duplicado. Si bien no parece tan malo, en programas más complejos encontrarás que este tipo de lógica es muy difícil de mantener. Típicamente, que lo vamos a querer es llamar fn1 desde fn2.

Desafortunadamente, no es tan fácil como parece. La respuesta obvia sería simplemente llamar fn1… diciendo answer.get(). Esto no funciona. Devuelve la respuesta incorrecta. Va a devolver 42. ¿Sabes por qué? Si no, puedes pausar el video para ver si te das cuenta.

Bien, vamos a explicarlo.

Cuando llamamos firmAnswer.get(), this es asignado a firmAnswer y JavaScript busca la propiedad get. La encuentra y ejecuta fn2. Esto ejecuta answer.get(), lo que asignará this a answer, para luego buscar la propiedad get en answer. Cuando la encuentra, ejecutará fn1 y tratará de devolver this.val. Como this está apuntando a answer, cuando busque por this.val, encontrará la propiedad en el objeto answer devolviendo 42 en vez de 3.14159, que es el valor esperado.

Entonces, para hacer que funcione correctamente, necesitamos utilizar la función call. Necesitamos decir answer.get.call(this). Puedes intentar descubrir por qué funcionaría esto.

Bien, así es cómo esto funciona.

Cuando llamamos firmAnswer.get(), this es asignado a firmAnswer… busca por get… lo encuentra… ejecuta fn2… y ahora dice answer.get.call(this). Esto asigna a this, bueno, el valor al que apunta this nuevamente. Pero luego ejecuta answer.get() directamente, que intenta devolver this.val, buscando la propiedad val en firmAnswer, la encuentra, devolviendo la respuesta esperada.

## 5. Clases e Instancias
Puedes organizar tus objetos Javascript de la manera que quieras, aunque una manera bastante común de hacerlo es separando tus métodos de tus datos.

Por ejemplo, tenemos este objeto answer que, cuando le pides que te de su value, devuelve el value que tiene almacenado.

Bien, normalmente quieres tener múltiples copias de este objeto, entonces la gente generalmente pondrá la función en un prototipo—al que llamaremos AnswerPrototype—y luego tendrán múltiples objetos extendiendo ese prototipo para darle valores especiales.

Aquí vemos que lifeAnswer tiene un value de 42, ya que es la respuesta de la Vida, el Universo, y Todo (ver Hitchhiking guide to the galaxy), y dessertAnswer tiene un value de pi, por, bueno, razones obvias (dessert = postre, pi = pie = pastel).

Si quisieras especializar esta respuesta, como hicimos con firmAnswer, puedes hacer lo mismo. Tenemos nuestro FirmAnswerPrototype mas su fn2—la que agrega “!!” al final—que extiende AnswerPrototype. También tenemos nuestra luckyAnswer y magicAnswer extendiendo a esta.

Cuando utilizas esta forma de organizar tus objetos, los prototipos son generalmente llamados “clases,” y los objetos—los que los extienden—generalmente llamados “instancias”. Una clase que extiende a otra es llamada “subclase”.

Se le dice “instanciar” a esta creación de una instancia, y te darás cuenta que esto implica un proceso de dos pasos. Primero, se crea el objeto extendiendo el prototipo, segundo, se inicializan sus estados. (Recuerda, las instancias usualmente se ocupan de los estados y los prototipos de los métodos—las clases de los métodos.) Entonces extendemos y luego inicializamos.

El problema que tiene esto es que la lógica de inicialización es duplicada cada vez que creamos un objeto. Esto no es tan problemático en un ejemplo sencillo, pero en programas reales, la lógica de inicialización suele ser más complicada. Entonces no queremos duplicarla en todos lados. Eso sería un gran problema de mantenimiento.

Esto también vulnera el encapsulamiento. Una de las cosas lindas que tiene la OOP es que nos permite decidir cómo nuestros datos van a ser almacenados de una manera en la que nadie más deba preocuparse. Simplemente provees acceso a esos datos a través de tus métodos y luego, si quieres cambiar la manera en la que los datos son almacenados, lo haces. Actualizas tus métodos—los de tu objeto—sin tener que actualizar el resto del programa.

Pero aquí, como todas nuestras instancias acceden a val directamente, no podemos cambiar la forma en que val es almacenado sin tener que cambiar a todas nuestras instancias.

Entonces lo que generalmente haces es usar algún tipo de función de inicialización. En este caso, voy a llamarla constructor(). Este es un método que muchos utilizan para inicializar sus objetos.

Así es como funciona. Digamos que queremos crear una nueva instancia de magicAnswer. Extendemos FirmAnswerPrototype y luego decimos magicAnswer.constructor(3). Esto llamado asignará this a magicAnswer y luego buscará la propiedad constructor. No la encontrará en magicAnswer, entonces la buscará en el prototipo. Tampoco la encontará allí, entonces buscará en el prototipo de FirmAnswerPrototype. Allí la encontrará y ejecutará fn0(value). fn0 va a asignar a la propiedad this._val el value. Entonces localiza a this, asigna el valor, y aquí estamos.

Puedes notar que agregué un guión bajo adelante en _val. Es una convención en la comunidad JavaScript para decir que esta propiedad es “privada”. En otras palabras, no deberías acceder o asignar valores a esa propiedad. Ahora bien, en JavaScript no hay forma de forzar a que esto sea así, pero esta es la manera correcta de trabajar. Y si sigues esta regla, esto significa que podemos cambiar AnswerPrototype sin cambiar la manera en que FirmAnswerPrototype debe ser programada para todas nuestras instancias.

Este es un panorama completo de herencia prototípica en JavaScript. Es un poco distinto a lo que mostré al comienzo, todavía no terminamos con esto. Pero utilizando este modelo puedes programar completamente orientado a objetos con JavaScript.

## 6. El modelo Clásico
Ahora, observemos el modelo clásico. Se va a construir sobre todo lo que hicimos hasta ahora.

Para comenzar, si recuerdas, en el modelo de prototipos, instanciamos un objeto creandolo y luego llamando a algún tipo de constructor, ¿no?. Esto es tan común que JavaScript tiene una palabra especial para hacerlo. Esta palabra es new.

De todas formas, new es un poquito, bueno, rara. No funciona de la manera que hemos visto hasta ahora, y eso es lo que diferencia al modelo clásico que estoy por mostrar con el de prototipos que estuve mostrando hasta ahora.

Ahora, antes de meternos en eso, tengo que mostrarte algo un poco raro sobre las funciones. ¿Recuerdas esa propiedad prototype que dije que explicaría luego? Bueno, voy a explicarla ahora. Y es… es rara.

Al crear una función, JavaScript crea un objeto con las propiedades name, length y prototype. Esta propiedad prototype en realidad referencia a un objeto completamente nuevo con una propiedad constructor que referencia a la función que acabas de crear. Entonces cada vez que defines una función en JavaScript, estás creando dos objetos: el de la función, y también su objeto prototype que está colgado por ahí.

Te dije que era raro.

Observemos esto más de cerca. ¿No te parece familiar?

Si. Es un prototipo. Es, básicamente, una clase. O sea que cada vez que defines una función en JS, estás realmente definiendo un constructor que está asociado con esta mini clase que no-hace-nada.

Ahora, claro está, no toda función que uno define fue pensada para ser un constructor. Entonces la convención en JS es que, si la pensamos como un constructor, debería comenzar con una letra mayúscula, y si fue pensada como una función tradicional, entonces la nombramos con minúsculas al comienzo.

Bien, hasta aquí un poco de la rareza con las funciones. Veamos como se pone en juego esto en el modelo clásico.

Primero, volvamos a nuestro modelo de prototipos. Vamos a revisarlo paso a paso.

Lo que vamos a hacer es crear una clase llamada AnswerPrototype. Vamos a crear un constructor en ella. Y cuando creamos ese constructor, JavaScript va a definir un objeto para la función y otro para su prototipo (al que no le daremos importancia; lo vamos a ignorar). También vamos a crear una función get en  AnswerPrototype y un par de instancias: lifeAnswer y dessertAnswer.

Este es un ejemplo básico usando el modelo de prototipos. Ahora hagamos lo mismo, pero esta vez utilizando el modelo clásico y la palabra new.

En el modelo clásico, se define el constructor primero. Entonces creemos la función Answer. JavaScript va a crear automáticamente un objeto a su lado, con una propiedad constructor que referencia a la misma función Answer. Este prototipo es nuestra clase. Es la que va a cumplir el mismo propósito que AnswerPrototype en el ejemplo previo. Entonces asignamos nuestro método get en AnswerPrototype. Luego podemos instanciarlo llamando a new Answer(). Vamos a decir new Answer(42) y esto nos va a dar la instancia correcta. Esto va a crear un hijo de Answer.prototype inicializandolo llamando al constructor Answer pasandole el valor 42.

Para saber que al crear a lifeAnswer debe usar a Answer.prototype como su prototipo JavaScript inspecciona la propiedad prototype presente en la función al utilizar la palabra clave new.

Lo mismo sucede con dessertAnswer.

Esto es el modelo clásico.

Esto se pone un poco más complicado cuando comenzamos a lidiar con subclases. Observemos cómo sería con una subclase. Agreguemos la clase FirmAnswer que hicimos en nuestro ejemplo previo con prototipos.

Nuevamente, vamos a comenzar creando el constructor FirmAnswer en primer lugar. JavaScript creará automáticamente el objeto FirmAnswer.prototype, pero no podemos utilizarlo ya que necesitamos que FirmAnswer.prototype extienda a Answer.prototype—y esto no es así. Lo que haremos es, asignar la propiedad FirmAnswer.prototype a un nuevo objeto que crearemos extendiendo a Answer.prototype. Esto hará que el viejo FirmAnswer.prototype quede sin referenciar, siendo recolectado por el garbage collector.

Luego, asignaremos la propiedad constructor para volver a referenciar a FirmAnswer. Ahora, hasta donde puedo decir, esta propiedad constructor no es necesaria. Todo lo que estamos haciendo con JS funciona bien sin esa propiedad. Pero vamos a asignarlas para ser consistentes. Quizá quieras probar en saltear este paso, pero debo admitir, no he sido lo suficientemente valiente para hacerlo, por lo que hazlo asumiendo el riesgo.

Bien, tenemos el FirmAnswer.prototype con su constructor. Ahora necesitamos asignarle el método get. Luego podemos instanciarlo normalmente diciendo new FirmAnswer(7). Esto creará nuestra luckyAnswer extendiendo FirmAnswer.prototype y podemos hacer lo mismo con magicAnswer.

Aquí hay una comparación de ambos modelos. He numerado las secciones para que correspondan, el número uno en el modelo de prototipos está haciendo lo exactamente lo mismo que el uno en el modelo clásico. Puedes pausar el video aquí si quieres estudiar esto en detalle.

## 7. instanceof
Hay algo más que quisiera compartirles.

A menudo es conveniente saber qué clase se usó para instanciar un objeto. Para ello, JavaScript tiene una palabra clave: instanceof. Funciona examinando al prototipo del constructor y comparándolo con el objeto. Es un poco complicado de entender a menos que lo veas, así que voy a mostrartelo.

Podemos preguntar si lifeAnswer es una instancia de Answer. Intuitivamente sabemos que es así. ¿Pero cómo lo sabe JavaScript? Bien, esto es lo que sucede:

Primero, examina a Answer.prototype. No al prototipo de Answer, sino a la propiedad prototype de Answer. Luego examina el prototipo de lifeAnswer. Si estas dos cosas son el mismo objeto, si, es una instancia.

Entonces lifeAnswer no podría ser una instancia de FirmAnswer. FirmAnswer.prototype está aquí. El prototipo de lifeAnswer está por aquí. No coinciden, entonces no es instanceof.

De todas formas, existe una salvedad—una excepción a la regla. luckyAnswer es una instancia de Answer porque JavaScript inspeccionará toda la cadena de prototipos. O sea, Javascript examinará Answer.prototype. Luego examinará al prototipo de luckyAnswer. No coinciden, pero subirá en la cadena de prototipos. Como coinciden, luckyAnswer es una instancia de Answer.

## 8. Future Directions
Esto es todo lo que debes saber para comprender cómo funciona la herencia en Javascript. Quiero compartir algunas cosas más, igualmente.

Primero, en la próxima versión de JavaScript, como es definida en la especificación EcmaScript 6 / ES2015, se incorpora una nueva sintaxis que simplifica la manera de hacer un modelo de herencia clásico. Es la sintaxis de class, y la podemos ver aquí a la izquierda.

Ahora, hasta donde pude investigar, esto genera exactamente el mismo modelo que vimos como herencia clásica, tal como muestro a la derecha. Pero será más fácil para trabajarlo.

Si quieres ver una comparación con el modelo clásico, aquí hay una parte por parte. Si quieres estudiarlo en profundidad, puedes pausar el video.

## 9. La guía definitiva
Yo llamé a este episodio “La guía definitiva para OOP en Javascript” lo que quizá es un poco pretencioso de mi parte. No es posible para un tutorial, sin importar su duración—y no lo quería hacer muy largo—de todas formas, no es posible cubrir en un tutorial absolutamente todo lo que hay para saber de JavaScript. Hay mucho material que dejé intencionalmente afuera, como getters y setters, variables estáticas, y más… la implementación de bind y apply, por ejemplo…

Lo que convierte a este video en la guía definitiva, es el  website que he creado para acompañarlo. Este sitio es un visualizador de objetos. Entonces puedes escribir código JavaScript y el sitio lo analizará mostrando el mapa de objetos en pantalla. Entonces si queremos saber qué hizo una función, podríamos añadirla aquí. Podemos tocar “Show all functions” y ver cómo las funciones se muestran como objetos.

Es una herramienta muy cool. Es muy divertida; tiene muchos ejemplos predefinidos para que juegues. Dale un vistazo. Esta es la mejor manera para comprender realmente cómo la herencia funciona en JavaScript y cómo funcionan los objetos también. Testea tu conocimiento escribiendo un poco de código, clickeando el botón de evaluate y viendo qué obtienes. Juega con tus propias maneras de generar estructuras de herencia; intenta tus abstracciones; prueba las abstracciones de los demás.

De cualquier forma, este es el recurso verdaderamente definitivo porque de hecho se ejecuta en tu navegador. Entonces te dirá lo que te diga tu navegador—te dirá cuál es el modelo Javascript utilizado por tu navegador. Dale un vistazo.

## 10. Recomendaciones
Para cerrar, estas son mis recomendaciones para que programes orientado a objetos con Javascript.

Primero, usa el modelo clásico. Se que no es muy popular, y es un poco raro, pero es el estándar. Todo el mundo lo entiende. Si usas el modelo clásico, cualquiera con una cercanía a la programación OOP en Javascript va a entender este acercamiento. No podemos decir lo mismo de otras opciones que utilices.

Es casi la única manera que tiene un buen soporte en las IDEs si quieres disponer de algún tipo de automatización, y es la única propuesta que soporta instanceof. Se puede hackear el soporte a instanceof utilizando un modelo de prototipos, pero no es muy conveniente ya que el código termina siendo más feo que el modelo clásico.

Segundo, "use strict";. Usa el modo estricto. Ayudará a prevenir situaciones donde la referencia this no esté bien definida.

Tercero, usa JSHint o algún linter similar. Te ayudará a detectar casos donde olvidaste usar new o lo utilizaste en el lugar equivocado, así como a utilizar convenciones como el uso de mayúsculas al nombrar una clase.

Cuarto, cuando la nueva sintaxis de EcmaScript 6 de clases esté disponible, ve y usala. Al momento de grabar este video, no está disponible en ninguno de los browser populares, y eso implica que va a pasar bastante hasta que esté disponible en IE. Pero cuando esté disponible, úsala. Tiene una sintaxis agradable y elegante. Se corresponde con el modelo clásico común, lo que significa que se lleva bien con todo lo que venías haciendo.

Por último, experimenta con el Object Playground. Creé este sitio especialmente para que sea una manera de aprender más sobre OOP en Javascript. Te sorprenderás con lo que puedes encontrar sobre esto. Por ejemplo, echale un vistazo para saber si necesitás o no la propiedad constructor. ¿Qué pasa cuando la eliminas? ¿Puedes encontrar un caso en el que la necesites, o un caso en el que no la necesites?

Bueno, esto es lo que he aprendido sobre programación orientada a objetos en JavaScript. Gracias a todos por mirar, ¡nos vemos la próxima!

# JakartaOne 2026 --- SCRIPT

> Guion hablado para una sesión de 30 minutos. Objetivo real: terminar
> alrededor de **28:30**.

## 00:00--02:30 · El problema

**\[PANTALLA\]** README de la demo o pantalla limpia con el nombre de la
charla.

Hola a todos. Soy Diego Silva, y hoy quiero presentarles un proyecto en
el que he estado trabajando que se llama **Eclipse Coffee Builder for
Jakarta EE**.

Pero antes de enseñarles Coffee Builder, quiero empezar con el problema
que intenta resolver.

Los que desarrollamos aplicaciones empresariales con Java sabemos que
cuando comenzamos un proyecto hay muchas decisiones importantes que
tomar.

Tenemos que pensar en nuestro modelo, nuestra arquitectura,
persistencia, servicios, APIs, la interfaz...

Esas son decisiones que **sí quiero tomar como desarrollador**.

Pero entre esas decisiones hay también una cantidad considerable de
trabajo que repetimos una y otra vez.

Crear la estructura del proyecto. Configurar dependencias. Crear el
`persistence.xml`. Configurar un datasource. Crear entidades,
repositorios, modelos, mappers...

Y si tenemos una aplicación web, configurar Jakarta Faces, crear backing
beans, páginas, formularios...

Todo eso es código perfectamente válido y necesario.

El problema es que una buena parte es también **predecible**.

Y después de hacerlo suficientes veces aparece una pregunta bastante
natural:

> Si ya conozco la estructura que quiero, ¿por qué tengo que volver a
> escribirla desde cero?

**\[PAUSA\]**

De esa pregunta nace Coffee Builder.

## 02:30--05:00 · Qué es Coffee Builder

**\[PANTALLA\]** `eclipse-ee4j/coffeebuilder`.

Coffee Builder es un proyecto para **automatizar la construcción
incremental de aplicaciones Jakarta EE**.

Y quiero remarcar la palabra *incremental*.

Coffee Builder no pretende ser otro framework que se coloque encima de
Jakarta EE.

No introduce un runtime que nuestra aplicación necesite para funcionar.

Y tampoco intenta generar una aplicación completa y decirnos: "listo,
esto es lo que tienes".

La idea es bastante más sencilla.

Partimos de un proyecto Jakarta EE y le vamos diciendo a Coffee Builder
qué capacidades queremos incorporar.

Persistencia. Modelos de dominio. Repositorios. Mapeo. Jakarta Faces.
PrimeFaces. Configuración para ejecutar la aplicación...

Y Coffee Builder va construyendo esa base utilizando tecnologías que ya
conocemos.

El resultado no es "código Coffee Builder".

Es Jakarta Persistence. Jakarta Data. CDI. Jakarta Faces. MapStruct.
PrimeFaces. Maven.

Cuando Coffee Builder termina, lo que queda es **nuestro proyecto**.

Podemos abrir una clase generada, entenderla, modificarla, borrarla o
continuar desarrollándola normalmente.

Y eso lleva a una idea que quiero que tengan presente durante toda la
demostración:

> **Coffee Builder genera la base. El desarrollador construye la
> aplicación.**

Hoy no voy a intentar mostrar todas las capacidades del proyecto porque
tenemos treinta minutos y Coffee Builder ya tiene bastante más que eso.

En cambio, vamos a hacer algo que creo que es mucho más interesante:

**vamos a construir una aplicación.**

## 05:00--06:30 · Personal Issue Tracker

**\[PANTALLA\]** `apuntesdejava/jakartaone-esp-2026-expo`, luego
`demo/entities.json`.

Bueno, hablar es fácil.

Vamos a ver si de verdad funciona.

Vamos a construir un pequeño **Personal Issue Tracker**.

Nada demasiado sofisticado, pero tampoco quiero hacer una aplicación con
una entidad que tenga solamente `id`, `name` y ya.

Vamos a tener proyectos y dentro de esos proyectos vamos a registrar
issues.

Nuestro modelo tiene dos entidades principales: `Project` e `Issue`.

Un proyecto tiene su identificador, una clave, un nombre y una
descripción.

Y un Issue ya tiene algunas cosas más interesantes.

**\[SEÑALAR\]** `project`, `title`, `description`, `type`, `status`,
`priority`, `labels`, `createdAt`.

Tenemos una relación con `Project`, campos normales, enums, una lista de
etiquetas y fecha/hora.

¿Por qué puse todo esto?

Porque quiero que Coffee Builder tenga que tomar algunas decisiones.

Si todos los campos fueran `String`, generar un formulario sería
bastante poco impresionante.

## 06:30--08:30 · Intención vs. presentación

**\[PANTALLA\]** Alternar `entities.json` y `forms.json`.

`entities.json` describe principalmente **qué tenemos**.

Coffee Builder puede saber que `priority` es un enum, que `labels` es
una lista de `String` o que `project` representa una relación.

Pero hay una decisión diferente: ¿cómo quiero presentar esas cosas al
usuario?

Para eso tenemos `forms.json`.

Aquí no quiero volver a repetir que `description` es un `String` o que
`createdAt` es un `LocalDateTime`. Eso ya lo sabe Coffee Builder.

Aquí describimos decisiones de **presentación**.

**\[MOSTRAR\]** `description.component = textarea`.

La descripción técnicamente es un `String`. Si no digo nada, Coffee
Builder podría generar un campo de texto normal. Pero yo quiero un
`textarea`.

**\[MOSTRAR\]** `project.displayField`.

`project` es una relación con un objeto `Project`, pero en un combo
quiero mostrar su nombre. Por eso puedo indicar `displayField`.

Fíjense en la separación:

`entities.json` describe principalmente la **semántica del modelo**.

`forms.json` describe principalmente **cómo quiero presentar ese
modelo**.

Y cuando no especifico una decisión de presentación, Coffee Builder
intenta inferirla.

Un número puede convertirse en `inputNumber`, una fecha en `datePicker`,
un enum en `selectOneMenu`, una relación en un selector y una lista de
`String` en chips.

No voy a configurar todo eso manualmente.

Quiero que lo veamos aparecer.

Ya tenemos entonces la intención.

Nos falta la aplicación.

**\[PANTALLA\]** PowerShell.

Así que ahora sí... vamos a construirla.

## 08:30--09:30 · Archetype

Hasta ahora solamente tenemos dos archivos de definición. No tenemos
aplicación.

Voy a empezar creando un proyecto Jakarta EE mínimo utilizando el
archetype de Coffee Builder.

Aquí estoy indicando el archetype, las coordenadas de nuestra aplicación
y el package base.

**\[ENTER\]**

El objetivo del archetype es deliberadamente pequeño. Prefiero comenzar
con una base Jakarta EE y agregar capacidades a medida que las necesito.

## 09:30--10:30 · Persistencia

Voy a utilizar `add-persistence`.

Le indico la persistence unit, el datasource, la URL de H2 y dónde
quiero declarar ese datasource.

**\[ENTER\]**

Coffee Builder agrega las dependencias necesarias, configura Jakarta
Persistence, crea el datasource y prepara el proyecto.

**\[OPCIONAL\]** Mostrar `web.xml` y `MODE=LEGACY`.

Yo no puse `MODE=LEGACY` en el comando. Coffee Builder conoce ciertas
configuraciones por defecto a través de su catálogo JDBC. Si yo
especifico explícitamente ese parámetro, mi configuración tiene
prioridad.

## 10:30--11:30 · Entidades

Ahora voy a ejecutar `add-entities`.

Aquí prácticamente toda la información está en un solo parámetro:
`entities-file`.

Es el JSON que acabamos de revisar.

**\[ENTER\]**

A partir de esa definición se generan las entidades de persistencia, los
enums, las relaciones y los repositorios de Jakarta Data.

## 11:30--12:45 · Modelo de aplicación

Pero yo no quiero que mi aplicación trabaje directamente con las
entidades de persistencia.

Voy a ejecutar `add-domain-models`, utilizando el mismo `entities-file`.

**\[ENTER\]**

Aquí Coffee Builder construye modelos de dominio, contratos de
repositorio, implementaciones y mappers. Para el mapeo estamos
utilizando MapStruct.

**\[PANTALLA\]** Mostrar `domain/` e `infrastructure/`.

No hemos cambiado de framework.

Seguimos viendo clases Java, interfaces, CDI, Jakarta Data, MapStruct...

Coffee Builder está escribiendo una base que después nosotros podemos
seguir trabajando.

## 12:45--13:30 · Jakarta Faces

Ahora quiero convertir esto en una aplicación web.

Ejecutamos `add-faces`.

Este goal agrega la configuración necesaria para Jakarta Faces y prepara
una página inicial de navegación.

**\[ENTER\]**

Todavía nuestro índice está prácticamente vacío. Eso va a cambiar cuando
empecemos a crear páginas.

## 13:30--14:00 · Template

Voy a crear también un template Facelets.

El goal es `add-face-template`.

Le indico el nombre y una región llamada `body`, donde se insertará el
contenido de nuestras páginas.

**\[ENTER\]**

Los templates son recursos de layout, así que no aparecen como páginas
de navegación.

## 14:00--15:30 · CRUD tipado

Y ahora viene la parte interesante.

Tenemos nuestro modelo. Tenemos Faces. Tenemos un template.

Voy a ejecutar `add-forms-from-entities`.

Este goal recibe nuestros dos archivos: `entities-file`, que describe el
modelo, y `forms-file`, que describe las decisiones de presentación.

**\[ENTER\]**

Coffee Builder combina ambas cosas.

Sabe que `status` es un enum, que `project` es una relación, que
`labels` es una lista, que `createdAt` contiene fecha y hora, y además
sabe que pedimos explícitamente un `textarea` para la descripción.

Así que puede elegir componentes PrimeFaces apropiados y generar el
CRUD.

**\[PANTALLA\]** Mostrar `ProjectList.xhtml`, `IssueList.xhtml`, beans e
`index.xhtml`.

Y nuestro índice también fue creciendo.

Coffee Builder registró las páginas que acaba de generar: **Projects.
Issues.**

Esto es incremental. Si el desarrollador ya tiene su propio
`index.xhtml`, Coffee Builder lo respeta por defecto.

## 15:30--17:30 · Payara y build final

Nos falta una última cosa: ejecutar la aplicación.

Voy a agregar la configuración para Payara Micro con `add-payaramicro`.

**\[ENTER\]**

Ahora sí:

``` powershell
.\mvnw.cmd clean verify "-Ppayaramicro" "payara-micro:start"
```

Hasta ahora deliberadamente no he detenido la demostración para compilar
después de cada paso.

Vamos a hacer una sola construcción completa al final.

Y si todo lo que acabamos de generar es coherente, esto debería
compilar, empaquetarse y desplegarse.

**\[ENTER\]**

Mientras Payara levanta:

Partimos de un proyecto mínimo. Agregamos persistencia. Generamos el
modelo de persistencia. Construimos el modelo de aplicación. Agregamos
Faces. Y finalmente generamos una interfaz CRUD a partir de la semántica
que habíamos definido.

Todo eso está ahora dentro de este proyecto.

**\[PAYARA READY\]**

Bueno... parece que sobrevivió. 😄

Vamos a verlo.

## \~17:30--18:00 · Home

**\[PANTALLA\]** `http://localhost:8080/personal-issue-tracker/`

Y esta es nuestra aplicación.

No es precisamente el website del año... 😄

pero recuerden: Coffee Builder genera la base. El diseño final ya es
trabajo nuestro.

Esta página tampoco la escribí yo. A medida que Coffee Builder generó
páginas, las fue registrando aquí.

Empecemos por Projects.

## 18:00--19:00 · Dos Projects

Primero necesitamos proyectos porque nuestros Issues tienen una relación
con ellos.

Crear:

``` text
CB | Coffee Builder | Eclipse Coffee Builder development
J1 | JakartaOne Demo | JakartaOne LiveStream 2026 demo
```

Ya tenemos dos proyectos. Ahora viene la parte más interesante.

Volver al índice → `Issues`.

## 19:00--21:00 · Primer Issue

**\[NEW\]**

Ahora fíjense en este formulario.

Yo no indiqué manualmente qué componente debía utilizarse para cada
campo.

Coffee Builder está combinando lo que sabía del modelo con las
decisiones de `forms.json`.

En `Project` aparecen nuestros dos objetos. Gracias al `displayField`,
mostramos el nombre.

Seleccionar `JakartaOne Demo`.

Pegar:

``` text
Title: Prepare JakartaOne Coffee Builder demo
Description: Validate the complete Coffee Builder demo flow for JakartaOne LiveStream 2026.
```

El título es un `String` normal, así que tenemos un input.

La descripción también es `String`, pero nosotros pedimos explícitamente
un `textarea`.

Seleccionar:

``` text
Type: TASK
Status: TODO
Priority: MEDIUM
```

Estos campos son enums, así que Coffee Builder generó selectores con sus
valores.

Agregar chips:

``` text
jakartaone
demo
coffee-builder
```

`labels` era nuestro `List<String>`, así que aquí tenemos chips.

Usar datePicker.

Y finalmente nuestro `LocalDateTime`: fecha y hora.

**\[SAVE\]**

Y ahí está.

Tenemos una aplicación que persiste un Issue relacionado con un Project,
con enums, una colección y fecha/hora.

Y nosotros no escribimos este CRUD.

Eso es exactamente la clase de trabajo repetitivo que quería sacar del
camino.

## 21:00--21:45 · Update

Editar Issue 1.

Pero claro, si estamos preparando esta charla, este Issue ya no debería
seguir en `TODO`.

Cambiar `TODO → IN_PROGRESS` y `MEDIUM → HIGH`.

Y después de todo lo que nos costó llegar hasta aquí... creo que también
podemos subirle la prioridad. 😄

**\[SAVE\]**

Create, read y update.

## 21:45--23:00 · Segundo Issue y Delete

Crear rápidamente:

``` text
Project: Coffee Builder
Title: Update Coffee Builder documentation
Description: Align the public documentation with the current Coffee Builder features.
Type: TASK
Status: DONE
Priority: MEDIUM
Labels: documentation, website
```

Este segundo Issue también resulta bastante real, porque fue justamente
lo último que hicimos antes de preparar esta charla.

Pero ya está terminado.

Así que...

**\[DELETE + confirmar\]**

...podemos borrarlo.

Y ahora sí tenemos el CRUD completo.

**\[OPCIONAL\]** Señalar selección múltiple: "También tenemos selección
múltiple y borrado en lote."

## 23:00--26:00 · ¿Qué acabamos de generar?

**\[PANTALLA\]** IDE.

Ahora quiero volver un momento al código.

Porque podríamos quedarnos con la impresión de que Coffee Builder hizo
alguna especie de magia.

Pero aquí no hay magia.

Hay código.

Tenemos entidades de Jakarta Persistence, repositorios con Jakarta Data,
modelos de dominio, mappers con MapStruct, CDI, backing beans de Jakarta
Faces y páginas XHTML con PrimeFaces.

Y todo está dentro de nuestro proyecto.

**\[ABRIR `IssueListBean.java`\]**

Señalar `@Named`, `@ViewScoped` e inyecciones.

Este bean no necesita Coffee Builder para ejecutarse.

Es un bean de Faces normal.

Aquí tenemos CDI. Aquí nuestros repositorios.

Si mañana quiero cambiar este código, lo cambio.

Coffee Builder ya hizo su trabajo.

**\[ABRIR `IssueList.xhtml`\]**

Señalar `p:selectOneMenu` y/o `p:chips`.

Y lo mismo ocurre con la página.

Esto es PrimeFaces normal.

Podemos modificarla, cambiar el layout, reemplazar componentes...

La aplicación no está atada a Coffee Builder.

## 26:00--28:30 · Cierre

Y esto es lo que quería mostrarles: Coffee Builder ya hizo su trabajo.

A partir de aquí seguimos nosotros.

Hoy recorrimos un solo camino: persistencia, modelo de aplicación y una
interfaz con Jakarta Faces y PrimeFaces.

Coffee Builder tiene más capacidades que no alcanzan a entrar en estos
treinta minutos, incluyendo otros goals y generación a partir de
OpenAPI.

Todo eso lo pueden revisar con más calma en los repositorios y en la
documentación del proyecto.

El código de Coffee Builder está en:

`eclipse-ee4j/coffeebuilder`

La documentación actual está en:

`eclipse-ee4j/coffeebuilder-website`

Y también voy a dejar disponible el repositorio de esta demostración,
con los JSON y la secuencia utilizada para reproducirla:

`apuntesdejava/jakartaone-esp-2026-expo`

Si quieren experimentar, modificar el modelo o simplemente ver qué
código genera Coffee Builder, tienen todo disponible allí.

Y si tuviera que resumir estos treinta minutos en una sola idea, sería
esta:

> **Coffee Builder genera la base.\
> El desarrollador construye la aplicación.**

Muchas gracias.

**\[STOP. Entregar al moderador / preguntas.\]**

# JakartaOne 2026 Demo — Coffee Builder

Esta guía contiene la secuencia probada para generar y ejecutar la aplicación **Personal Issue Tracker** utilizando Eclipse Coffee Builder.

> Los comandos están preparados para PowerShell.
>
> Los parámetros `-D...` se escriben entre comillas para evitar problemas de interpretación en PowerShell.

## Punto de partida

Repositorio de la demo:

```text
C:\proys\apuntesdejava\jakartaone-esp-2026-expo
```

Archivos de definición:

```text
demo\entities.json
demo\forms.json
```

La aplicación se genera dentro de:

```text
work\personal-issue-tracker
```

## 1. Crear el proyecto Jakarta EE

Desde la raíz del repositorio:

```powershell
New-Item -ItemType Directory -Force .\work | Out-Null
Set-Location .\work
```

Generar el proyecto:

```powershell
mvn archetype:generate `
  "-DarchetypeGroupId=org.eclipse.coffeebuilder" `
  "-DarchetypeArtifactId=jakarta-ee-minimal-archetype" `
  "-DarchetypeVersion=0.1.0-SNAPSHOT" `
  "-DgroupId=dev.apuntesdejava" `
  "-DartifactId=personal-issue-tracker" `
  "-Dversion=1.0.0-SNAPSHOT" `
  "-Dpackage=dev.apuntesdejava.personal.issue.tracker" `
  "-DinteractiveMode=false"
```

Entrar al proyecto:

```powershell
Set-Location .\personal-issue-tracker
```

Validar el proyecto mínimo:

```powershell
.\mvnw.cmd verify
```

Resultado esperado:

```text
BUILD SUCCESS
```

## 2. Agregar persistencia

```powershell
mvn "org.eclipse.coffeebuilder:coffee-builder-maven-plugin:0.1.0-SNAPSHOT:add-persistence" `
  "-Dpersistence-unit-name=defaultPU" `
  "-Ddatasource-name=defaultDatasource" `
  "-Durl=jdbc:h2:mem:test;DB_CLOSE_DELAY=-1" `
  "-Ddeclare=web.xml"
```

Coffee Builder aplica automáticamente los parámetros JDBC definidos para H2.

El datasource generado debe contener:

```xml
<url>jdbc:h2:mem:test;DB_CLOSE_DELAY=-1;MODE=LEGACY</url>
```

No es necesario especificar manualmente `MODE=LEGACY`.

## 3. Generar entidades

```powershell
mvn "org.eclipse.coffeebuilder:coffee-builder-maven-plugin:0.1.0-SNAPSHOT:add-entities" `
  "-Dentities-file=C:\proys\apuntesdejava\jakartaone-esp-2026-expo\demo\entities.json"
```

Validar:

```powershell
.\mvnw.cmd verify
```

En esta etapa se generan, entre otros elementos:

- entidades Jakarta Persistence;
- relaciones entre entidades;
- enums;
- repositorios de infraestructura.

## 4. Generar Domain Models

```powershell
mvn "org.eclipse.coffeebuilder:coffee-builder-maven-plugin:0.1.0-SNAPSHOT:add-domain-models" `
  "-Dentities-file=C:\proys\apuntesdejava\jakartaone-esp-2026-expo\demo\entities.json"
```

Validar:

```powershell
.\mvnw.cmd verify
```

Se generan:

- domain models;
- domain repositories;
- implementaciones de repositorio;
- mappers MapStruct;
- semántica de identidad basada en el identificador de cada modelo.

## 5. Agregar Jakarta Faces

```powershell
mvn "org.eclipse.coffeebuilder:coffee-builder-maven-plugin:0.1.0-SNAPSHOT:add-faces"
```

Coffee Builder crea automáticamente:

```text
src/main/webapp/index.xhtml
```

y configura:

```xml
<welcome-file>index.xhtml</welcome-file>
```

El `index.xhtml` es administrado por Coffee Builder mientras conserve su marcador de propiedad.

Si ya existe un `index.xhtml` creado por el desarrollador, se preserva por defecto.

El reemplazo debe ser explícito:

```powershell
mvn "org.eclipse.coffeebuilder:coffee-builder-maven-plugin:0.1.0-SNAPSHOT:add-faces" `
  "-Doverwrite=true"
```

## 6. Crear el Facelet template

```powershell
mvn "org.eclipse.coffeebuilder:coffee-builder-maven-plugin:0.1.0-SNAPSHOT:add-face-template" `
  "-Dname=WEB-INF/templates/main" `
  "-Dinserts=body"
```

El template generado debe contener:

```xml
<ui:insert name="body">body</ui:insert>
```

## 7. Generar los CRUD PrimeFaces

```powershell
mvn "org.eclipse.coffeebuilder:coffee-builder-maven-plugin:0.1.0-SNAPSHOT:add-forms-from-entities" `
  "-Dforms-file=C:\proys\apuntesdejava\jakartaone-esp-2026-expo\demo\forms.json" `
  "-Dentities-file=C:\proys\apuntesdejava\jakartaone-esp-2026-expo\demo\entities.json"
```

Validar:

```powershell
.\mvnw.cmd verify
```

Se generan las páginas:

```text
ProjectList.xhtml
IssueList.xhtml
```

El índice de navegación se actualiza automáticamente:

```text
Application pages

- Projects
- Issues
```

Los componentes PrimeFaces son inferidos utilizando la semántica de `entities.json` y las decisiones de presentación de `forms.json`.

Entre los componentes utilizados están:

- `p:inputText`
- `p:inputTextarea`
- `p:inputNumber`
- `p:datePicker`
- `p:selectOneMenu`
- `p:chips`

## 8. Agregar Payara Micro

```powershell
mvn "org.eclipse.coffeebuilder:coffee-builder-maven-plugin:0.1.0-SNAPSHOT:add-payaramicro"
```

## 9. Construir y ejecutar

```powershell
.\mvnw.cmd clean verify "-Ppayaramicro" "payara-micro:start"
```

Esperar mensajes similares a:

```text
personal-issue-tracker was successfully deployed
Running on PrimeFaces 15.0.17
Payara Micro ... ready
```

Abrir:

```text
http://localhost:8080/personal-issue-tracker/
```

## 10. Flujo funcional de demostración

### Projects

Abrir:

```text
Projects
```

Crear un proyecto, por ejemplo:

```text
Key:         PROJECT-1
Name:        Project One
Description: JakartaOne demo project
```

Guardar.

### Issues

Volver al índice y abrir:

```text
Issues
```

Crear un Issue.

Ejemplo:

```text
Project:     Project One
Title:       Issue 1
Description: A great issue
Type:        TASK
Status:      TODO
Priority:    MEDIUM
Labels:      one, two, last
Created At:  seleccionar fecha/hora
```

El campo Project utiliza la relación `ManyToOne`.

Los enums utilizan `selectOneMenu`.

`List<String>` utiliza PrimeFaces Chips.

### Update

Editar el Issue y modificar, por ejemplo:

```text
Status:   IN_PROGRESS
Priority: HIGH
```

Guardar y comprobar que la tabla refleja los cambios.

### Delete individual

Eliminar un Issue mediante el botón de la fila.

### Delete múltiple

Crear varios Issues.

Seleccionar dos filas.

El botón superior debe mostrar algo similar a:

```text
2 issues selected
```

Ejecutar Delete.

Las filas seleccionadas deben desaparecer.

El borrado individual debe continuar funcionando después de un borrado múltiple.

## Resultado esperado

A partir de dos archivos declarativos:

```text
entities.json
forms.json
```

Coffee Builder genera una aplicación Jakarta EE que incluye:

```text
Jakarta Persistence
        ↓
Jakarta Data
        ↓
Domain Models
        ↓
Repositories
        ↓
MapStruct
        ↓
Jakarta Faces
        ↓
PrimeFaces
        ↓
Payara Micro
```

El proyecto resultante compila, empaqueta y ejecuta sin modificaciones manuales al código generado.

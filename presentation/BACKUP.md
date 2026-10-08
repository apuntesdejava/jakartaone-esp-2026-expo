# JakartaOne 2026 --- Backup / Contingencias

> Objetivo: no improvisar decisiones bajo presión.

## Principio

**Un fallo técnico no debe convertirse en el tema de la charla.**

Si algo no responde rápidamente, cambiar al plan preparado y continuar
la historia.

## Preparación

-   [ ] Coffee Builder `develop` actualizado e instalado localmente.
-   [ ] Repo de la demo actualizado.
-   [ ] `work/` vacío para el flujo principal.
-   [ ] Dependencias Maven presentes localmente.
-   [ ] Java/Maven verificados.
-   [ ] Navegador abierto.
-   [ ] PowerShell con fuente legible.
-   [ ] Comandos disponibles en `DEMO.md` / historial.
-   [ ] Proyecto de respaldo ya generado y compilado en otra carpeta.
-   [ ] Instancia de respaldo capaz de arrancar sin regeneración.
-   [ ] `DEMO-DATA.md` abierto.
-   [ ] Cronómetro + `RUN-OF-SHOW.md` visibles.

## Si falla un goal

No depurar en vivo.

> Tengo una versión de la aplicación ya generada para no convertir estos
> treinta minutos en una sesión de troubleshooting. Vamos a continuar
> desde ese punto y después pueden reproducir toda la secuencia desde el
> repositorio de la demo.

Cambiar al proyecto preparado.

## Si Maven descarga o falla la red

No esperar indefinidamente. Cambiar al proyecto preparado. El valor está
en el flujo y el código resultante, no en demostrar Maven Central.

## Si Payara no levanta

Corregir solo un error evidente que tome segundos. En otro caso usar la
instancia preparada.

> La generación ya la vimos. Para no gastar la charla diagnosticando el
> runtime, voy a continuar con la instancia que dejé preparada.

## Si falla un CRUD

No corregir código en vivo. Usar instancia de respaldo o pasar al código
generado y continuar la historia.

## Si voy atrasado

**+1 minuto:** no abrir `web.xml`; no mostrar código intermedio.

**+2 minutos:** no bulk delete; crear un solo Issue; reducir Issue 2 a
una mención.

**+3 minutos:** pasar al proyecto/aplicación preparada; mostrar
resultado; recap de código; cierre.

**A 27:30:** ir directamente al cierre.

## Si voy adelantado

1.  Mostrar brevemente `IssueEntity.java`.
2.  Mostrar bulk delete.
3.  Enseñar una segunda parte de `IssueList.xhtml`.
4.  Mencionar más goals en la documentación.

Nunca introducir una feature nueva no ensayada.

## Referencias

-   Proyecto: https://github.com/eclipse-ee4j/coffeebuilder
-   Documentación fuente:
    https://github.com/eclipse-ee4j/coffeebuilder-website
-   Demo: https://github.com/apuntesdejava/jakartaone-esp-2026-expo

## Cierre corto de emergencia

> Lo importante de lo que acabamos de ver no es solamente que Coffee
> Builder pueda generar estas piezas. Es que el resultado sigue siendo
> una aplicación Jakarta EE normal, que podemos entender y continuar
> modificando. Coffee Builder genera la base; el desarrollador construye
> la aplicación. El proyecto, la documentación y esta demo quedan
> disponibles en GitHub. Muchas gracias.

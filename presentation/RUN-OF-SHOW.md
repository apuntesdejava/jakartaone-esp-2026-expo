# JakartaOne 2026 --- Run of Show

> Chuleta operativa. Mantener visible junto al cronómetro. **Objetivo de
> final: 28:30.** **Regla principal: no recuperar tiempo hablando más
> rápido; recuperar tiempo saltando contenido.**

## Checkpoints

         Tiempo Debo estar en                   Siguiente
  ------------- ------------------------------- -----------------------
          00:00 Presentación                    El problema
          02:30 Qué es Coffee Builder           Issue Tracker
          05:00 Repo de la demo                 `entities.json`
          06:30 `forms.json`                    Terminal
      **08:30** **Terminal**                    Archetype
          09:30 Persistencia                    Entidades
          10:30 `add-entities`                  Domain
          11:30 `add-domain-models`             Faces
          12:45 `add-faces`                     Template
          13:30 `add-face-template`             Forms
      **14:00** **`add-forms-from-entities`**   Payara
          15:30 `add-payaramicro`               Build final
    **\~17:30** **Payara / navegador**          Projects
          18:00 Projects                        Crear 2
          19:00 Issues                          Issue 1
          21:00 Update                          Issue 2
      **23:00** **STOP CRUD**                   Código generado
      **26:00** **Cierre**                      Repos/docs
      **28:30** **FIN**                         Entregar al moderador

## Semáforo

-   🟢 **≤ 18:00 al llegar al navegador:** normal.
-   🟡 **19:00--20:00:** reducir explicación del CRUD y no mostrar bulk
    delete.
-   🔴 **\> 20:00:** usar aplicación preparada si hace falta; priorizar
    Issue 1 y resultado.
-   🟡 **24:00 y aún en navegador:** cortar inmediatamente y volver al
    código.
-   🔴 **27:30:** abandonar cualquier demo restante y ejecutar cierre
    corto.

## Qué NO cortar

1.  `entities.json` vs `forms.json`.
2.  `add-forms-from-entities`.
3.  Aplicación funcionando.
4.  Mostrar que el resultado es Jakarta EE/PrimeFaces normal.
5.  Frase de cierre.

## Orden de sacrificio

1.  No abrir `web.xml`.
2.  No mostrar `MODE=LEGACY`.
3.  No abrir clases después de `add-entities`; mostrar solo árbol.
4.  No mostrar `FacesConfiguration.java`.
5.  No abrir `index.xhtml` antes del navegador.
6.  No hacer bulk delete.
7.  Crear solo un Issue.
8.  Reducir el código final a `IssueListBean.java` y una señal de
    `IssueList.xhtml`.

## Goals: frase antes de Enter

  -----------------------------------------------------------------------
  Goal                                Qué decir
  ----------------------------------- -----------------------------------
  Archetype                           Proyecto Jakarta EE mínimo +
                                      coordenadas + package base.

  `add-persistence`                   Persistence unit, datasource, URL
                                      H2 y declaración en `web.xml`.

  `add-entities`                      Un parámetro clave:
                                      `entities-file`.

  `add-domain-models`                 Mismo modelo; genera domain,
                                      repositorios, implementaciones y
                                      mappers.

  `add-faces`                         Agrega Faces y prepara navegación.

  `add-face-template`                 Nombre del template + región
                                      `body`.

  `add-forms-from-entities`           Combina `entities-file` +
                                      `forms-file`.

  `add-payaramicro`                   Configura ejecución con Payara
                                      Micro.
  -----------------------------------------------------------------------

## Pantallas

**Laptop compartida:** repo/README → JSON → PowerShell → IDE → navegador
→ IDE → repos/docs.

**Pantalla auxiliar:** cronómetro grande + este documento +
`DEMO-DATA.md`.

**Ultrawide:** `DEMO.md`, repos, terminal auxiliar y aplicación
preparada de respaldo.

## Reglas de ritmo

-   Explicar **antes** de ejecutar cada goal; los goals son demasiado
    rápidos para hablar mientras corren.
-   No leer comandos Maven completos.
-   Señalar goal + parámetros relevantes.
-   No leer JSON ni Java línea por línea.
-   Cada cambio de pantalla debe tener una razón.
-   A las 23:00, dejar de jugar con el CRUD.
-   A las 26:00, empezar el cierre.

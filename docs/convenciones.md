# Convenciones

## Nombre del repositorio

`kebab-case`, minúsculas, sin acentos ni `ñ`, sin el nombre de la empresa como prefijo
—ya está en la organización.

| Sí | No |
|---|---|
| `gestion-cobranzas` | `GestionCobranzas`, `gestion_cobranzas` |
| `votacion-api` | `fycls-votacion-api` |
| `savi-documentacion` | `SAVI-documentación` |

El nombre describe **qué es**, no quién lo hizo ni cuándo. Si el repositorio es la API
de un producto, el sufijo `-api` ayuda; `-web`, `-app` y `-infra` siguen la misma idea.

## Descripción

Todo repositorio tiene descripción: una línea, en presente, que diga qué hace.
Es lo único que se ve en la lista de la organización.

```bash
gh repo edit fycls-ingenieria/REPO --description "Procesa las inscripciones de la votación anual"
```

## Ramas

`main` es la única rama permanente. Todo lo demás es temporal y se borra al mezclar.

| Prefijo | Para |
|---|---|
| `feat/` | Funcionalidad nueva |
| `fix/` | Corrección de un error |
| `chore/` | Dependencias, configuración, tareas de mantenimiento |
| `docs/` | Solo documentación |
| `refactor/` | Cambio interno sin efecto visible |

Después del prefijo, una descripción corta en `kebab-case`:
`feat/exportar-cartera-excel`, `fix/timeout-en-conciliacion`.

## Commits

[Conventional Commits](https://www.conventionalcommits.org/es/), en español, en presente
y sin punto final.

```
feat(cobranzas): exportar la cartera vencida a Excel
fix(auth): renovar el token antes de que expire
chore: subir Django a 5.1
docs: documentar el flujo de conciliación
```

El cuerpo del commit explica **por qué**, no qué — el qué ya está en el diff. Un commit
resuelve una cosa; si el mensaje necesita un "y", probablemente son dos commits.

## Estructura mínima

Todo repositorio trae, desde el primer commit:

```
.
├── README.md        # qué es, cómo se instala, cómo se corre
├── .gitignore       # apropiado al lenguaje
└── .env.example     # las variables que hacen falta, con valores falsos
```

El `README.md` responde cuatro preguntas, en este orden: qué hace, qué necesito
instalado, cómo lo levanto, cómo lo pruebo. Si alguien del equipo no puede levantar el
proyecto siguiendo solo el README, el README está incompleto.

## Lista de verificación al crear un repo

- [ ] Nombre en `kebab-case` y descripción de una línea
- [ ] Visibilidad **privada**
- [ ] `README.md` con instalación y ejecución
- [ ] `.gitignore` del lenguaje (`gh repo create` lo ofrece con `--gitignore`)
- [ ] `.env.example` si el proyecto usa variables de entorno
- [ ] Acceso otorgado al equipo, no persona por persona
- [ ] Ningún secreto en el primer commit

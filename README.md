# Lineamientos de GitHub — Fycls Ingeniería

Cómo trabajamos en la organización [`fycls-ingenieria`](https://github.com/fycls-ingenieria):
qué acceso tiene cada quien, cómo se nombra y se estructura un repositorio, y cómo
llega el código a `main`.

Este repositorio es la fuente única de la norma. Si algo aquí ya no refleja la
realidad, se corrige aquí mediante un pull request — no en un chat.

## Contenido

| Documento | Para qué |
|---|---|
| [Permisos y roles](docs/permisos.md) | Quién puede qué, y cómo se otorga acceso |
| [Convenciones](docs/convenciones.md) | Nombres, ramas, commits, estructura |
| [Flujo de trabajo](docs/flujo-de-trabajo.md) | De la rama al merge |

## Lo esencial en diez líneas

1. Los repositorios se crean **privados** por defecto.
2. El nombre va en `kebab-case`, en minúsculas, sin acentos.
3. Todo repo nace con `README.md` y `.gitignore`.
4. La rama principal se llama `main` y no se le hace push directo.
5. Las ramas de trabajo son `feat/`, `fix/`, `chore/`, `docs/`.
6. Los commits siguen [Conventional Commits](https://www.conventionalcommits.org/es/).
7. Todo cambio entra por pull request, aunque lo revise una sola persona.
8. Ningún secreto se sube al repositorio — ni en código, ni en `.env`, ni en un commit "que luego borro".
9. El acceso se otorga por equipo, no persona por persona.
10. Solo los *owners* de la organización borran, transfieren o cambian la visibilidad de un repositorio.

## Crear un repositorio nuevo

```bash
gh repo create fycls-ingenieria/nombre-del-proyecto --private --add-readme \
  --description "Una línea que explique qué hace"
```

Quien lo crea queda como **admin de ese repositorio**. Antes de escribir la primera
línea de código, revisa la [lista de verificación](docs/convenciones.md#lista-de-verificación-al-crear-un-repo).

## Pedir acceso

Abre un issue en este repositorio indicando el repo, el nivel que necesitas
(`pull`, `triage`, `push`, `maintain`) y para qué. Los niveles están explicados en
[Permisos y roles](docs/permisos.md).

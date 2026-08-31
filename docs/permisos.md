# Permisos y roles

## Dos capas distintas

GitHub separa el permiso **en la organización** del permiso **sobre cada repositorio**.
Confundirlas es el error más común y el más caro.

### Rol en la organización

| Rol | Qué puede | Quién lo tiene |
|---|---|---|
| **Owner** | Todo, en todos los repositorios. Se salta cualquier restricción de la organización. | Solo quien administra la cuenta |
| **Member** | Crear repositorios (y es admin de los que crea). Cero autoridad sobre los demás. | Todo el equipo |
| **Outside collaborator** | Solo los repositorios donde se le invitó, uno por uno. | Externos y contratistas |

**Member es el rol normal de trabajo.** Un member puede crear todos los repositorios
que necesite y administrarlos por completo, sin ninguna capacidad de tocar los repos de
los demás. Owner se reserva para quien deba poder destruir.

### Nivel sobre un repositorio

Es una escala: cada nivel incluye todo lo del anterior.

| Nivel | Alcance |
|---|---|
| `none` | Ni siquiera aparece en su lista |
| `pull` | Clonar y leer |
| `triage` | Gestionar issues y pull requests, sin escribir código |
| `push` | **Escribir código. El nivel por defecto de un colaborador** |
| `maintain` | Configurar el repositorio, sin poder borrarlo |
| `admin` | Borrar, transferir, cambiar visibilidad |

Otorga `push` salvo que exista una razón concreta para dar más.

## Cómo se otorga el acceso

El permiso base de la organización es `none`: nadie ve un repositorio hasta que se le
da acceso explícito. Se otorga **por equipo**, no persona por persona — así se revisa de
un vistazo y se revoca en un solo lugar.

```bash
# Agregar a alguien al equipo
gh api -X PUT orgs/fycls-ingenieria/teams/desarrollo/memberships/USUARIO -f role=member

# Dar al equipo acceso a un repositorio
gh api -X PUT orgs/fycls-ingenieria/teams/desarrollo/repos/fycls-ingenieria/REPO -f permission=push
```

Para un acceso puntual a un solo repositorio, sin pasar por el equipo:

```bash
gh api -X PUT repos/fycls-ingenieria/REPO/collaborators/USUARIO -f permission=push
```

## Restricciones vigentes en la organización

| Ajuste | Valor | Efecto |
|---|---|---|
| `default_repository_permission` | `none` | El acceso nunca es automático |
| `members_can_create_repositories` | `true` | Cualquiera puede crear repos |
| `members_can_create_public_repositories` | `false` | Los repos nacen privados |
| `members_can_delete_repositories` | `false` | Nadie borra ni transfiere repos, ni los propios |
| `members_can_change_repo_visibility` | `false` | Nadie vuelve público un repo privado |
| `members_can_invite_outside_collaborators` | `false` | Los externos los invita un owner |
| `members_can_fork_private_repositories` | `false` | No se saca copia de un repo privado a una cuenta personal |

Para auditar el estado real en cualquier momento:

```bash
gh api orgs/fycls-ingenieria --jq 'to_entries[] | select(.key | startswith("members_") or startswith("default_")) | "\(.key) = \(.value)"'
```

Y para ver quién tiene acceso a un repositorio:

```bash
gh api repos/fycls-ingenieria/REPO/collaborators --jq '.[] | .login + " → " + .role_name'
```

## Límite conocido del plan

La organización está en **GitHub Free**, que no ofrece *branch protection* ni *rulesets*
en repositorios privados. La API responde `403 — Upgrade to GitHub Pro or make this
repository public` al intentar crear una regla.

Consecuencia práctica: en los repos privados **`main` no está protegida técnicamente**.
No se puede impedir por configuración un `push --force`, un merge sin revisión o el
borrado de la rama. Mientras eso siga así, la regla de "todo entra por pull request" se
sostiene por disciplina del equipo, no por el sistema. Los repositorios públicos sí
aceptan reglas sin costo adicional.

## Secretos

Ningún secreto entra al repositorio. Ni claves, ni tokens, ni cadenas de conexión, ni
archivos `.env`, ni en un commit temporal — un commit borrado sigue siendo recuperable
y un repo privado que se vuelve público arrastra todo su historial.

Para lo que necesite el CI:

```bash
gh secret set NOMBRE_DEL_SECRETO --repo fycls-ingenieria/REPO
```

Si un secreto llegó a subirse: **rótalo primero**, y después limpia el historial. Nunca
al revés.

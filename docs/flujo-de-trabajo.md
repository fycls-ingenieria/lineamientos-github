# Flujo de trabajo

De la rama al merge. El ciclo completo en seis pasos.

## 1. Partir de `main` actualizada

```bash
git switch main && git pull
git switch -c feat/exportar-cartera-excel
```

## 2. Trabajar en commits pequeños

Un commit por decisión. Es más fácil revisar seis commits claros que uno de 800 líneas,
y es la diferencia entre revertir un cambio puntual o revertir una semana.

## 3. Abrir el pull request

```bash
gh pr create --fill --base main
```

Se abre **en borrador** si todavía no está listo para revisión:

```bash
gh pr create --fill --base main --draft
```

Un PR se explica solo: qué cambia, por qué, y cómo se verifica. Si toca la interfaz,
lleva captura. Si es un fix, dice cómo se reproducía el error.

## 4. Revisión

Todo cambio pasa por al menos una persona, aunque el equipo sea pequeño y el cambio
parezca obvio. La revisión no es un trámite de aprobación: es la única lectura que
tendrá ese código antes de producción.

Quien revisa comenta sobre el código, no sobre quien lo escribió, y distingue lo que
bloquea de lo que es preferencia. Quien recibe la revisión responde a todos los
comentarios, aunque sea para decir por qué no aplica.

```bash
gh pr review 42 --approve
gh pr review 42 --request-changes --body "El timeout queda sin manejar si la API no responde"
```

## 5. Mezclar

Squash merge, para que `main` tenga un commit por cambio y un historial legible.

```bash
gh pr merge 42 --squash --delete-branch
```

## 6. Borrar la rama

`--delete-branch` ya lo hace. Las ramas mezcladas no se quedan: una lista de ramas
muertas esconde las vivas.

---

## Nota sobre la protección de `main`

En el plan actual (GitHub Free) los repositorios privados **no admiten reglas de
protección de rama**. Nada impide técnicamente un push directo a `main` ni un
`push --force`. Este flujo se cumple por acuerdo del equipo.

Si se contrata GitHub Team, lo primero a activar en cada repositorio privado:

```bash
gh api -X POST repos/fycls-ingenieria/REPO/rulesets --input - <<'JSON'
{
  "name": "proteger-main",
  "target": "branch",
  "enforcement": "active",
  "conditions": { "ref_name": { "include": ["refs/heads/main"], "exclude": [] } },
  "rules": [
    { "type": "deletion" },
    { "type": "non_fast_forward" },
    { "type": "pull_request",
      "parameters": {
        "required_approving_review_count": 1,
        "dismiss_stale_reviews_on_push": true,
        "require_code_owner_review": false,
        "require_last_push_approval": false,
        "required_review_thread_resolution": true
      }
    }
  ]
}
JSON
```

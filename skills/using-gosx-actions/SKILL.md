---
name: using-gosx-actions
description: |
  Use this skill when implementing GoSX forms, named server actions,
  mutation handlers, form or JSON parsing, validation failures, field errors,
  redirects, action state, CSRF protection, or POST-redirect-GET flows.
---

# Using GoSX Actions

## When to reach for GoSX actions

Use actions for server-side mutations: create records, update settings, submit forms, delete resources, trigger workflows, and return validation state. Actions are named handlers registered beside a page module.

Use ordinary API routes when the caller is not a page form/action flow. Use an island handler only for local client state that does not need a server mutation.

## What GoSX actions are

An action is `func(ctx *action.Context) error`. The context carries the original request, parsed `FormData`, optional JSON payload, and helpers for structured success, validation failure, and redirect results. File-routed pages expose page-relative `__actions/<name>` endpoints when `route.FileActions` are registered.

## Quick start

Register an action in `page.server.go`:

```go
package app

import (
    "log"
    "strings"

    "github.com/odvcencio/gosx/action"
    "github.com/odvcencio/gosx/route"
)

func init() {
    err := route.RegisterFileModuleHere(route.FileModuleOptions{
        Actions: route.FileActions{
            "subscribe": func(ctx *action.Context) error {
                email := strings.TrimSpace(ctx.FormData["email"])
                if email == "" {
                    ctx.ValidationFailure("Email is required.", map[string]string{
                        "email": "Please enter an email address.",
                    })
                    return nil
                }
                return ctx.Success("Subscribed.", map[string]string{"email": email})
            },
        },
    })
    if err != nil {
        log.Fatal(err)
    }
}
```

Render the form in `.gsx`:

```go
func Page() Node {
    return <form method="post" action={actionPath("subscribe")}>
        <input type="hidden" name="csrf_token" value={csrf.token} />
        <input name="email" value={actions.subscribe.values.email} />
        <p>{actions.subscribe.fieldErrors.email}</p>
        <button type="submit">Subscribe</button>
    </form>
}
```

## When NOT to use GoSX actions

Do not use actions for GET data loading. Use route loaders.

Do not trust client validation. Validate on the server action and return field errors.

Do not skip CSRF for browser form posts. Mount session middleware and include the `csrf_token` field.

Do not return raw ad hoc JSON shapes when `ctx.Success`, `ctx.ValidationFailure`, and `ctx.Redirect` communicate the state the framework expects.

## Going deeper

- `ctx.FormData`: parsed form values.
- `ctx.Payload`: JSON body for client-invoked actions.
- `ctx.ValidationFailure`: field-level errors plus submitted values.
- `ctx.Redirect`: POST-redirect-GET flow.
- `ctx.ActionState(name)`: retrieve flashed state from route context.
- Public source: [odvcencio/gosx](https://github.com/odvcencio/gosx)

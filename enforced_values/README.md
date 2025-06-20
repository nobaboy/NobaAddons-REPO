# Enforced Options

This directory is used by the mod to load `.json` files that override values used for certain config options
at runtime, allowing for disabling broken functionality until a release can be made to fix it.

Overridden values cannot be modified by the user, while the value they originally set is preserved in
the (de)serialized config file.

>[!warning]
> Files in this directory *must not be deleted*, as the mod does not currently have the capability
> to delete files that have since been deleted in the repo; instead, use a `{"type": "false"}` condition.

## Schema

```json5
{
  "override": {
    // Fragment of a full config file, overriding certain values. In this case, this sets `autoShareCorpses` in
    // the Glacite Mineshaft group to false.
    "mining": {
      "glaciteMineshaft": {
        "autoShareCorpses": false
      }
    }
  },
  // When this should be applied; in this case, this simply checks if(true), meaning this is always active.
  // See below for other available conditions.
  // This must always be present.
  "if": {"type": "true"},
  // A message to send in chat on the first join in a session, or after updating the repo.
  "message": "Auto Share Corpses has been disabled due to ..."
}
```

## Available Conditions

<details>
<summary><code>true</code></summary>

Always returns `true`.

```json5
{
  "type": "true"
}
```

</details>
<details>
<summary><code>false</code></summary>

Always returns `false`. **Use this to deactivate an enforced option.**

```json5
{
  "type": "false"
}
```

</details>
<details>
<summary><code>not</code></summary>

Inverts the value of another check.

```json5
{
  "type": "not",
  "check": {"type": "false"}
}
```

</details>
<details>
<summary><code>mod</code></summary>

```json5
{
  "type": "mod",
  "id": "other_mod",
  // optional; only returns true if the version of other_mod is >=1.0.0
  "min": "1.0.0",
  // optional; only returns true if the version of other_mod is <=1.2.0
  "max": "1.2.0"
}
```

</details>
<details>
<summary><code>self</code></summary>

Same as `mod`, but implicitly targets the `nobaaddons` mod `id`.

```json5
{
  "type": "self",
  // optional; only returns true if the mod version is >=1.0.0
  "min": "1.0.0",
  // optional; only returns true if the mod version is <=1.2.0
  "max": "1.2.0"
}
```

</details>
<details>
<summary><code>minecraft</code></summary>

Same as `mod`, but implicitly targets the `minecraft` mod `id`.

```json5
{
  "type": "self",
  // optional; only returns true if the current minecraft version is >=1.21.4
  "min": "1.21.4",
  // optional; only returns true if the current minecraft version is <=1.21.6
  "max": "1.21.6"
}
```

</details>
<details>
<summary><code>environment</code></summary>

Checks if the player is on the given Hypixel server environment.

```json5
{
  "type": "environment",
  // returns true if the player is on alpha.hypixel.net
  "environment": "BETA"
}
```

</details>
<details>
<summary><code>production</code></summary>

Returns `true` if the player is not in a development environment.

```json5
{
  "type": "production"
}
```

</details>
<details>
<summary><code>all</code></summary>

Returns `true` if all provided `checks` also return `true`.

```json5
{
  "type": "all",
  "checks": [
    {"type": "production"},
    {"type": "environment", "environment": "BETA"}
  ]
}
```

</details>
<details>
<summary><code>any</code></summary>

Returns `true` if any of the provided `checks` return `true`.

```json5
{
  "type": "any",
  "checks": [
    {"type": false},
    {"type": "production"}
  ]
}
```

</details>
<details>
<summary><code>none</code></summary>

Returns `true` if none of the provided `checks` return `true`.

```json5
{
  "type": "none",
  "checks": [
    {"type": "true"}
  ]
}
```

</details>

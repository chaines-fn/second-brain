# Push Copilot ai-toolbox marketplace via MDM

For IT: register the `fieldnation/ai-toolbox` plugin marketplace for GitHub Copilot in VS Code across all managed machines, with no plugins enabled by default (developers browse and opt in themselves).

Source: [VS Code enterprise AI settings docs](https://code.visualstudio.com/docs/enterprise/ai-settings#_manage-agent-plugins-and-marketplaces), current as of 2026-08-12.

## Mechanism

VS Code reads "Copilot managed settings" — a governance layer separate from core VS Code enterprise policy — from OS-level managed preferences. This is the same definition Copilot CLI reads, so one push covers both VS Code and the CLI.

Two settings matter:

| Setting key | What it does |
|---|---|
| `extraKnownMarketplaces` | Registers an additional plugin marketplace so it shows up under `@agentPlugins` in the Extensions view. |
| `enabledPlugins` | Force-enables/disables specific plugin IDs by default. **Leave this unset** — that's what keeps "no plugins enabled by default." |

## Windows

Push via Intune (or GPO) to:

- **Registry key:** `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\GitHubCopilot`
- **Value name:** `extraKnownMarketplaces`
- **Value type:** `REG_SZ` (string), containing a JSON blob
- **Value data:**
  ```json
  {"ai-toolbox":"fieldnation/ai-toolbox"}
  ```

Do not create an `enabledPlugins` value at all.

## macOS

Push via Intune (or another MDM) as a custom configuration profile:

- **Preference domain:** `com.github.copilot`
- **Key:** `extraKnownMarketplaces`
- **Value:** same JSON string as above

Do not include an `enabledPlugins` key in the profile.

## Verify

On a test machine after the policy lands, open VS Code and run **Developer: Policy Diagnostics** from the Command Palette. Check the **Managed Settings** section — `extraKnownMarketplaces` should show as applied, and the ai-toolbox marketplace should appear when searching `@agentPlugins` in the Extensions view, with no plugins pre-enabled.

## Two things to confirm before scaling this out

The docs don't show a worked example for these, so validate on one machine first:

1. **JSON shape for `extraKnownMarketplaces` with an auto-update override.** Docs confirm the plain shorthand `"owner/repo[#ref]"` string works. They mention an "enterprise-managed auto-update override" is also possible but don't show its exact JSON shape.
2. **macOS plist value typing.** Docs don't specify whether `extraKnownMarketplaces` should be typed as a `<string>` or `<dict>` in the `.mobileconfig`/profile payload — test with a string first (matching the Windows `REG_SZ` shape) since that's the only form the docs illustrate directly.

## Note: this is separate from core VS Code enterprise policy

VS Code also has its own ADMX (Windows) / `.mobileconfig` (macOS) policy path under `Software\Policies\Microsoft\VSCode`, using the same policy names (`ChatExtraMarketplaces`, `ChatEnabledPlugins`). That path only affects VS Code, not Copilot CLI. If both paths configure the same key on a machine, the Copilot managed-settings value (above) wins — so there's no need to configure both. Use the Copilot managed-settings path since the ai-toolbox rollout is meant to cover Copilot CLI too.

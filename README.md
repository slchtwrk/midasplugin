# midasplugin

A GitHub Copilot skill and custom agent for building plugins for **MIDAS CIVIL NX**.

A CIVIL NX plugin is a static web page in a zip: the host opens `index.html` in
an embedded browser, hands it a MAPI key, and the page talks to CIVIL NX over
HTTPS. There is no SDK. Everything hard about the work is in the API's
undocumented behaviour — and that is what this skill carries.

Everything here was **measured against live CIVIL NX 2026 sessions** while
building and patching real plugins. In several places it corrects the published
JSON manual.

## What's in it

```
.github/
  skills/midasplugin/SKILL.md  the canonical workflow and six critical rules
  agents/                     the selectable MIDAS CIVIL Plugin Builder agent
  copilot-instructions.md     workspace task-context policy
references/
  mapi.md                     connection, error semantics, reads, writes, image capture
  result-tables.md            POST /post/TABLE, tokens, load-case series, the envelope trap,
                              construction stages and the OPT_CS family split
  write-shapes.md             verified payloads for NODE, ELEM, MATL, SECT, THIK, GRUP, LCOM
  host.md                     the plugin-host contract, manifest, packaging, WebView2 quirks
  ui.md                       layout, theming, moaui notes for patching shipped bundles
  testing.md                  the mock + Node harness pattern
  probing.md                  settling an unknown API shape by writing to a scratch model
  pitfalls.md                 the pre-flight checklist
  example-plugin.md           the worked example, annotated: what a finished zip contains
assets/
  template/                   a working plugin: client, mock server, 26-assertion test suite
  scripts/                    pack.ps1 and verify-zip.ps1
examples/                     a real shipped plugin, as the zip that was released
```

## The worked example

`examples/` holds **Concurrent Force Reporter v1.0.0** — a plugin that actually
shipped — as the exact zip that was released. It is there so that "what am I
handing over at the end?" has an answer you can unzip and look at, rather than a
description you have to trust.

`assets/template/` is where a plugin *starts*. The example is what one *ends up
as*: 20 plain-text files at the zip root, nothing compiled or minified, no
`node_modules`, no test suite, no mock server, and no external requests. The
template's `package.json`, `test/` and `mock-midas/` are how a plugin gets built
and are deliberately absent from the archive the host opens.

```powershell
Add-Type -AssemblyName System.IO.Compression.FileSystem
$zip = [System.IO.Compression.ZipFile]::OpenRead(
    (Resolve-Path .\examples\MIDAS_CIVIL_NX_Concurrent_Force_Reporter_v1.0.0.zip).Path)
try { $zip.Entries | Select-Object FullName, Length } finally { $zip.Dispose() }
```

`references/example-plugin.md` walks through it: the manifest and the window-size
trap Windows display scaling causes, why the read/write claim sits in the header,
why the logic lives in DOM-free modules, why `VALIDATION.md` ships inside the zip
and has to say what was *not* checked — and a ship checklist to run your own
archive against.

Ask Copilot to "compare my plugin zip against the example".

## Install

Clone this repository and open it in VS Code with GitHub Copilot enabled:

```powershell
git clone https://github.com/manojkheterpalmidas-arch/midasplugin.git
code .\midasplugin
```

Install Node.js to run the offline mock and test harness. Packaging uses
PowerShell and .NET on Windows.

## Use with GitHub Copilot in VS Code

The [workspace skill](.github/skills/midasplugin/SKILL.md) contains the full,
canonical workflow; there is no root skill or wrapper. The
[custom agent](.github/agents/midas-civil-plugin-builder.agent.md) uses that
skill and the shared `references/`, `assets/`, and `examples/` resources.
These customizations apply to this workspace; they are not installed globally.

Ask Copilot to build or debug a CIVIL NX plugin to trigger the skill, or select
**MIDAS CIVIL Plugin Builder** from the agent picker for a focused workflow.

The [workspace instructions](.github/copilot-instructions.md) apply task-context
selection rules to Copilot chats, not just the custom agent. Keep each plugin's
context and implementation under `tasks/<task-name>/`, with a `brief.md`
identifying its sources, applicable standard editions, examples, code, and tests.
Name the task when starting work; Copilot should ask if the selection is
ambiguous and avoid other task folders unless you authorize cross-task work.
Shared MIDAS guidance remains available. These are behavioral instructions,
not enforced file-access restrictions.

### Start a task brief

Copy [the task brief template](assets/task-brief.template.md) to
`tasks/<task-name>/brief.md`. It includes common agent safeguards and a short
starter brief covering the goal, source material and examples, intended workflow,
and success checks. Fill in what you know and leave unknowns as `TBD`. The agent
reads the context, asks focused follow-up questions, and records approved
decisions as the task develops; a complete specification is not required up front.
The template is version-controlled outside the ignored `/tasks/` folder, while
each populated brief and its task material remain local.

## Update

The skill is corrected whenever a live session turns up something new, so it is
worth checking for updates occasionally. Copilot does not update the checkout
automatically. From the repository root, fetch and review upstream changes before
integrating them, especially if you have local customizations:

```powershell
git fetch origin
git diff HEAD..origin/main
```

Start a new Copilot chat after updating customizations. If the agent or skill
does not appear, reload the VS Code window.

The template runs on its own too, with no Copilot involved:

```powershell
Set-Location .\assets\template
node .\mock-midas\server.js    # then open the URL it prints
# In a separate terminal in the template folder:
node .\test\run.js            # 26 assertions, no CIVIL NX needed
```

## A taste of what's inside

Five behaviours that every plugin gets wrong at least once:

- **Errors arrive as HTTP 200 with an `error` key.** Checking `response.ok`
  reports every rejected write as a success.
- **A successful write also returns a `message` field.** Treating any `message`
  as failure makes the plugin report an error *after* the data landed — and the
  user commits again and duplicates everything.
- **An absent table returns 200 `{"message":""}`; a wrong table key returns
  404.** So 404 means *your key is wrong*, not *the model has none*.
- **A verified key does not mean a live session.** `/mapikey/verify` still
  answers `keyVerified: true` after CIVIL NX closes, with
  `status: "disconnected"`.
- **`OPT_CS` is a mode switch, not a filter.** One `/post/TABLE` request returns
  the construction-stage series *or* everything else, never both — and the
  family it excludes comes back absent at HTTP 200 with no error. A combination
  mixing stage cases with static ones cannot be read in one call.

## Scope and caveats

Written for **CIVIL NX 2026** on Windows. MIDAS can change behaviour between
builds. Treat every shape here as a strong starting point and re-verify anything
a plugin depends on; where something was never confirmed against a live model it
is marked **unverified**, and should stay marked.

Not affiliated with or endorsed by MIDAS IT

Plugins Made till now - 

Construction Stage Reporter---------- Marketplace version 1.0.0 ----------
Construction_Stage_Wizard--------- Marketplace version 1.0.0 ----------
 
Existing
Concurrent Force Reporter---------- Marketplace version 1.0.0 ----------  https://support.midasuser.com/hc/en-us/articles/62123537156889-Concurrent-Force-Reporter

RC Slab and Shell Assessment ---------- Marketplace version 1.0.1 ---------- https://support.midasuser.com/hc/en-us/articles/61971621469849

Eurocode Load Combinations Plugin ---------- Marketplace version 2.0.2 ---------- https://support.midasuser.com/hc/ko/articles/61259382041369

Load Combination Contribution Analyzer ---------- Marketplace version 1.0.3 ---------- https://support.midasuser.com/hc/ko/articles/61258768334233

Model Report Builder ---------- Marketplace version 1.0.1 ---------- https://support.midasuser.com/hc/en-us/articles/61655350763289-Model-Report-Builder

CS454 Auto Lane Generator ---------- Marketplace version 1.0.4 ---------- https://support.midasuser.com/hc/ko/articles/61259225090329

CS 454 Load Assessment Combinations ---------- Marketplace version 1.0.2 ---------- https://support.midasuser.com/hc/en-us/articles/60997850893209

CS 454 Moving Load Generator ---------- Marketplace version 1.0.3 ---------- https://support.midasuser.com/hc/en-us/articles/60998764028185

Eurocode Auto Lane Generator ---------- Marketplace version 1.0.2 ---------- https://support.midasuser.com/hc/ko/articles/61259174909849

Eurocode Moving Load Case Generator ---------- Marketplace version 1.0.1 ---------- https://support.midasuser.com/hc/ko/articles/61259043302041

Skew Grillage Geometry ---------- Marketplace version 1.0.4 ---------- https://support.midasuser.com/hc/en-us/articles/60848423734169

Point to Patch Convertor ---------- Marketplace version 1.0.1 ---------- https://support.midasuser.com/hc/en-us/articles/61486703401753-Point-to-Patch-Convertor

Bulk Tabular Result Exporter ---------- Marketplace version 1.0.1 ---------- https://support.midasuser.com/hc/en-us/articles/60848073556633
 

## Licence

MIT — see [LICENSE](LICENSE). Use it, fork it, ship plugins with it.

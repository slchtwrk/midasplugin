# Workspace Instructions

## Plugin Task Context

These rules apply when creating, editing, or investigating a particular
plugin. Repository maintenance and shared MIDAS guidance changes do not require
selecting a plugin task.

- Establish the active folder under `tasks/<task-name>/` before reading
  task-specific content or making plugin changes. Resolve it from the user's
  explicit task path, plugin name, or explicitly identified plugin files.
- You may list immediate task folder names to identify candidates. Do not read
  other tasks' contents or search across `tasks/` to infer a match. If selection
  is ambiguous, ask the user before proceeding.
- For a new plugin, establish its task name and folder with the user. An
  explicit name or path in the request is sufficient. Do not adopt another
  task because its subject is similar.
- Use the shared [task brief template](../assets/task-brief.template.md) when
  creating `tasks/<task-name>/brief.md`. Start with a reduced brief, extract
  supported details from the active task's context, and clarify gaps as needed
  for the next step rather than requiring a complete specification up front.
  Record approved decisions in the brief; placeholders are not defaults.
- State the active task when starting or switching plugins. Read its
  `brief.md` first to identify the applicable sources, standard editions,
  worked examples, plugin code, and tests. If the brief or required material
  is missing, unreadable, or contradictory, report the issue and ask for
  clarification before implementing affected behavior.
- Scope task-specific searches, reads, edits, and validation commands to the
  active folder. Shared [MIDAS guidance](skills/midasplugin/SKILL.md),
  [references](../references/), [templates](../assets/template/), and
  [shipped examples](../examples/) remain available; they do not establish the
  active task's engineering requirements.
- Do not read or reuse another task's context unless the user explicitly
  authorizes cross-task work. When authorized, identify the source and verify
  applicability before adopting its engineering assumptions or
  interpretations.
- When switching tasks, stop applying the previous task's engineering
  requirements. Keep task selection per conversation; do not create a shared
  "current task" file.
- Pass the active folder and the same context boundaries to delegated agents.

These instructions guide agent behavior; they are not a filesystem
access-control boundary.

## Plugin Files for the Active Task

- Create plugin files under the active task's folder, typically
  `tasks/<task-name>/plugins/`.
- Name plugin files according to their functionality and place them in the
  appropriate subfolder when needed.
- Avoid creating plugin files outside the active task's folder unless the user
  explicitly authorizes it.
- When modifying existing plugin files, follow the active task's requirements
  and avoid inadvertently affecting other tasks.
- Document significant changes in the plugin file or task brief to maintain
  clarity and traceability.
- Maintain a versioning scheme for released plugin files to track changes and
  ensure compatibility with the active task's requirements.
- Keep plugin files in draft status until they have been reviewed and approved
  for use within the active task. Assign a release version number upon approval.
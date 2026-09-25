# lapwOS — ACCEPTED REQUIREMENTS

**Document status:** Source of Truth  
**Lifecycle status:** ACCEPTED decisions only  
**Version:** 0.2  
**Purpose:** This document contains only decisions that have been explicitly accepted for lapwOS. Unresolved design topics, proposals, and ideas belong in `PROPOSED.md` or `IDEA.md`.

---

## 0. How to use this document

- An item in this document is an accepted product decision or an accepted architectural constraint.
- Acceptance does **not** mean every implementation detail is already designed.
- Where this document explicitly says “separate design topic”, implementation must not invent a solution.
- Requirement IDs are stable and must not be reused.
- If an accepted requirement changes, the old requirement should move to `DEPRECATED.md` and a new requirement ID should be created.
- Conversation history is not the permanent source of truth; this document is.

### Requirement lifecycle

`IDEA → PROPOSED → ACCEPTED → DEPRECATED`

Rejected decisions belong in `REJECTED.md`.

---

# 1. Product Scope and Principles

## REQ-001 — Single-user desktop MVP

**Status:** ACCEPTED

lapwOS MVP is a single-user desktop application.

Future multi-user/web/manager functionality is outside MVP and belongs to IDEA/future scope.

---

## REQ-002 — Product-first development

**Status:** ACCEPTED

Product behavior and requirements must be settled before implementation where practical.

Implementation must not silently invent product behavior for unresolved requirements.

---

## REQ-003 — Simple by default

**Status:** ACCEPTED

The main UI should remain simple and operationally fast.

Rarely used details belong in detailed history, reports, secondary screens, or progressive disclosure rather than the primary workflow.

---

## REQ-004 — Avoid over-configuration

**Status:** ACCEPTED

lapwOS should use sensible fixed defaults wherever possible.

Not every behavior should become a user-configurable setting merely because it technically can.

---

# 2. Task — Core

## REQ-005 — Minimal required Task fields

**Status:** ACCEPTED

Only these fields are mandatory when creating a Task:

- Task Name
- Priority

All other Task properties are optional and may be completed later.

---

## REQ-006 — New Task default status

**Status:** ACCEPTED

A newly created Task starts in:

`Başlanmadı`

Creating a Task does not start its timer.

---

## REQ-007 — Fixed New Task entry point

**Status:** ACCEPTED

The application provides a fixed, easily accessible:

`+ Yeni Task`

entry point.

---

## REQ-008 — Task creation exposes optional capabilities

**Status:** ACCEPTED

The Task creation screen exposes available optional creation capabilities, including where applicable:

- Steps
- Templates
- Classifications
- Dates
- Estimates
- Project relationships
- Other already-supported optional Task properties

Optional does not mean mandatory.

---

## REQ-009 — Post-create behavior

**Status:** ACCEPTED

After creating a Task:

- the user remains on the same page/context,
- a brief success notification is shown,
- the notification includes the Task name,
- an `Aç` action is available.

---

## REQ-010 — Task statuses

**Status:** ACCEPTED

The current Task status set is:

1. Başlanmadı
2. Devam Ediyor
3. Beklemede
4. Tamamlandı
5. Arşivlendi

`Çöp Kutusu` is a separate lifecycle state and is not treated as an ordinary active Task status.

---

## REQ-011 — Task state controls

**Status:** ACCEPTED

Context-appropriate timer/lifecycle actions are:

| State | Main action |
|---|---|
| Başlanmadı | Başlat |
| Beklemede | Başlat |
| Devam Ediyor | Durdur / Ara |
| Tamamlandı | Yeniden Aç |
| Arşivlendi | Arşivden Çıkar |
| Çöp Kutusu | Geri Yükle |

Exact timer interaction details remain subject to the Time Engine design.

---

## REQ-012 — Task start

**Status:** ACCEPTED

Starting a Task:

- starts its work timer/UI timing state,
- changes Task status to `Devam Ediyor`,
- records a start event in the Time Engine.

The first actual timer start defines the Task's actual start date.

---

## REQ-013 — Task completion

**Status:** ACCEPTED

A Task can be completed directly without first manually selecting `Beklemede`.

Completing a Task:

- stops its active work timing,
- records the relevant work,
- records a completion event,
- changes the current Task state to `Tamamlandı`.

---

## REQ-014 — Actual start vs creation time

**Status:** ACCEPTED

Task creation time and actual work start time are different concepts.

Creating a Task records a Task-created event/time.

The actual start date is defined by the first actual timer start.

---

# 3. Task — Reopen, Correction, Completion History

## REQ-015 — Reopen completed Task

**Status:** ACCEPTED

A completed Task can be reopened without changing its immutable Task ID.

Reopening does not erase previous completion history.

---

## REQ-016 — Reopen reasons

**Status:** ACCEPTED

When reopening a completed Task, the available reasons are:

1. Devam / Ek Çalışma
2. Düzeltme
3. Revizyon
4. Diğer

The reopen interaction should remain simple rather than becoming a long form.

---

## REQ-017 — Preserve original completion information

**Status:** ACCEPTED

The original completion/bitiş information is preserved when a Task is reopened.

Historical actual dates are not rewritten.

---

## REQ-018 — Multiple completion events

**Status:** ACCEPTED

A Task may have multiple historical completion events.

After the latest completion, its current state may again be `Tamamlandı`.

Total actual work time is derived from the sum of all applicable historical work periods.

---

## REQ-019 — Correction as separate work object

**Status:** ACCEPTED

A correction is a separate work object associated with the original Task.

A correction may contain, as applicable:

- reason,
- creation date,
- start date,
- target finish,
- actual finish,
- status.

---

## REQ-020 — Task history is preserved

**Status:** ACCEPTED

Reopen, additional work, correction, revision, and completion history must not overwrite the underlying historical record.

Detailed history may expose these events while the primary Task screen remains simple.

---

# 4. Task — Deletion and Recycle Bin

## REQ-021 — Recycle Bin

**Status:** ACCEPTED

A mistakenly created or unwanted Task can be moved to:

`Çöp Kutusu`

---

## REQ-022 — Immutable ID in Recycle Bin

**Status:** ACCEPTED

A Task retains its immutable ID while in the Recycle Bin.

---

## REQ-023 — Preserve history during deletion period

**Status:** ACCEPTED

Moving a Task to the Recycle Bin does not immediately erase its:

- Task history,
- Time Engine history,
- historical relationships.

---

## REQ-024 — Restore Task

**Status:** ACCEPTED

A Task can be restored from the Recycle Bin.

Restoration returns it to its pre-delete status and Project relationships.

---

## REQ-025 — Deleted Tasks excluded from normal operation

**Status:** ACCEPTED

Tasks in the Recycle Bin:

- disappear from normal Task lists,
- are excluded from the Decision Engine,
- are not treated as active work.

---

## REQ-026 — Manual emptying

**Status:** ACCEPTED

The user can empty the Recycle Bin manually at any time.

No confirmation dialog is required.

---

## REQ-027 — Fixed 30-day retention

**Status:** ACCEPTED

Recycle Bin retention is fixed at 30 days.

The retention period is not configurable.

After 30 days, normal Task operational data may be permanently deleted according to the retention policy.

---

## REQ-028 — Historical time survives Task deletion

**Status:** ACCEPTED

Permanent Task operational deletion must not corrupt historical time accounting.

Time previously spent on deleted Tasks remains reportable under:

`Silinmiş Görevlere Harcanan Zaman`

---

## REQ-029 — Archive, Recycle Bin, permanent deletion are distinct

**Status:** ACCEPTED

The following are distinct concepts:

- Archive
- Recycle Bin
- Permanent deletion

They must not be treated as interchangeable states.

---

# 5. Task — Notes and Notebook

## REQ-030 — Independent Notebook domain

**Status:** ACCEPTED

Notebook / Not Defteri is a separate domain from Tasks.

It provides quick scratch notes without requiring Task creation.

---

## REQ-031 — Notebook placement

**Status:** ACCEPTED

The Notebook is presented as an independent right-side area in the main desktop workflow.

---

## REQ-032 — Notebook pages

**Status:** ACCEPTED

Notebook supports:

- creating pages,
- editing pages,
- deleting pages.

Notebook pages are permanently deleted directly; they do not use the Task Recycle Bin.

---

## REQ-033 — Notebook is not event-first

**Status:** ACCEPTED

Notebook does not log every edit as Time Engine events.

It is intentionally a current-content scratchpad.

---

## REQ-034 — Manual conversion from Notebook

**Status:** ACCEPTED

If a Notebook note is important, the user manually converts/recreates the relevant information as a Task or Step.

lapwOS does not automatically infer that a note should become a Task.

---

## REQ-035 — Task Notes

**Status:** ACCEPTED

A Task has one Notes section associated with the Task, located under Steps in the Task work area.

There are no per-Step Notes.

---

## REQ-036 — Task Notes history behavior

**Status:** ACCEPTED

Task Notes are current-state content.

They do not have edit history, edit timestamps, or separate Time Engine events.

Task Notes follow the Task's Recycle Bin lifecycle.

---

# 6. Task — Templates

## REQ-037 — Task templates

**Status:** ACCEPTED

Task templates are separate objects from Tasks.

Templates are versioned.

Each template version is immutable.

---

## REQ-038 — Template contents

**Status:** ACCEPTED

A Task template may contain initial Task configuration such as:

- Task name,
- Priority,
- Steps,
- Step weights,
- Classifications,
- Estimate,
- suitable planning/initial fields.

Notes and historical work data are not template history fields.

---

## REQ-039 — Template creates independent Task

**Status:** ACCEPTED

Creating a Task from a template creates an independent Task.

Subsequent changes to the Task do not modify the template.

The Task retains its template origin/version information where appropriate.

---

## REQ-040 — Template name behavior

**Status:** ACCEPTED

A template may auto-fill the Task Name.

The user can edit the generated name.

---

## REQ-041 — Changing template during creation

**Status:** ACCEPTED

A template/version may be changed during Task creation.

When changed:

- previous template-derived values are cleared,
- the newly selected template/version is loaded,
- no preserve/merge dialog is required.

---

## REQ-042 — Save existing Task as template

**Status:** ACCEPTED

An existing Task can be saved as a template.

The original Task remains a Task.

---

# 7. Task — Dates and Estimates

## REQ-043 — Task date fields

**Status:** ACCEPTED

Task supports these distinct concepts:

- Planlanan Başlangıç Tarihi
- Gerçek Başlangıç Tarihi
- Bitiş Tarihi
- Tahmini Süre
- Gerçekleşen Süre
- Hedef Bitiş Tarihi
- Son Tarih

---

## REQ-044 — Date semantics

**Status:** ACCEPTED

Definitions:

- `Planlanan Başlangıç Tarihi` = intended/planned start
- `Gerçek Başlangıç Tarihi` = first actual timer start
- `Bitiş Tarihi` = actual completion date
- `Hedef Bitiş Tarihi` = intended/target finish
- `Son Tarih` = hard deadline

---

## REQ-045 — Estimates are optional

**Status:** ACCEPTED

Task estimates are optional.

An estimate represents expected work duration; it is not a deadline or target date.

---

## REQ-046 — Estimate history

**Status:** ACCEPTED

Changes to Task estimates are historically preservable.

Example:

`30 dk → 60 dk → 90 dk`

while actual work may be `82 dk`.

---

## REQ-047 — Estimates are not auto-updated

**Status:** ACCEPTED

lapwOS does not automatically rewrite a user's estimate based on actual performance.

Warnings about estimation should be aggregate/report-oriented rather than constant nagging.

---

## REQ-048 — Editing completed Task planning fields

**Status:** ACCEPTED

Planning/organizational fields such as estimate and planned dates may be edited after completion.

Historical actual start/end dates are not rewritten.

---

## REQ-049 — Priority remains editable

**Status:** ACCEPTED

Task Priority can be edited even after completion.

---

## REQ-050 — Current fields vs historical events

**Status:** ACCEPTED

Current Task fields may be edited according to their semantics.

Historical Time Engine events are immutable and must not be rewritten.

---

## REQ-051 — Actual duration presentation

**Status:** ACCEPTED

The primary Task screen shows Actual Duration as one total value.

Detailed work periods belong in Task history/details.

---

# 8. Task — Steps

## REQ-052 — Steps are optional

**Status:** ACCEPTED

Not every Task needs Steps.

---

## REQ-053 — Manual Step creation

**Status:** ACCEPTED

Users can add Steps manually using:

`+ Adım Ekle`

Templates may also prepopulate Steps.

---

## REQ-054 — Step editing and ordering

**Status:** ACCEPTED

Template-provided Steps are editable and reorderable.

Drag-and-drop is preferred.

Up/down controls may serve as a fallback.

---

## REQ-055 — Simple Step completion

**Status:** ACCEPTED

Steps are simple checkboxes.

Users may check and uncheck them freely.

No detailed Step history is required.

---

## REQ-056 — Step dependencies

**Status:** ACCEPTED

A Step may have a simple prerequisite/dependency.

The dependency produces a warning but does not block the user from opening or proceeding.

---

## REQ-057 — Weighted Step progress

**Status:** ACCEPTED

Task progress may optionally use weighted Steps.

Defaults:

- equal weight,
- user-editable weights,
- weights do not need to sum to 100,
- the system normalizes them.

Progress may be represented as percentage, numeric value, or graph.

---

## REQ-058 — Time tracking remains at Task level

**Status:** ACCEPTED

Time tracking is performed at Task level, not separately per Step.

---

## REQ-059 — Step completion does not auto-complete Task

**Status:** ACCEPTED

Completing all Steps does not automatically complete the Task.

Completing a Task with incomplete Steps is allowed.

The system may show a small informational message but must not block completion.

---

## REQ-060 — Reopen preserves Step state

**Status:** ACCEPTED

When a Task is reopened, incomplete Steps remain incomplete.

---

# 9. Task — Project Relationships

## REQ-061 — Zero, one, or multiple Projects

**Status:** ACCEPTED

A Task may belong to:

- zero Projects,
- one Project,
- multiple Projects.

---

## REQ-062 — No duplicated Task records

**Status:** ACCEPTED

Multi-Project membership does not duplicate the Task.

The Task retains one immutable Task ID and one set of:

- Priority,
- Status,
- Steps,
- time history.

Projects provide contextual/access relationships.

---

## REQ-063 — Same Task name is allowed

**Status:** ACCEPTED

Separate Task records may have the same name.

Immutable Task ID distinguishes them.

---

## REQ-064 — Project relationship is editable

**Status:** ACCEPTED

Project relationships can be added, removed, or changed later.

---

## REQ-065 — Adding Project includes historical Task time

**Status:** ACCEPTED

When a Task is added to a Project, its historical Task time is included in that Project's time accounting.

This is intentional and is not reversed merely because the relationship was added later.

---

## REQ-066 — Removing Project preserves history

**Status:** ACCEPTED

Removing a current Project relationship removes the current relationship but does not erase historical relationship information.

---

## REQ-067 — Projectless Tasks

**Status:** ACCEPTED

A Task may exist without a Project.

Projectless Tasks are available under:

`Günlük İşler`

---

## REQ-068 — Project Priority is not Decision Engine input

**Status:** ACCEPTED

Project Priority is not used directly by the Decision Engine.

---

# 10. Project — Core

## REQ-069 — Nested Project hierarchy

**Status:** ACCEPTED

Projects support nested hierarchy:

`Project → Subproject → ... → Task → Step`

The data model has no artificial fixed depth limit.

The UI must remain usable at practical depths.

---

## REQ-070 — One direct parent

**Status:** ACCEPTED

A Project has at most one direct parent Project.

The Project hierarchy is a tree.

---

## REQ-071 — Direct Tasks and Subprojects

**Status:** ACCEPTED

A Project may contain:

- direct Tasks,
- child Projects/Subprojects.

---

## REQ-072 — Task visibility through hierarchy

**Status:** ACCEPTED

Tasks associated with descendant Projects can be visible/filterable from ancestor Project contexts.

This is a visibility/context rule.

Visibility must not create duplicate Task records or duplicate source time events.

---

## REQ-073 — Project dependencies are not MVP

**Status:** ACCEPTED

Project-level dependencies are outside MVP.

Task and Step dependencies are the supported dependency concepts for MVP.

---

## REQ-074 — Project creation minimum

**Status:** ACCEPTED

Only Project Name is mandatory when creating a Project.

---

## REQ-075 — Project Priority

**Status:** ACCEPTED

Project Priority is optional.

It exists for organization, filtering, and reporting only.

It does not feed the Decision Engine.

---

## REQ-076 — Project name changes

**Status:** ACCEPTED

Project names can be changed.

Where historical/audit context requires it, previous names may remain available in history.

---

## REQ-077 — Project color

**Status:** ACCEPTED

Project color is optional and visual only.

It does not affect priority or Decision Engine behavior.

---

## REQ-078 — Project Description

**Status:** ACCEPTED

Project Description represents the current editable purpose/context of the Project.

---

## REQ-079 — Project Notes

**Status:** ACCEPTED

Project Notes are separate dated entries intended to preserve historical project notes.

---

## REQ-080 — Manual Project Task order

**Status:** ACCEPTED

Projects may have a manual internal Task order.

The order is organizational/visual only.

It is not:

- Priority,
- Dependency,
- Decision Engine ranking.

---

## REQ-081 — Order/dependency conflict

**Status:** ACCEPTED

If manual order conflicts with a dependency, the system may warn.

It must not silently reorder the user's Task list.

---

# 11. Project — Lifecycle and Archive

## REQ-082 — Project views

**Status:** ACCEPTED

Project navigation supports:

- Active Projects
- Archived Projects
- All Projects

Default view is Active Projects.

---

## REQ-083 — Sidebar Project ordering

**Status:** ACCEPTED

Project sidebar supports drag/drop ordering.

`Günlük İşler` remains a fixed top-level entry.

Archive is positioned near the bottom.

---

## REQ-084 — Archive behavior

**Status:** ACCEPTED

Archiving a Project may cascade to child Subprojects and Tasks when they are exclusively under that Project.

Tasks shared with another active Project remain active.

Historical associations are preserved.

---

## REQ-085 — Project restore

**Status:** ACCEPTED

Restoring a Project reactivates it.

Child items restore to their remembered pre-archive states where applicable.

---

## REQ-086 — Project completion

**Status:** ACCEPTED

Project completion is manually managed.

Before a Project can be marked Completed:

- there must be no active direct/descendant Tasks,
- child Projects must be Completed or Archived,
- archived Tasks do not count as active.

---

## REQ-087 — Project dates

**Status:** ACCEPTED

Project supports distinct date concepts:

- Start
- Target Finish
- Deadline
- Actual Finish

Project dates are independent of Task dates.

---

## REQ-088 — Project estimate

**Status:** ACCEPTED

Project may show both:

- manually entered Project Estimate,
- Task-derived estimated total.

These are separate values.

---

## REQ-089 — Project actual time

**Status:** ACCEPTED

Project Actual Time is derived from the Time Engine.

For nested Projects:

`direct Task time + descendant Project time`

must not double-count the same underlying source events.

---

## REQ-090 — Project templates

**Status:** ACCEPTED

Project Templates are versioned.

Template versions are immutable.

---

## REQ-091 — Recurring Project template version choice

**Status:** ACCEPTED

Recurring Project creation may use either:

- the latest template version, or
- a fixed template version.

Existing Project instances are not changed when a template is later edited/versioned.

---

# 12. Decision Engine

## REQ-092 — Suggest, do not force

**Status:** ACCEPTED

The Decision Engine suggests the next Task.

It does not force the user to execute the suggestion.

---

## REQ-093 — Candidate exclusions

**Status:** ACCEPTED

The Decision Engine excludes from normal candidates:

- Tamamlandı
- Arşivlendi
- Çöp Kutusu
- Tasks with incomplete required prerequisites

---

## REQ-094 — Waiting Tasks

**Status:** ACCEPTED

Waiting Tasks are not automatically excluded solely because their status is `Beklemede`.

The waiting reason/context matters.

External waiting may temporarily reduce eligibility; user-initiated waiting may remain eligible.

Exact scoring behavior is a separate design topic.

---

## REQ-095 — Working context

**Status:** ACCEPTED

The current active Project/working context can constrain or influence Decision Engine candidates.

---

## REQ-096 — Next-in-flow signal

**Status:** ACCEPTED

A natural successor to the current work can receive a stronger recommendation signal.

---

## REQ-097 — Ranking signals

**Status:** ACCEPTED

The Decision Engine may use signals including:

- Priority
- Deadline proximity
- FIFO/age
- current context
- dependency eligibility
- next-in-flow
- other accepted future signals

Exact mathematical scoring remains a separate design topic.

---

## REQ-098 — Priority and Deadline are separate

**Status:** ACCEPTED

Priority/importance and Deadline are separate Decision Engine concepts.

Their relative weights are configurable.

The configured weights total 100%.

Example:

`60% Priority / 40% Deadline`

---

## REQ-099 — Project Priority excluded

**Status:** ACCEPTED

Project Priority does not directly affect Decision Engine ranking.

---

## REQ-100 — Explainability

**Status:** ACCEPTED

Decision Engine recommendations must provide concise reasons.

Examples:

- same Project,
- deadline today,
- previous Task continuation,
- waiting for 4 days.

The user should be able to understand why a Task was recommended.

---

## REQ-101 — Task dependencies

**Status:** ACCEPTED

Tasks support simple:

`Ön koşul / Depends On`

relationships.

Multiple prerequisites are allowed.

All required prerequisites must be completed for normal eligibility.

Dependencies do not block opening or starting a Task; they produce warnings/eligibility signals.

---

## REQ-102 — Circular dependency handling

**Status:** ACCEPTED

Circular dependencies are detected and warned about.

lapwOS must not silently delete, rewrite, or auto-resolve the dependency.

---

## REQ-103 — Future Decision Engine signals

**Status:** ACCEPTED AS FUTURE DESIGN CONSTRAINT

Potential future signals include:

- FIFO/age
- remaining estimate
- waiting duration
- context
- dependency
- target date
- other useful signals

These are not automatically MVP scoring rules.

---

# 13. Spontaneous Work

## REQ-104 — First-class spontaneous work

**Status:** ACCEPTED

Spontaneous work is a first-class concept.

The user must be able to start/capture it with minimal input.

---

## REQ-105 — Retroactive Task conversion

**Status:** ACCEPTED

Spontaneous work can later be associated with/converted into a Task while preserving the historical time already spent.

Future recurring Task creation may also be supported later.

---

# 14. Time Engine

## REQ-106 — Event-first architecture

**Status:** ACCEPTED

The Time Engine is event-first.

The visible timer is a UI layer over an underlying historical event model.

The system must be able to reconstruct past activity from events.

---

## REQ-107 — Minimum Time Event Vocabulary

**Status:** ACCEPTED

lapwOS'un temel Time Engine event vocabulary'si aşağıdaki event'lerden oluşur:

### Work

- `WORK_STARTED`
- `WORK_PAUSED`

### Waiting

- `WAITING_STARTED`
- `WAITING_ENDED`

### Interruption

- `INTERRUPTION_STARTED`
- `INTERRUPTION_ENDED`

### Task Lifecycle

- `TASK_COMPLETED`
- `TASK_REOPENED`

`WORK_STARTED`, Task veya Spontaneous Work üzerinde yeni bir çalışma döneminin başladığını ifade eder.

`WORK_PAUSED`, aktif çalışma döneminin duraklatıldığını ifade eder.

Çalışmaya yeniden başlamak için ayrı bir `WORK_RESUMED` event'i kullanılmaz; yeni bir `WORK_STARTED` event'i oluşturulur.

Task değişimini ifade eden ayrı bir `TASK_SWITCHED` event'i kullanılmaz.

Task geçişi, kaynak Task üzerindeki bekleme ve hedef Task üzerindeki yeni çalışma event'leri üzerinden anlaşılabilir.

Task oluşturulması Time Engine event'i olarak zorunlu değildir.

Task'ın oluşturulma zamanı Task domain verisi olarak tutulur.

Session için `SESSION_STARTED` veya `SESSION_ENDED` Time Engine event'leri kullanılmaz.

Time Engine event'leri anlamlı durum geçişlerini temsil eder.

Her UI eyleminin ayrı bir Time Engine event'i olması zorunlu değildir.
---

## REQ-108 — Do not log every second

**Status:** ACCEPTED

The Time Engine does not need a per-second event stream.

Events mark meaningful state transitions; durations are derived from event timestamps.

Event kayıtları minimum fakat geçmiş zaman çizelgesini yeniden oluşturmak için yeterli olmalıdır.

Event'ler gerçekleşmiş durum değişikliklerini temsil eder; her UI eyleminin ayrı bir event olması zorunlu değildir.

Event kayıtları duration bilgisini source of truth olarak taşımaz. Süreler event timeline'ından türetilir.

Time Engine event'leri immutable tarihsel kayıtlar olarak ele alınır.

---

## REQ-109 — Multi-dimensional reporting

**Status:** ACCEPTED

The same historical time record must support reporting by:

- Task
- Project
- Session
- day
- week
- month
- year

and arbitrary date ranges where the report requires it.

---

## REQ-110 — Historical event immutability

**Status:** ACCEPTED

Historical Time Engine events are immutable.

Current Task/Project fields may be edited according to their semantics, but historical timing events must not be rewritten.

---

## REQ-111 — Time Engine retention

**Status:** ACCEPTED

Historical time data is intended for long-term/yearly reporting.

Task deletion must not destroy historical time accounting.

---

## REQ-112 — Task actual time definition

**Status:** ACCEPTED

Task `Gerçekleşen Süre` represents actual work time.

It excludes time classified as:

- waiting,
- interruption,
- break,
- other non-work periods.

---

## REQ-113 — Project time derives from events

**Status:** ACCEPTED

Project actual time is derived from underlying Time Engine work records/events.

The system must avoid double-counting source events when aggregating nested Projects.

---

## REQ-114 — Timer is not the source of truth

**Status:** ACCEPTED

The visible timer must not be treated as the authoritative historical record.

The event history is the source of truth for time accounting.

---

## REQ-115 — Session Concept

**Status:** ACCEPTED

Session, kullanıcı tarafından isteğe bağlı olarak oluşturulan bir çalışma hedefi / countdown katmanıdır.

Session:

- Time Engine'in tarihsel kaynak nesnesi değildir,
- kendi başına çalışma süresi olarak muhasebeleştirilmez,
- herhangi bir Task'ı otomatik olarak başlatmaz,
- kullanıcı tarafından açık bir çalışma eylemi gerçekleştirilmesini gerektirir,
- aynı Session içinde birden fazla Task arasında geçiş yapılmasına izin verebilir.

Örneğin kullanıcı 2 saatlik bir Session oluşturabilir ve arayüzde:

`Session Kalan Süre 01:47:15`

gibi bir countdown görebilir.

Session'ın varlığı herhangi bir Task'ın timer'ını otomatik olarak başlatmaz.

Session geçmişi için ayrı `SESSION_STARTED` veya `SESSION_ENDED` Time Engine event'leri zorunlu değildir.

Bir zaman aralığında kesintisiz çalışmanın ne kadar sürdüğü gibi bilgiler, gerektiğinde Time Engine event timeline'ından türetilir.

---

## REQ-116 — Session and Continuous Work Analysis

**Status:** ACCEPTED

Session'ın kendisi Time Engine'de tarihsel çalışma süresi olarak muhasebeleştirilmez.

Session ile ilgili gösterimler, gerektiğinde kullanıcı hedefi ile gerçek Time Engine olaylarının karşılaştırılmasına dayanabilir.

Örneğin:

- Session hedef süresi,
- Session sırasında gerçekleşen gerçek çalışma süresi,
- tamamlanan Task sayısı,
- Session sırasında gerçekleşen Kesintiler,
- Session sırasında gerçekleşen diğer çalışma durumları

gösterilebilir.

Bir Session hedefinin tamamlanmaması `başarısız` olarak etiketlenmez.

Kesintisiz çalışma süresi gibi analizler Session'ın kendi süresinden değil, Time Engine event timeline'ından türetilir.

Session'ın kendisi Time Engine'in ikinci bir kaynak noktası haline gelmemelidir.

---

## REQ-117 — Waiting is reportable

**Status:** ACCEPTED

Waiting is important reportable activity.

Reports should be able to show:

- what was waited for,
- why,
- duration.

---

## REQ-118 — Interruption reasons are reportable

**Status:** ACCEPTED

Interruptions should record reasons and support later reporting.

---

## REQ-119 — Time Engine semantic separation

**Status:** ACCEPTED DESIGN CONSTRAINT

Task status `Beklemede` and Time Engine event categories such as:

- WAITING
- INTERRUPTION
- BREAK
- Task switching

must not be blindly treated as the same concept.

Their exact relationship is a separate Time Engine design topic.

Implementation must not invent this mapping prematurely.

---

# 15. Interruptions

## REQ-120 — Interruption reasons

**Status:** ACCEPTED

Interruption reasons are first-class reportable information.

Examples discussed include:

- manager,
- staff,
- citizen,
- phone,
- meeting,
- break,
- smoking break,
- unwanted conversation,
- urgent work,
- technical issue,
- other.

---

## REQ-121 — Involuntary interruption reporting

**Status:** ACCEPTED

lapwOS should support reporting the count of involuntary interruptions over arbitrary date ranges.

---

# 16. Reporting

## REQ-122 — Reporting periods

**Status:** ACCEPTED

Reports support:

- daily,
- weekly,
- monthly,
- quarterly,
- half-yearly,
- yearly

and arbitrary date ranges where appropriate.

---

## REQ-123 — Reporting topics

**Status:** ACCEPTED

Reporting should be able to answer questions such as:

- What was done?
- How long did it take?
- Which Project?
- Which Task?
- How much time was spent waiting?
- Why did waiting happen?
- How many interruptions occurred?
- Why did interruptions occur?
- How much did actual time exceed estimates?
- How much correction/rework occurred?
- Which work histories are long-running?

---

## REQ-124 — Deleted Task time reporting

**Status:** ACCEPTED

Time spent on permanently deleted Tasks remains represented historically under:

`Silinmiş Görevlere Harcanan Zaman`

---

## REQ-125 — Productivity score

**Status:** ACCEPTED DESIGN DIRECTION

A productivity score may exist in the future.

The score must be nonjudgmental.

No concrete productivity scoring formula is accepted yet.

The formula must be designed before implementation.

---

# 17. Dashboard and Main Workflow

## REQ-126 — Dashboard core

**Status:** ACCEPTED

The dashboard includes, as core concepts:

- graphs,
- agenda,
- upcoming Tasks,
- overdue Tasks,
- calendar,
- large red timer button,
- `Şimdi bunu yap`,
- approaching deadlines (`Bitiş Tarihi Yaklaşanlar`).

---

## REQ-127 — Aggregate warnings

**Status:** ACCEPTED

The application should prefer aggregate warnings and useful summaries over constant per-Task nagging.

---

## REQ-128 — Wide-screen workflow

**Status:** ACCEPTED

The application is designed primarily for desktop/wide-screen use.

The user may keep it permanently open, including on a second monitor.

---

## REQ-129 — Main Task layout

**Status:** ACCEPTED

The Task work area is conceptually organized as:

- left: Project/sidebar context,
- center: Task work area,
- right: independent Notebook.

The central Task area roughly follows:

1. title
2. timer/time area
3. Task information
4. Steps
5. Notes

Exact visual design remains open.

---

## REQ-130 — Timer detail UI deferred

**Status:** ACCEPTED

Detailed timer interaction/UI is intentionally deferred until the Time Engine and user workflow are further designed.

---

## REQ-131 — Completion summary

**Status:** ACCEPTED

After Task completion, the user may see:

- completion summary,
- work details,
- next-Task recommendations.

Because of the wide-screen context, approximately 10 detailed recommendations may be shown.

---

## REQ-132 — Recommendation navigation

**Status:** ACCEPTED

Clicking a recommended Task navigates to that Task.

It does not automatically start the timer.

The user may ignore recommendations and use the sidebar.

---

# 18. Classification System

## REQ-133 — User-defined classifications

**Status:** ACCEPTED

Users can create classification fields and their options from Settings.

Initial predefined options may be supplied.

---

## REQ-134 — Classifications are optional

**Status:** ACCEPTED

Classification fields are optional.

The user controls whether classification fields are visible/used in the Task creation form.

---

## REQ-135 — Core fields are not classifications

**Status:** ACCEPTED

Core concepts cannot be replaced by classifications.

At minimum, these remain core:

- Task Name
- Priority
- Status
- dates
- estimate
- actual time
- Project relationship

---

## REQ-136 — Classification structure

**Status:** ACCEPTED

Classification structure is:

`Classification Field → Options`

A Task can have at most one selected Option per Classification Field.

An empty selection is allowed.

---

## REQ-137 — Classification purpose

**Status:** ACCEPTED

Classifications support:

- meaning/context,
- filtering,
- grouping,
- reporting.

They do not directly control the Decision Engine.

---

## REQ-138 — Disable instead of delete

**Status:** ACCEPTED

Classification Options can be disabled rather than deleted.

Disabled Options are hidden from normal use but remain available under:

`Devre Dışı Bırakılanlar`

and can be re-enabled.

Classification Fields can also be disabled.

Historical data is preserved.

---

## REQ-139 — Rename without ID change

**Status:** ACCEPTED

Renaming a Classification Field does not change its ID.

Renaming an Option does not change its ID.

Existing/historical Tasks continue to reference the same IDs.

---

## REQ-140 — No automatic retro-classification

**Status:** ACCEPTED

Creating a new Classification does not automatically classify old Tasks.

The system must not guess classifications.

---

## REQ-141 — Classification ordering

**Status:** ACCEPTED

Option order and Field order support drag/drop.

Ordering is visual/organizational only.

---

## REQ-142 — No bulk classification management in MVP

**Status:** ACCEPTED

Bulk classification management is outside MVP.

---

## REQ-143 — Classification field creation

**Status:** ACCEPTED

Creating a Classification Field requires only its name.

Options can be added later.

A newly created Field is active by default.

---

## REQ-144 — Classification placement on Task

**Status:** ACCEPTED

Classifications are secondary Task information.

They should appear under a secondary area such as:

`Diğer Bilgiler`

rather than dominating the main Task workflow.

---

# 19. User Manual

## REQ-145 — Built-in User Manual

**Status:** ACCEPTED

lapwOS includes its own User Manual.

---

## REQ-146 — Manual location

**Status:** ACCEPTED

The User Manual is accessible through:

`Ayarlar → Kullanım Kılavuzu`

A dedicated Help area on the main screen is not required.

---

## REQ-147 — Manual maintenance

**Status:** ACCEPTED

The User Manual must be kept synchronized with the implemented product behavior.

---

## REQ-148 — Permanent deletion warning in manual

**Status:** ACCEPTED

The User Manual must clearly warn that emptying the Recycle Bin permanently deletes data.

The warning must be written in all caps.

The accepted humorous note is:

> “TASARIMCININ BU UYARIYI OLUŞTURACAK KADAR ENERJİSİ YOKTU.”

---

# 20. AI / Development Governance

## REQ-149 — Source of Truth hierarchy

**Status:** ACCEPTED

The project uses this authority hierarchy:

1. Accepted Requirements
2. Architecture Decisions / ADRs
3. AI Manifesto
4. Roadmap
5. Proposed Requirements
6. Ideas
7. Conversation History

---

## REQ-150 — Requirement lifecycle discipline

**Status:** ACCEPTED

Requirements must have an explicit lifecycle:

- IDEA
- PROPOSED
- ACCEPTED
- REJECTED
- DEPRECATED

---

## REQ-151 — Stable requirement IDs

**Status:** ACCEPTED

Accepted requirements use stable IDs.

IDs must not be silently reused for different meanings.

---

## REQ-152 — Ambiguity and contradiction handling

**Status:** ACCEPTED

If requirements conflict or are ambiguous:

- the conflict is identified explicitly,
- implementation does not guess silently,
- the unresolved point is moved to the appropriate proposed/design document,
- the accepted requirement remains unchanged until a new decision is made.

---

## REQ-153 — AI must not pretend implementation occurred

**Status:** ACCEPTED

The AI must not claim that code, files, migrations, tests, commits, or repository changes were completed unless that work was actually performed and verified.

---

## REQ-154 — Documentation is part of the product

**Status:** ACCEPTED

Project documentation is treated as a product artifact and source of truth, not disposable notes.

---

## REQ-155 — Single Active Task Timer
**Status:** ACCEPTED

Aynı anda yalnızca bir Task'ın çalışma timer'ı aktif olabilir.

lapwOS aynı anda iki veya daha fazla Task'ın çalışma timer'ının
aktif olmasına izin vermez.

---

## REQ-156 — Task Transition

**Status:** ACCEPTED

Kullanıcı başka bir Task'a geçmek istediğinde `Geçiş Yap`
eylemini kullanabilir.

Aktif bir Task varsa:

1. Mevcut Task duraklatılır.
2. Hedef Task başlatılır.
3. Hedef Task'ın timer'ı hemen çalışmaya başlar.
4. Bu işlem Kesinti olarak kaydedilmez.

`Geçiş Yap`, kullanıcının açıkça Task değiştirme niyetini
ifade eden kontrollü bir Task-switch işlemidir.

---

## REQ-157 — Task Status ve Timer State Ayrımı

**Status:** ACCEPTED

Task Status ile Timer State birbirinden bağımsız kavramlardır.

- Task Status, işin durumunu ifade eder.
- Timer State, zaman takibinin o anda çalışıp çalışmadığını ifade eder.

Timer'ın duraklatılması Task Status'un otomatik olarak değiştirilmesi anlamına gelmez.

Task'ın `Beklemede` durumunda olması da tek başına Timer'ın nasıl davrandığını belirlemez.

Bu iki kavram yalnızca tanımlanmış iş akışları üzerinden birbirleriyle ilişkilendirilebilir.

---

## REQ-158 — Task Bekleme Nedeni

**Status:** ACCEPTED

Bir Task `Beklemede` durumuna alınırken bekleme nedeni belirtilmesi zorunludur.

Bekleme nedeni, Task'ın neden ilerlemediğini açıklayan ayrı bir bilgidir.

Örnek nedenler:

- Kurumdan cevap bekleniyor
- Yönetici onayı bekleniyor
- Başka bir personelin işlemi bekleniyor
- Bilgi / belge bekleniyor
- Sistem / teknik sorun
- Kullanıcı Seçimi
- Diğer

Bekleme nedeni daha sonra raporlanabilir olmalıdır.

---

## REQ-159 — Dışsal Bekleme Sayacı

**Status:** ACCEPTED

Bir Task dışsal bir nedenle `Beklemede` olduğunda, ilgili bekleme süresi Time Engine tarafından ölçülür.

Dışsal bekleme süresi:

- Task'ın çalışma süresine dahil edilmez.
- Bekleme süresi olarak ayrı hesaplanır.
- Başlangıç ve bitiş olayları üzerinden ölçülür.
- Raporlarda ayrı olarak gösterilebilir.

Örneğin kurumdan cevap bekleme, yönetici onayı bekleme veya bilgi/belge bekleme süreleri dışsal bekleme olarak ölçülür.

---

## REQ-160 — Kullanıcı Seçimi Beklemesi

**Status:** ACCEPTED

`Kullanıcı Seçimi`, Task'ın `Beklemede` durumuna alınabilen özel bir bekleme nedenidir.

Bekleme nedeni `Kullanıcı Seçimi` olduğunda:

- Task `Beklemede` durumundadır.
- Task'ın çalışma timer'ı çalışmaz.
- Bu süre dışsal bekleme süresine dahil edilmez.
- Kullanıcının kendi tercihiyle beklettiği süre ayrı olarak tarihsel olarak korunur.

`Kullanıcı Seçimi` nedeniyle geçen süre veri olarak kaybolmaz; ancak dışsal bekleme süresi hesabına dahil edilmez.

---

## REQ-161 — Bekleme Sürelerinin Ayrıştırılması

**Status:** ACCEPTED

Task'ın `Beklemede` olduğu sürelerin tamamı aynı tür süre olarak kabul edilmez.

En azından şu ayrım korunmalıdır:

- Dışsal bekleme süresi
- `Kullanıcı Seçimi` süresi

Örneğin:

10:00 — Kurumdan cevap bekleme başladı
12:00 — Kurumdan cevap geldi
12:00 — Kullanıcı Seçimi ile bekletildi
15:00 — Task yeniden çalışmaya hazır

Raporlama:

- Dışsal bekleme: 2 saat
- Kullanıcı Seçimi: 3 saat

Bu ayrım tarihsel olarak korunmalıdır.

---

## REQ-162 — Beklemenin Sonlandırılması ve Çalışmanın Yeniden Başlatılması

**Status:** ACCEPTED

Bir Task'ın bekleme durumunun sona ermesi, Task'ın çalışma timer'ının otomatik olarak başlaması anlamına gelmez.

Bekleme sona erdirildiğinde:

- Bekleme süresi Time Engine tarafından sonlandırılmış olarak loglanır.
- Bekleme kaydı tarihsel olarak korunur.
- Task'ın çalışma timer'ı otomatik olarak başlamaz.
- Kullanıcı çalışmaya devam etmek istediğinde `Başlat / Devam Et` eylemini açıkça kullanır.

Bu sayede "beklemenin sona ermesi" ile "çalışmanın başlaması" birbirinden ayrı olaylar olarak izlenir.

---

## REQ-163 — Bekleme Sebebinin Değiştirilmesi

**Status:** ACCEPTED

Bir Task beklemedeyken bekleme sebebi değişebilir.

Bekleme sebebi değiştirildiğinde mevcut bekleme kaydı değiştirilmez.

İşleyiş:

1. Mevcut bekleme kaydı sonlandırılır.
2. Mevcut bekleme kaydının başlangıç ve bitiş zamanı korunur.
3. Yeni bekleme sebebi ile yeni bir bekleme kaydı oluşturulur.
4. Yeni bekleme kaydının başlangıç zamanı, önceki beklemenin sona erdiği zamandır.

Örnek:

`text
10:00 → 14:30
Kurumdan cevap bekleniyor

14:30 → 16:00
Yönetici onayı bekleniyor

---

## REQ-164 — Beklemede Durumunda Timer Başlatma Kısıtı

**Status:** ACCEPTED

Task Status ile Timer State birbirinden bağımsız kavramlar olmakla birlikte, veri bütünlüğünü korumak amacıyla bazı durum kombinasyonları sistem tarafından kısıtlanır.

Bir Task'ın Status'u `Beklemede` ise çalışma timer'ı başlatılamaz.

Kullanıcının Task üzerinde yeniden çalışmaya başlayabilmesi için öncelikle:

1. Task Status'u `Beklemede` durumundan `Devam Ediyor` durumuna alınır.
2. Ardından `Başlat / Devam Et` eylemi kullanılabilir.
3. Çalışma timer'ı bu eylemden sonra başlar.

Bekleme sona erdiğinde Task'ın çalışma timer'ı otomatik olarak başlamaz.

Normal `Duraklat / Beklet` durumunda ise Task Status'u `Devam Ediyor` olarak kalabilir ve kullanıcı doğrudan `Başlat / Devam Et` ile timer'ı yeniden çalıştırabilir.

Bu kısıtlama, Task Status ile Timer State arasındaki kavramsal ayrımı korurken veri bütünlüğünü sağlamak amacıyla uygulanır.

Temel prensip:

> Kullanıcıya esneklik, veriye disiplin.

---
## REQ-165 — Kesintinin Sonlandırılması

**Status:** ACCEPTED

Bir Task `Kesinti` durumundayken aynı Task üzerinde çalışmaya devam edilemez.

Kullanıcının aynı Task üzerinde çalışmaya devam edebilmesi için önce mevcut kesintinin sona erdirilmesi gerekir.

Kesinti sona erdirildiğinde:

- Kesinti kaydı Time Engine tarafından sonlandırılır.
- Kesintinin başlangıç ve bitiş zamanı tarihsel olarak korunur.
- Task'ın çalışma timer'ı kesintinin sona ermesiyle otomatik olarak başlamaz.
- Kullanıcı çalışmaya devam etmek istediğinde `Başlat / Devam Et` eylemini kullanır.

Kesinti devam ederken başka bir Task'a geçmek için `Geçiş Yap` kullanılabilir.

`Geçiş Yap` kullanıldığında mevcut Task'ın kesintisi sonlandırılır ve Task Transition kuralları uygulanır.
---

## REQ-166 — Task Transition Davranışı

**Status:** ACCEPTED

`Geçiş Yap`, kullanıcının aktif olarak çalıştığı Task'tan başka bir Task'a geçmesini sağlayan kontrollü bir eylemdir.

Kullanıcı `Geçiş Yap` ile başka bir Task'a geçtiğinde:

1. Mevcut Task'ın aktif çalışma timer'ı durdurulur.
2. Mevcut Task `Beklemede` durumuna alınır.
3. Mevcut Task için yeni bir bekleme kaydı oluşturulur.
4. Bekleme nedeni sistem tarafından `Başka göreve geçiş yapıldı` olarak belirlenir.
5. Hedef Task'ın Status'u `Devam Ediyor` olur.
6. Hedef Task'ın çalışma timer'ı hemen başlar.
7. Bu işlem ayrıca `Kesinti` olarak kaydedilmez.

Task Transition, mevcut Task'ın çalışmasının sonlandırılması değil, başka bir Task üzerinde çalışmaya geçilmesi olarak değerlendirilir.

---
## REQ-167 — Kesinti Sırasında Task'a Devam Etme Kısıtı

**Status:** ACCEPTED

Bir Task `Kesinti` durumundayken kullanıcı aynı Task üzerinde doğrudan çalışmaya devam edemez.

Kesintinin sona ermesi için öncelikle kesinti sonlandırılmalıdır.

Kesinti devam ederken:

- `Başlat / Devam Et` ile aynı Task'ın çalışma timer'ı başlatılamaz.
- Yeni bir `Kesinti` başlatılamaz.
- `Duraklat / Beklet` işlemi yapılamaz.
- Kullanıcı `Geçiş Yap` ile başka bir Task'a geçebilir.

`Geçiş Yap` kullanıldığında mevcut Task için kesinti kaydı sonlandırılır ve Task `Beklemede` durumuna alınır.

Bekleme nedeni:

`Başka göreve geçiş yapıldı`

olarak kaydedilir.
---
## REQ-168 — Task Görüntüleme ve Aktif Task'a Geçiş Ayrımı

**Status:** ACCEPTED

Task'ı görüntülemek ile Task üzerinde çalışmaya başlamak birbirinden farklı eylemlerdir.

Bir Task'ın detaylarının görüntülenmesi:

- Mevcut aktif Task'ın timer'ını değiştirmez.
- Görüntülenen Task'ın timer'ını başlatmaz.
- Herhangi bir Task Transition oluşturmaz.

Kullanıcının başka bir Task üzerinde çalışmaya başlaması için açıkça `Geçiş Yap` eylemini kullanması gerekir.

`Geçiş Yap` kullanıldığında:

- Mevcut aktif Task, Task Transition kurallarına göre `Beklemede` durumuna alınır.
- Hedef Task aktif hale gelir.
- Hedef Task'ın çalışma timer'ı başlar.
- Aynı anda yalnızca bir Task'ın çalışma timer'ı aktif olabilir.

Task'a dönüş veya Task üzerinde çalışmaya başlama işlemi otomatik olarak gerçekleşmez.

Bu ayrım, kullanıcının Task'lar arasında gezinirken yanlışlıkla çalışma timer'ı başlatmasını önlemek ve bilinçli Task seçimini desteklemek amacıyla uygulanır.

Temel prensip:

> Task'ı görmek, Task'a geçmek anlamına gelmez.
---
## REQ-169 — Duraklatılmış Sürenin Muhasebeleştirilmemesi

**Status:** ACCEPTED

`Duraklat / Beklet` ile geçen süre, ayrı bir zaman kategorisi olarak toplamlanmaz ve çalışma dışı süre muhasebesine dahil edilmez.

Time Engine'in temel ölçümü Task üzerinde gerçekleşen fiili çalışma süresidir.

Bu nedenle:

- Duraklatılmış süre için ayrı bir toplam süre oluşturulmaz.
- Duraklatılmış süre çalışma süresine dahil edilmez.
- Mesai bitimi, programın kapatılması veya uzun süreli kullanıcı yokluğu gibi durumlarda oluşan süreler `Duraklatılmış Süre` olarak raporlanmaz.
- Kullanıcının kendi tercihiyle çalışmadığı süreler, anlamlı bir olay olarak ayrıca sınıflandırılmadığı sürece zorunlu bir zaman kategorisine dönüştürülmez.

`Kesinti` ve `Bekleme` gibi anlamlı olaylar ise kendi kabul edilmiş kuralları kapsamında tarihsel olarak takip edilir.

Temel prensip:

> lapwOS kullanıcının bütün gününü muhasebeleştirmeye çalışmaz; yaptığı işi ve iş yapmasını etkileyen anlamlı olayları takip eder.

Ana ölçüm:

> **Bu Task üzerinde gerçekten ne kadar çalışıldı?**
---
## REQ-170 — Mola ve Kesinti Raporlama Ayrımı

**Status:** ACCEPTED

`Mola`, Time Engine içerisinde ayrı bir zaman muhasebesi türü olarak modellenmez.

Mola, `Kesinti` kayıtları içerisinde bir kesinti nedeni olarak tutulur.

Time Engine:

- Kesintinin gerçekleştiğini,
- Kesintinin başlangıç ve bitiş zamanını,
- Kesinti nedenini

tarihsel olarak kaydeder.

Mola süresinin toplam kesinti içerisindeki payı ve molalar hariç süre gibi türetilmiş göstergeler Raporlama katmanında hesaplanır.

Örneğin raporlama katmanı:

- Toplam Kesinti
- Mola Süresi
- Dışsal Sebeplerle Yaşanan Kesintiler
- Molalar Hariç Süre

gibi göstergeler oluşturabilir.

Bu hesaplamalar Time Engine'in temel olay modelini değiştirmez.

Temel prensip:

> Time Engine gerçekleşen olayları kaydeder; raporlama katmanı bu olaylardan anlamlı göstergeler üretir.
---
## REQ-171 — Kesinti Nedeni Tanımları

**Status:** ACCEPTED

Kesinti kayıtları bir veya daha fazla Kesinti Nedeni ile ilişkilendirilebilir.

Sistem başlangıçta aşağıdaki Kesinti Nedenlerini sağlar:

- Yönetici
- Personel
- Okullar
- Vatandaş
- Telefon
- Mola
- Acil İş
- Teknik Problem
- İstenmeyen Sohbet
- Toplantı
- Diğer

Kullanıcı yeni Kesinti Nedeni oluşturabilir.

Kesinti Nedenleri silinemez; pasifleştirilebilir.

Pasifleştirilen nedenler yeni Kesinti kayıtlarında seçilemez ancak geçmiş kayıtlarla olan ilişkileri korunur.

Kesinti Nedenlerinin görünen adı değiştirilebilir.

Her Kesinti Nedeni değişmez bir ID ile tanımlanır. Ad değişikliği geçmiş Kesinti kayıtlarının referansını değiştirmez.
---
## REQ-172 — Kesinti Nedeninin Girilme Zamanı

**Status:** ACCEPTED

Kesinti başlatılırken Kesinti Nedeni girilmesi zorunlu değildir.

Kesinti Nedeni, Kesinti sonlandırma sürecinde girilebilir.

Kullanıcı `Kesinti` eylemini kullandığında Kesinti hemen başlar ve Time Engine tarafından takip edilir.

Kullanıcı Kesintiyi sonlandırmak istediğinde neden seçimi ekranına geçer.

Kesinti, neden seçimi ve kayıt işlemi tamamlanana kadar devam ediyor kabul edilir.
---
## REQ-173 — Kesinti Nedeni Zorunluluğu

**Status:** ACCEPTED

Bir Kesinti kaydının tamamlanabilmesi için en az bir Kesinti Nedeni seçilmelidir.

Kesinti Nedeni seçimi tamamlanmadan Kesinti sonlandırılamaz.

Kesinti Nedeni için açıklama alanı bulunur ancak açıklama zorunlu değildir.
---
## REQ-174 — Çoklu Kesinti Nedeni

**Status:** ACCEPTED

Bir Kesinti birden fazla Kesinti Nedeni ile ilişkilendirilebilir.

Birden fazla neden seçildiğinde sistem başlangıçta tüm nedenlere eşit ağırlık verir.

Tek bir neden seçildiğinde ağırlıklandırma arayüzü gösterilmez.
---
## REQ-175 — Kesinti Nedeni Ağırlığı

**Status:** ACCEPTED

Bir Kesinti birden fazla Kesinti Nedeni içeriyorsa her neden için 1–5 arasında bir ağırlık belirlenebilir.

Varsayılan ağırlık tüm seçili nedenler için eşittir.

Ağırlıkların toplamının belirli bir değere, örneğin %100'e, eşit olması gerekmez.

Raporlama katmanı Kesinti süresini nedenlerin ağırlıklarına göre dağıtır.

Örneğin:

- Yönetici: 5
- Telefon: 4
- Mola: 3

ise toplam ağırlık 12 kabul edilir ve Kesinti süresi:

- Yönetici: `5 / 12`
- Telefon: `4 / 12`
- Mola: `3 / 12`

oranında dağıtılır.

Ağırlık sistemi Time Engine'in temel olay modelini değiştirmez; süre dağılımı raporlama katmanında hesaplanır.
---
## REQ-176 — Kesinti Sonlandırma Süreci

**Status:** ACCEPTED

`Kesintiyi Bitir` eylemi Kesintiyi anında sonlandırmaz.

Bu eylem Kesintinin sonlandırılması için gerekli veri giriş sürecini başlatır.

Kullanıcı:

1. Bir veya daha fazla Kesinti Nedeni seçer.
2. Gerekirse açıklama girer.
3. Birden fazla neden varsa ağırlıkları düzenler.
4. `Kaydet` eylemini kullanır.

Kesinti ancak `Kaydet` işlemi başarıyla tamamlandığında sonlandırılmış kabul edilir.

Neden girişi ekranında kullanıcıya `Vazgeç` seçeneği sunulmaz.

Kullanıcı `Kaydet` işlemini tamamlayana kadar Kesinti süresi devam eder.
---
## REQ-177 — Kesinti Sırasında İşlem Kısıtları

**Status:** ACCEPTED

Aktif bir Kesinti devam ederken kullanıcı normal çalışma akışına dönemez.

Kesinti tamamlanmadan:

- başka Task'a geçilemez,
- başka Task görüntülenemez,
- aynı Task üzerinde çalışmaya başlanamaz,
- yeni Kesinti başlatılamaz,
- `Duraklat / Beklet` kullanılamaz.

Kesinti önce tamamlanmalı, ardından normal Task çalışma akışına dönülmelidir.
---
## REQ-178 — Kesinti Sırasında Uygulama Kapatma

## REQ-178 — Kesinti Sırasında Uygulama Kapatma

**Status:** ACCEPTED

Aktif bir Kesinti sırasında uygulamanın normal kullanıcı arayüzü üzerinden kapatılması engellenir.

İşletim sistemi tarafından zorla sonlandırma, uygulama çökmesi veya benzeri beklenmeyen kapanma durumlarında aktif Kesinti otomatik olarak sonlandırılmaz.

Uygulama yeniden açıldığında aktif Kesinti recovery durumu olarak ele alınır.

Kullanıcı Kesintiyi recovery sonrasında tamamlamadan:

- Task değiştiremez,
- başka Task görüntüleyemez,
- çalışma başlatamaz,
- yeni Kesinti başlatamaz,
- Duraklatma yapamaz,
- diğer çalışma eylemlerini gerçekleştiremez.

Kullanıcı Kesintinin gerçek bitiş zamanını biliyorsa bu zamanı girebilir.

Gerçek bitiş zamanı bilinmiyorsa uygulamanın yeniden açıldığı zaman Kesintinin bitiş zamanı olarak kullanılabilir.

Kullanıcı tarafından Kesinti Nedeni girilememişse:

- Kesinti Nedeni: `Diğer`
- Kaynak: `Sistem Fallback`

olarak kaydedilebilir.

Sistem Fallback ile oluşturulan kayıtlar, kullanıcının normal akışta `Diğer` seçtiği Kesinti kayıtlarından ayırt edilebilir.

Recovery sonrasında Kesintinin tamamlanması Task'ın çalışma timer'ını otomatik olarak başlatmaz.

Kullanıcı çalışmaya devam etmek istediğinde açık bir çalışma eylemi gerçekleştirmelidir.
---
## REQ-179 — Duraklatılmış Task'ın Status'u

**Status:** ACCEPTED

`Duraklat / Beklet` eylemi kullanıldığında Task Status'u `Devam Ediyor` olarak kalır.

Duraklatma, Task'ın Status'unu `Beklemede` durumuna otomatik olarak değiştirmez.

Duraklatılmış Task'ın çalışma timer'ı aktif değildir.

---
## REQ-180 — Duraklatılmış Task'tan Geçiş

**Status:** ACCEPTED

Duraklatılmış bir Task'tan başka bir Task'a `Geçiş Yap` ile geçilebilir.

`Geçiş Yap` açık kullanıcı niyetini ifade eden bir eylemdir.

Duraklatılmış bir Task'ın görüntülenmesi tek başına geçiş oluşturmaz ve timer başlatmaz.

Kullanıcı hedef Task üzerinde çalışmaya başlamak için açıkça `Geçiş Yap` eylemini kullanmalıdır.
---
## REQ-181 — Uygulama Açılışında Dashboard

**Status:** ACCEPTED

lapwOS her uygulama açılışında Dashboard ile başlar.

Uygulamanın açılması herhangi bir Task'ın çalışma timer'ını otomatik olarak başlatmaz.

Dashboard üzerinde duraklatılmış Task'lar ayrı bir bölümde gösterilebilir.

Duraklatılmış bir Task'ın Dashboard'da görüntülenmesi timer'ı başlatmaz.

Kullanıcı çalışmaya başlamak için açıkça ilgili çalışma eylemini kullanmalıdır.
---
## REQ-182 — Task Görüntüleme ve Açık Çalışma Eylemi

**Status:** ACCEPTED

Dashboard üzerinde bir Task'ın görüntülenmesi Task'ın çalışma timer'ını başlatmaz.

Dashboard'daki bir Task'a veya Decision Engine önerisine tıklanması yalnızca ilgili Task'ın görüntülenmesini sağlar.

Task'ın çalışma timer'ı yalnızca açık bir kullanıcı çalışma eylemi sonucunda başlatılabilir:

- Aktif bir Task yoksa `Başlat` eylemi ile,
- Başka bir Task aktifse hedef Task üzerindeki `Geçiş Yap` eylemi ile.

Task görüntüleme, öneriye tıklama, Dashboard navigasyonu veya başka bir pasif kullanıcı etkileşimi Task'ın timer'ını otomatik olarak başlatamaz.

`Başlat` ve `Geçiş Yap` açık kullanıcı niyetini ifade eden çalışma eylemleridir.
---
## REQ-183 — Task Durumuna Göre Çalışma Eylemleri

**Status:** ACCEPTED

Task ekranında gösterilen çalışma eylemleri, Task Status ve Timer State'e göre belirlenir.

### Başlanmadı

Aşağıdaki eylemler kullanılabilir:

- `Başlat`
- `Tamamla`

### Devam Ediyor + Timer çalışıyor

Aşağıdaki eylemler kullanılabilir:

- `Duraklat / Beklet`
- `Kesinti`
- `Tamamla`

### Devam Ediyor + Timer duraklatılmış

Aşağıdaki eylemler kullanılabilir:

- `Devam Et`
- `Tamamla`

### Beklemede

Aşağıdaki çalışma eylemi kullanılabilir:

- `Devam Et`

Beklemede olan Task'ın timer'ı otomatik olarak başlatılamaz.

### Başka bir Task aktifken görüntülenen Task

Hedef Task üzerinde aşağıdaki eylem kullanılabilir:

- `Geçiş Yap`

`Geçiş Yap`, açık kullanıcı niyetini ifade eder ve hedef Task'ın çalışma timer'ını başlatır.

### Tamamlandı

Aşağıdaki eylem kullanılabilir:

- `Yeniden Aç`

Tamamlanmış Task üzerinde normal çalışma eylemleri gösterilmez.

### Aktif Kesinti

Aktif bir Kesinti sırasında yalnızca:

- `Kesintiyi Bitir`

eylemi kullanılabilir.

Kesinti tamamlanmadan Task değiştirme, başka Task görüntüleme, yeni Kesinti başlatma veya Duraklatma gibi diğer çalışma eylemleri kullanılamaz.

`Başlat` ve `Devam Et` kullanıcı arayüzünde farklı metinler olarak gösterilebilir. Her ikisi de Time Engine açısından açık kullanıcı niyetiyle yeni bir çalışma döneminin başlatılmasını ifade eder.
---
## REQ-184 — Timer State Modeli

**Status:** ACCEPTED

lapwOS Timer State modeli üç temel durumdan oluşur:

- `IDLE`
- `RUNNING`
- `PAUSED`

### IDLE

Çalışma timer'ının aktif olmadığı durumu ifade eder.

`IDLE`, Task'ın neden çalışmadığını tek başına açıklamaz.

Task aşağıdaki durumlardan herhangi birindeyken Timer State `IDLE` olabilir:

- Henüz başlanmamış Task
- Beklemede olan Task
- Kesinti sırasında olan Task
- Çalışması sonlandırılmış Task
- Tamamlanmış Task

### RUNNING

Task'ın çalışma timer'ının aktif olduğu ve çalışma süresinin biriktiği durumu ifade eder.

Aynı anda yalnızca bir Task `RUNNING` Timer State'inde olabilir.

### PAUSED

Task'ın çalışma timer'ının kullanıcı tarafından `Duraklat / Beklet` eylemiyle geçici olarak durdurulduğu durumu ifade eder.

`PAUSED` durumunda çalışma süresi birikmez.

Duraklatma Task Status'unu otomatik olarak değiştirmez.

Timer State ile Task Status birbirinden bağımsız kavramlardır.
---
## REQ-185 — Kesinti ve Beklemenin Timer State'den Ayrılması

**Status:** ACCEPTED

Kesinti ve Bekleme, Timer State olarak modellenmez.

Kesinti ve Bekleme kendi yaşam döngülerine ve zaman bilgilerine sahip ayrı kavramlardır.

Timer State yalnızca çalışma timer'ının mevcut durumunu ifade eder:

- `IDLE`
- `RUNNING`
- `PAUSED`

### Kesinti

Kesinti sırasında Task'ın çalışma timer'ı aktif değildir.

Kesinti için ayrıca başlangıç ve bitiş zamanları ile kesintinin nedenleri ve ilgili diğer bilgiler saklanır.

Kesinti süresi Time Engine ve raporlama tarafından ayrı olarak hesaplanabilir ve nedenlerine göre filtrelenebilir.

Kesinti sırasında Timer State `IDLE` olabilir; `INTERRUPTED` ayrı bir Timer State değildir.

### Bekleme

Bekleme sırasında Task'ın çalışma timer'ı aktif değildir.

Bekleme nedeni ve bekleme süresi ayrı olarak takip edilir.

Bekleme için ayrı bir Timer State oluşturulmaz.

Timer State'in `IDLE` olması, Task'ın neden çalışmadığını tek başına ifade etmez.

Task'ın çalışmama nedeni ilgili Task Status, Bekleme veya Kesinti bilgileri üzerinden belirlenir.
---
## REQ-186 — Kesinti Sürelerinin Nedenlere Göre Raporlanması

**Status:** ACCEPTED

Kesinti süreleri Time Engine tarafından ayrı bir zaman aralığı olarak izlenebilir ve raporlama katmanında nedenlerine göre filtrelenebilir.

Bir Kesinti birden fazla nedene sahip olabilir.

Birden fazla neden seçildiğinde her neden için ağırlık değeri kullanılabilir.

- Varsayılan ağırlıklar eşit olabilir.
- Kullanıcı ağırlıkları değiştirebilir.
- Ağırlıkların toplamının 100 olması zorunlu değildir.
- Her nedenin süre payı, toplam ağırlığa oranlanarak hesaplanır.

Örneğin 60 dakikalık bir Kesinti için:

- Telefon: 5
- Personel: 3
- Toplantı: 2

ağırlıkları verilirse toplam ağırlık 10 olur ve raporlama süresi:

- Telefon: 30 dakika
- Personel: 18 dakika
- Toplantı: 12 dakika

olarak dağıtılabilir.

Kesinti nedenleri ve süre dağılımları raporlama amacıyla kullanılabilir.

Timer State modeli, Kesinti nedenlerinin ve sürelerinin raporlanabilmesini engellememelidir.
---
## REQ-187 — Recovery State Mekanizması

**Status:** ACCEPTED

lapwOS, uygulamanın beklenmeyen şekilde kapanması durumunda son bilinen uygulama durumunu tespit edebilmek için ayrı bir Recovery State mekanizmasına sahip olmalıdır.

Recovery State, Time Engine'in kaynağı değildir.

Recovery State yalnızca uygulamanın son bilinen çalışma durumunun yeniden oluşturulmasına yardımcı olan bir snapshot mekanizmasıdır.

Recovery State aşağıdaki gibi temel bilgiler içerebilir:

- Snapshot zamanı
- Uygulamanın çalışma durumu
- Aktif Work/Task kimliği
- Timer State
- Aktif Kesinti bilgisi
- Aktif Session bilgisi
- Son güvenilir Time Engine event kimliği

Recovery State geçmiş kayıtların üzerine yazılarak güncellenir.

Recovery State geçmiş snapshot'ları append ederek biriktirmez.

Bu nedenle Recovery State dosyasının boyutu zaman içinde snapshot geçmişi nedeniyle sürekli büyümemelidir.

Recovery State ile Time Engine birbirinden bağımsız sorumluluklara sahiptir:

- Time Engine → gerçek zamanlı çalışma tarihçesinin kaynağı
- Recovery State → beklenmeyen kapanma sonrası son bilinen uygulama durumunun kaynağı
---
## REQ-188 — Recovery Snapshot Periyodu

**Status:** ACCEPTED

Recovery State snapshot'ı varsayılan olarak **1 dakikalık periyotlarla** güncellenmelidir.

Snapshot mekanizmasının amacı uygulamanın son bilinen durumunu mümkün olduğunca güncel tutmaktır.

1 dakikalık snapshot periyodu, 10 dakikalık periyoda göre beklenmeyen kapanma sonrasında belirsiz kalan zaman aralığını azaltır.

Recovery Snapshot periyodu, Time Engine event kayıtlarının yerine geçmez.

Önemli çalışma durumu değişiklikleri Time Engine tarafından event olarak kendi yaşam döngüsü içinde ayrıca kaydedilir.

Recovery Snapshot'ın periyodik olarak yazılması, Time Engine event'lerinin oluşturulma ve kaydedilme mantığını değiştirmez.
---
## REQ-189 — Recovery State ve Time Engine Ayrımı

**Status:** ACCEPTED

Recovery State, Time Engine'in ikinci bir kaynağı olarak kullanılmamalıdır.

Time Engine'in kaynağı immutable Time Event kayıtlarıdır.

Recovery State yalnızca son bilinen uygulama durumunu temsil eder.

Recovery State üzerinden geçmişte gerçekleştiği kesin olarak doğrulanamayan yeni bir çalışma, bekleme veya kesinti süresi otomatik olarak oluşturulamaz.

Örneğin uygulama `RUNNING` durumundayken beklenmeyen şekilde kapanırsa, son Recovery Snapshot ile uygulamanın yeniden açılması arasındaki zaman otomatik olarak çalışma süresine eklenmemelidir.

Recovery mekanizması bilinmeyen zaman aralıklarını tahmin ederek Time Engine tarihçesine yeni süreler yazamaz.

Bu kural, lapwOS'un geçmişte gerçekleştiği kesin olarak bilinmeyen zamanı otomatik olarak uydurmamasını sağlar.
---
## REQ-190 — Recovery State ile Beklenmeyen Kapanma Tespiti

**Status:** ACCEPTED

lapwOS, uygulamanın normal şekilde kapatılıp kapatılmadığını tespit edebilmek için Recovery State içinde uygulama yaşam döngüsünü belirten bir durum bilgisi tutmalıdır.

Uygulama çalışırken Recovery State normal çalışma durumunu temsil etmelidir.

Normal uygulama kapanışı sırasında Recovery State güvenli kapanış durumuna geçirilmelidir.

Uygulama yeniden açıldığında önceki Recovery State güvenli kapanış durumunda değilse beklenmeyen kapanma olasılığı tespit edilmelidir.

Beklenmeyen kapanma tespiti ile kullanıcı müdahalesi gerektiren recovery durumu aynı kavram değildir.

Örneğin aktif bir çalışma timer'ı yoksa beklenmeyen kapanma tespit edilmiş olsa bile kullanıcıya ayrıca recovery ekranı gösterilmesi zorunlu değildir.
---
## REQ-191 — Time Event Timestamp Hassasiyeti

**Status:** ACCEPTED

Time Engine event timestamp'leri dakika ile sınırlandırılmamalıdır.

Event kayıtları mümkün olduğunca hassas bir zaman bilgisiyle saklanmalıdır.

Örneğin:

- `WORK_STARTED` → 10:23:41
- `WORK_PAUSED` → 11:07:18

gibi zaman bilgileri korunabilir.

Çalışma süresi ve diğer zaman aralıkları event timestamp'lerinden hesaplanır.

Raporlama katmanı bu süreleri kullanıcıya dakika bazında gösterebilir.

Dolayısıyla:

**Veri saklama hassasiyeti ≠ raporlama gösterim hassasiyeti**

olmalıdır.

Bu yaklaşım gelecekte daha hassas raporlama ihtiyacının ortaya çıkması halinde geçmiş zaman bilgisinin kaybedilmesini önler.
---
## REQ-192 — Recovery Snapshot İçeriği

**Status:** ACCEPTED

Recovery Snapshot, recovery amacı için gerekli minimum uygulama durumunu içermelidir.

Temel olarak aşağıdaki bilgiler snapshot içinde tutulabilir:

- `schema_version`
- `saved_at`
- uygulama çalışma durumu
- aktif Work/Task kimliği
- Timer State
- aktif Kesinti durumu
- Session durumu
- `last_event_id`
- `last_event_timestamp`

Recovery Snapshot'a geçmiş Time Engine event'lerinin tamamı tekrar yazılmamalıdır.

Recovery Snapshot geçmiş event tarihçesinin kopyası değildir.

`last_event_id` ve `last_event_timestamp`, Recovery State ile Time Engine arasındaki son bilinen noktayı ilişkilendirmek için kullanılabilir.
---
## REQ-193 — Sleep / Hibernate Sırasında Çalışma Timer'ının Durumu

**Status:** ACCEPTED

İşletim sistemi tarafından Sleep veya Hibernate durumuna geçiş güvenilir şekilde tespit edilebildiğinde, aktif çalışma timer'ı otomatik olarak durdurulmalıdır.

Sleep veya Hibernate öncesinde aktif bir Task'ın Timer State'i:

`RUNNING → IDLE`

olmalıdır.

Bu sistem kaynaklı durdurma, Time Engine üzerinde `WORK_PAUSED` event'i ile kaydedilmelidir.

Sistem kaynaklı durdurma, kullanıcı tarafından gerçekleştirilen `Duraklat / Beklet` eyleminden ayırt edilebilir olmalıdır.

Sleep veya Hibernate süresince geçen zaman çalışma süresine dahil edilmemelidir.

Bilgisayar Sleep veya Hibernate durumundan döndüğünde Task'ın çalışma timer'ı otomatik olarak yeniden başlatılamaz.

Wake sonrasında Task `IDLE` durumda kalır.

Kullanıcı çalışmaya devam etmek istediğinde açık bir `Devam Et` eylemi gerçekleştirmelidir.

`Devam Et` eylemi yeni bir `WORK_STARTED` event'i oluşturur.

Bu davranış, bilgisayarın kullanıcı müdahalesi olmadan çalışma timer'ını yeniden başlatmasını engeller.
---
## REQ-194 — Aktif Kesinti Recovery Davranışı

**Status:** ACCEPTED

Uygulama beklenmeyen şekilde kapandığında aktif bir Kesinti bulunuyorsa, Kesinti otomatik olarak sonlandırılamaz.

Recovery sonrasında aktif Kesinti tamamlanmadan aşağıdaki çalışma eylemleri kullanılamaz:

- Task değiştirme
- Başka Task görüntüleme
- Çalışma başlatma
- Yeni Kesinti başlatma
- Duraklatma
- Diğer çalışma eylemleri

Öncelikle mevcut Kesinti sonlandırılmalıdır.

Recovery ekranında kullanıcıdan Kesintinin bitiş zamanını belirlemesi istenir.

Kullanıcı:

- Kesintinin gerçek bitiş zamanını seçebilir.
- Gerçek bitiş zamanını bilmiyorsa uygulamanın yeniden açıldığı zamanı bitiş zamanı olarak seçebilir.

Kesinti sonlandırıldıktan sonra normal Task çalışma akışına dönülür.

Kesintinin recovery sonrasında sonlandırılması Task'ın çalışma timer'ını otomatik olarak başlatmaz.

Kullanıcı çalışmaya devam etmek istediğinde açık bir çalışma eylemi gerçekleştirmelidir.

Recovery mekanizması Kesintinin gerçek bitiş zamanını kullanıcı adına tahmin edemez.
---
## REQ-195 — Event Gerçekleşme ve Kayıt Zamanlarının Ayrılması

**Status:** ACCEPTED

Time Engine event'lerinde olayın gerçekleştiği zaman ile olayın sisteme kaydedildiği zaman birbirinden ayrılabilir.

Event modeli aşağıdaki temel zaman bilgilerini içerebilir:

- `occurred_at` — olayın gerçekleştiği zaman
- `recorded_at` — olayın sisteme kaydedildiği zaman

Normal çalışma sırasında bu iki zaman değeri genellikle birbirine çok yakın veya aynı olabilir.

Recovery, gecikmiş kayıt veya benzeri durumlarda birbirlerinden farklı olabilir.

Örneğin bir Kesinti gerçekten 10:47'de sona ermiş ancak uygulama bunu recovery sırasında 11:10'da kaydetmişse:

- `occurred_at = 10:47`
- `recorded_at = 11:10`

olabilir.

`occurred_at`, Time Engine zaman çizelgesinin oluşturulmasında esas alınan olay zamanıdır.

`recorded_at`, olayın sisteme ne zaman kaydedildiğini gösterir ve tarihçenin kayıt sürecini takip etmek amacıyla kullanılabilir.

Event'lerin immutable olması ilkesi korunur.
---



# 21. Explicitly Unresolved / Separate Design Topics

These are **not missing requirements**. They are intentionally left open and must not be invented by implementation.

### TOPIC-001 — Timer stop/pause semantics

Exact mapping among:

- Task `Beklemede`
- WAITING
- INTERRUPTION
- BREAK
- Task switching
- other Time Engine states

must be designed separately.

### TOPIC-002 — Exact Decision Engine scoring formula

Signals are accepted, but the exact mathematical model is not.

### TOPIC-003 — Waiting reason taxonomy

Waiting is accepted as reportable, but the final standardized reason taxonomy is not yet fixed.

### TOPIC-004 — Interruption category tree

Interruption reasons are accepted.

A proposed category tree exists, but it is not an accepted final taxonomy.

### TOPIC-005 — Multi-Project time attribution

Task membership in multiple Projects is accepted.

The precise accounting model for attribution when a Task belongs to multiple independent Projects remains a separate design topic.

### TOPIC-006 — Productivity score formula

The existence/direction is accepted as future design, but no formula is accepted.

### TOPIC-007 — Exact timer UI

Timer interaction and detailed presentation remain open.

---

# 22. Future Scope — Explicitly Not MVP

The following are intentionally not MVP requirements:

- multi-user web application,
- manager/employee roles,
- manager Task assignment,
- permissions/access control,
- manager workload views,
- manager reporting,
- user-created procedural manuals / personal knowledge base,
- Project-level dependencies,
- bulk classification management,
- automatic note-to-Task inference,
- concrete productivity scoring formula.

These belong in `IDEA.md` or future roadmap material unless separately accepted later.

---

# 23. Final Rule

**If it is not in ACCEPTED.md, it is not an accepted product requirement.**

If a new idea is discussed in ChatGPT, it does not become part of the product merely because it was discussed.

The idea must pass through the requirement lifecycle and be explicitly accepted before implementation treats it as authoritative.

---

**End of ACCEPTED.md**

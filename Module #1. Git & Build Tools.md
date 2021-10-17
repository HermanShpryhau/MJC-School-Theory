# Module #1. Git & Build Tools

## Build Tools

#### What are build tools? Why are they used?

#### Explain gradle build life cycle.
**Initialization**
Gradle supports single and multi-project builds. During the initialization phase, Gradle determines which projects are going to take part in the build, and creates a Project instance for each of these projects.

**Configuration**
During this phase the project objects are configured. The build scripts of all projects which are part of the build are executed.

**Execution**
Gradle determines the subset of the tasks, created and configured during the configuration phase, to be executed. The subset is determined by the task name arguments passed to the gradle command and the current directory. Gradle then executes each of the selected tasks.

#### How can you run build?

#### How can you run tests?

#### How can you generate javadoc?

#### What are a gradle project and task?

#### How can you display the available projects? 

#### How can you display the available tasks?

#### What is dependency management? Why is it needed?

#### What is dependency?

#### What is repository?

#### What is scope of a dependency (configuration)?

#### What is dependency cache? Why is it needed?

#### What is transitive dependencies?

#### Which kind of dependencies you know? Tell about module and file dependencies.
#### 
#### What is the difference between api and implementation scopes?

#### How can you display project dependencies?

#### What is the use of multi-project builds?

#### What do allprojects and subprojects sections mean?

#### What is the use of plugins?

#### What is gradle wrapper?

## Git

#### What does VCS mean? What VCS do you know? Name at least 2. Tell about the differences.
Разница по механизму работы с файлами:
1. Lock-modify-unlock.
*SCCS, VAULT*
Заблокировать файл на сервере для изменения файла -> внести изменения -> снять блокировку.

2. Copy-modify-merge.
*GIT, Mercurial*
Сервер проверяет изначальную версию у клиента и текущую версию у себя. Если они совпадают, то вносятся изменения в файл на сервере. Если нет, то сервер предлагает обновить клиенту версию, чтобы начальная версия клиента совпадал с актуальной версией фала на сервере.

#### Why do we need VCS? What principles are they built upon?
1. Backup and restore
2. Synchronization
3. Undo
4. Track changes and ownership
5. Sandboxing – возможность создать изолированные "контейнеры" для проверки чего-нибудь
6. Branching

#### What are the basic concepts of git (working tree, indices, branches, etc.)?
**Рабочее дерево *(Working tree)*** — любая директория в вашей файловой системе, связанная с репозиторием (что можно видеть по наличию в ней поддиректории «.git»). Включает в себя все файлы и поддиректории.

**Коммит *(Commit)***. В роли существительного: «моментальный снимок» рабочего дерева в какой-то момент времени. В роли глагола: коммитить (закоммитить) — добавлять коммит в репозиторий.

**Репозиторий *(Repository)*** — это набор коммитов, т.е. просто архив прошлых состояний рабочего дерева проекта на вашей или чьей-то машине.

**Ветка *(Branch)*** — просто имя для коммита, также называемое ссылкой (reference). Определяет происхождение — «родословную» коммита, и таким образом, является типичным представлением «ветки разработки».

**Индекс (Index).** Место хранения состояний фалов в репозитории. В индексе для каждого файла хранятся его хэши блоков его состояния в **рабочей директории**, **staging area**, **репозитории**.

/media/index.png

#### What is HEAD? How can you detach a HEAD? How (and where) can you attach a head back?
**HEAD** — заголовок. Используется репозиторием для определения того, что выбрано с помощью checkout.

**Если субъект checkout — ветка**, то HEAD будет ссылаться на нее, показывая, что имя ветки должно быть обновлено во время следующего коммита. **Если субъект checkout — коммит**, то HEAD будет ссылаться только на него. В этом случае HEAD называется обособленным (detached).

Если после перехода в состояние detached HEAD необходимо вернуться в обычное состояние, можно просто checkout на необходимую ветку, и тогда HEAD снова будет указывать на выбранную ветку, а не на определённый commit. Если были сделаны какие-то изменения, то после checkout на ветку, они исчезнут. Если необходимо их сохранить, то надо создать новую ветку и checkout в нее.

```
$ git checkout -b feature
```

#### What is the difference between @~ and @^? Where can you use these expressions?
`@` – псевдоним для `HEAD`
После `~` можно указать необходимое число коммитов.
`^` * n аналогичен `~n`.
Используется в `rebase`, `diff`, `show`. 

#### What is rebasing, merging, cherry-picking? What is the difference between them?

#### What is git GC? What is it used for? Can you interact wit git GC?
Используется для сборки мусора и проверки не привесили ли определенные служебные ресурсы выделенного для них трэшхолда, который остается после работы гита (например недостижимые коммиты). Можно настроить через `git config`.
Отрабатывает автоматически при исполнении следующих команд:
- `git pull`
- `git merge`
- `git rebase`
- `git commit`

#### Can you have multiple remote repositories for one local? Explain the answer.

#### What is a bare repository?

#### What is happening inside .git/ directory?

#### What are hooks? How to use them? Where to find samples? What are they commonly used for? Investigate husky.

#### What are merge conflicts? How to deal with them?

#### What is vim? Where is it used in git?

#### How to search for a word in vim?

#### How to use regular expressions in vim?

#### How to exit from vim?
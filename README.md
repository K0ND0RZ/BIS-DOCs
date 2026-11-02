# BIS — Block Integration Stream

BIS is a lightweight organizational memory and context-generation system. It transforms authorized project communication into persistent semantic memory and generates query-oriented contexts from that memory when needed.

## Processes

### Semantic input

Authorized administrators submit factual project information through Patrick Blue on Telegram using `/block`.

Accepted blocks are stored in the local semantic pool, which serves as the canonical organizational memory of BIS. A searchable representation of these blocks is synchronized with an OpenAI Vector Store.

### Context generation

Authorized administrators submit a precise question through Patrick Red using `/file`.

OpenAI retrieves the semantic blocks relevant to the query and constructs a Dynamically Oriented Context (DOC).

Each DOC contains:

- **Pragmatics** — a compact orientation to the requested context;
- **Chronology** — a concise reconstruction of the relevant temporal sequence;
- **Integration** — a detailed synthesis of relevant facts and relationships;
- **Unresolved** — relevant information that cannot be established from the available memory;
- **Sources** — identifiers of the semantic blocks actually used.

The completed DOC is published as a Markdown file in the public `BIS-DOCs` GitHub repository. Patrick Red returns the public GitHub link to Telegram.

Published DOC filenames use the canonical form:

```text
(YYYYMMDD)_<localized_slug>.md
```

Example:

```text
(20261102)_négycsatornás_ilf_hazai_relevanciája.md
```

## Rules

The local semantic pool is the canonical memory of BIS.

OpenAI provides semantic retrieval and context generation.

GitHub stores published DOCs and their version history.

Persistent memory contains factual semantic blocks. Interpretation and integration occur during DOC generation.

The orientation of every DOC is determined by the administrator's `/file` query.

Every successfully published DOC is preserved as a separate Markdown file and Git commit.

Git commit messages use a short underscore-separated event name.

Generated DOC publication commits use the localized DOC slug as the event name.

Example:

```text
DOC:    (20261102)_négycsatornás_ilf_hazai_relevanciája.md
COMMIT: négycsatornás_ilf_hazai_relevanciája
```

Manual commits use an event name that describes the operation.

Examples:

```text
commit_protokoll_egységesítése
readme_frissítése
github_publisher_frissítése
```

Git stores the commit timestamp, author, committer, signature data, and commit hash. Local Git history provides the audit view for these records.

### Language configuration

BIS core uses language-neutral structures.

The current operational language is Hungarian.

A language profile controls:

- DOC output language;
- section labels;
- generation instructions;
- generated DOC title;
- localized slug;
- generated publication commit event name;
- human-facing Telegram status text.

The Hungarian profile produces Hungarian DOC content, titles, slugs, section labels, and publication commit event names.

Example:

```text
DOC:    (20261102)_négycsatornás_ilf_hazai_relevanciája.md
COMMIT: négycsatornás_ilf_hazai_relevanciája
```

The future English administrative soft fork uses the same BIS core with an English language profile.

Example:

```text
DOC:    (20261102)_domestic_relevance_of_four_channel_ilf.md
COMMIT: domestic_relevance_of_four_channel_ilf
```

Core invariants across language profiles:

- semantic block schema: `date`, `seq`, `text`;
- canonical local semantic pool;
- block identifiers and source references;
- OpenAI Vector Store structure and synchronization;
- DOC field structure and semantic roles;
- Telegram authorization and topic restrictions;
- GitHub publication mechanism;
- filename construction rule `(YYYYMMDD)_<localized_slug>.md`;
- commit creation mechanism;
- local Git history and audit semantics.

The generated title, localized slug, DOC filename, and publication commit event name form one consistent language-specific metadata chain.

## Authentication layers

BIS uses separate authentication mechanisms for Telegram, OpenAI, GitHub publication, GitHub SSH access, and local Git commit signing.

**Telegram** — Patrick Blue authenticates with `BLUE_BOT_TOKEN` and Patrick Red with `RED_BOT_TOKEN`. Both are Telegram Bot API tokens. The designated supergroup, topic ID, and administrator status define where authorized `/block` and `/file` operations are accepted.

**OpenAI** — BIS authenticates with `OPENAI_API_KEY`, an OpenAI secret API key. `OPENAI_VECTOR_STORE_ID` is the `vs_...` resource identifier of the private Vector Store used for semantic retrieval and DOC generation.

**GitHub publication** — Patrick Red authenticates to the GitHub REST API with `GITHUB_TOKEN`, a fine-grained Personal Access Token restricted to the `BIS-DOCs` repository with Contents read/write permission. This credential is used when generated DOC files and their publication commits are created.

**GitHub SSH** — local Git operations use an Ed25519 SSH key pair. `id_ed25519` is the private authentication key stored on the local machine, and `id_ed25519.pub` is its public counterpart registered with GitHub. This key pair is used for clone, pull, and push operations.

**Git commit signing** — local commits are signed with an OpenPGP/GPG signing key pair. The private GPG key creates the commit signature and is protected by the GPG signing passphrase. The public GPG key is registered with GitHub for signature verification. Git identifies the active signing key through `user.signingkey`, using the GPG Key ID; the full GPG fingerprint provides its unique cryptographic identity.

## Software chain

```text
Telegram
   ↓
Patrick Blue / Patrick Red
   ↓
Local Semantic Pool
   ↓
OpenAI Vector Store + DOC Generation
   ↓
GitHub BIS-DOCs
   ↓
Public DOC link returned to Telegram
```

**Telegram** — operational interface.  
**Local storage** — canonical semantic memory.  
**OpenAI** — semantic retrieval and contextual integration.  
**GitHub** — durable public publication and version history for generated DOCs.

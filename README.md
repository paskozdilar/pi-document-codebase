# pi-document-codebase

Pi skill for generating practical onboarding documentation for a repository.

## Install

```bash
pi install git:github.com/paskozdilar/pi-document-codebase
```

## Use

```text
/skill:document-codebase
```

The skill asks Pi to write docs under `docs/codebase/` using an adaptive structure:
- always: `README.md`, `gaps.md`
- usually: `modules/README.md`, `modules/<module>.md`
- when useful: `architecture.md`, `operations.md`

# lib-libc

codegen-generated libc FFI bindings — generic ports plus the
`c-linux-x86_64` adapters, emitted under `handl-packages/generated/` (a
gitignored build artifact).

## Layout

| path | role |
| --- | --- |
| `specs/libc.sdkgen.yaml` | committed sdkgen input narrowing the surface per header basename |
| `scripts/hnl-gate.sh` | the gate — regenerates bindings (`LIBC_HEADERS`) then typechecks/lints/tests |
| `handl-packages/generated/` | build artifact — one `.hnl` module per C header, gitignored |
| `pkg.hnl` | manifest — metadata only; the repo ships no `src/` modules |

## Contract

- One module per C header (`libc/epoll.hnl` ← `sys/epoll.h`, …); the
  `specs/libc.sdkgen.yaml` config narrows each header's surface.
- Consumed through `.handl.toml` path deps — generated output is not
  committed.

## Regenerate

```bash
# one invocation per bound header — see LIBC_HEADERS in scripts/hnl-gate.sh
hnl codegen c /usr/include/sys/epoll.h \
  --output handl-packages/generated/libc \
  --config specs/libc.sdkgen.yaml
```

Needs libclang — set `LIBCLANG_PATH` if the loader cannot find it.

## Verification

```bash
HNL=../host-cli/target/debug/hnl scripts/hnl-gate.sh
```

## License

MIT.

<!-- myco:agent-awareness -->

## Agent awareness

- Session start / any work discussion: run `myco status --prompt`
  before proposing next steps — know the board first.
- User describes new work → offer to capture it (`myco todo add`,
  `issue new`, `proposal new`). Don't wait to be asked.
- Picking up a task: `myco resume <ID>` is the briefing.
- Wrapping up: `myco handoff <ID>` anything resumable; keep statuses
  honest (claimed → in-progress → blocked/done).

<!-- /myco:agent-awareness -->

# Fighting Vipers (Sega Saturn, Sega AM2, 1996) reverse-engineering notes and symbols

Hand-named function map, naming prompt and tools for **Fighting Vipers** (GS-9101, Japan Rev C): the main program `AAFV.BIN` (SH-2, loaded at 0x06004000) and its code overlays `MOVIE.BIN` (0x060a6000), `SELEPROG.BIN` (0x05c78000, VDP1 VRAM area) and `BACKUP.BIN` (0x00200000, low work RAM). Bases come from loader call arguments and pointer votes into SH-2 prologues (see NOTES.md).
Current state (after round 1 naming + one review round on the main program):

| Program | Functions | Named | Provisional |
|---|---|---|---|
| AAFV | 1,416 | 1,416 | 94 |
| MOVIE | 623 | 623 | 264 |
| SELEPROG | 44 | 44 | 43 |
| BACKUP | 22 | 22 | 21 |

0 `FUN_` placeholders left.

- Round 1 (11-01): every function Ghidra found (SH-2 `4f22`/`2fe6` prologue seeding + `CreateAt.java`) named from its decompile and strings in ~150-function slices by Sonnet agents; unsure names carry `provisional` in the comment.
- Review round 1 (11-01): the main program's provisional names re-read with callers/callees (`ghidra/review/all_*_rv.txt`: `<prog> 0x<addr> <old> CONFIRM/RENAME/UNSURE <new> <reason>`).
- Overlays: names copied from already-named programs by masked byte hashes (`tools/xmatch.py`: SH-2 literal-pool words that look like pointers are masked, a name is copied only on a unique match), the rest named like round 1.

Subsystem prefixes (most common first): gfs_/cdc_/cd_ (Saturn GFS file system and CD block), menu_, fight_/fighter_, vdp2_/vram_/gfx_, smpc_ (pads, system), math_, movie_, select_, stm_/strm_ (streams), snd_/sound_/audio_, backup_ (save RAM).

Same method as [deadoralive-decomp](https://github.com/indieindie7/deadoralive-decomp). Non-commercial, done for study and preservation.

**No game data is included**: no disc image, no executables, no overlays, no decompiled C, no game text,
no Ghidra database. You need your own copy of the game to use any of this.

## What is here

| Path | Content |
|---|---|
| `NOTES.md` | disc layout, load bases and how they were found, pipeline, log |
| `ghidra/hand_<prog>.txt` | hand names: `0x<addr> func <name> hand <comment>` (apply with `ghidra/scripts/ApplyNames.java`, hand mode) |
| `ghidra/funcinfo_<prog>.tsv` | per-function export: `F` address name source size, `C` callees (string rows removed) |
| `ghidra/naming/` | the naming prompt |
| `ghidra/review/` | review verdict lines |
| `ghidra/scripts/` | `FindSh2FunctionsMem.java` (prologue seeding), `CreateAt.java`, `ApplyNames.java`, `ExportFuncInfo.java`, `CsDumpAll.java` / `DumpDecomp.java` (decompile dump), `DataRefs.java`, `DeleteFunctions.java` |
| `tools/` | `slice.py` (naming slices), `pubfuncinfo.py` |

## Licence

Notes, names and tools: MIT (see `LICENSE`). Fighting Vipers belongs to its rights holders; nothing from the
game is distributed here.

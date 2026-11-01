# Fighting Vipers (Saturn) decomp — NOTES
## Index
1. Source + layout  2. Pipeline  3. Log
## 1. Source + layout
- User 11-01: <own disc rip> (Japan Rev C, 19 tracks, disc 'Fighting Vipers (Japan) (Rev C)'). Unpacked: saturn/image (bin/cue), saturn/cd (isoextract.py 2352, Scorcher/tools).
- IP.BIN: SEGA SEGASATURN, GS-9101 V1.013, 1996-09-18, area J, 1st read addr 0x06004000.
- Main exe AAFV.BIN, load base 0x06004000 (verified: call/pointer targets hit SH-2 prologues 4f22 best at this base).
- saturn/cd = 230 files, 70.7 MB. Per-fighter MOT_xxx / Mxxx0-1 / Cxxx / PO_xxxnn; CMD_TBL.BIN, xxxSTG.BIN stages. OVERLAYS (SH-2 code, not imported yet): MOVIE.BIN (122080 B, 492 prologues), SELEPROG.BIN (27784 B, 60), BACKUP.BIN (7976 B, 21). OPHD.BIN/OPHD2.BIN 23 MB = movie data (false positives).
- Never push disc data. No public decomp checked.
## 2. Pipeline (copied from DeadOrAlive 10-27)
- ghidra/run.sh <abs log> <args> (project FV in ghidra/), ghidra/import.sh <file in bin/> <base> = import SH-2 + FindSh2FunctionsMem + CreateAt + ExportFuncInfo + CsDumpAll. Runs with HOGGATE=ok when hoggate blocks.
- tools/slice.py <decomp> ghidra/naming <P> -> ~150-fn naming slices. Prompt ghidra/scripts/prompt_FV.txt (from prompt_DOA.txt).
- Then naming (Sonnet agents -> naming/q_<n>.txt), hand_<exe>.txt, ApplyNames, export, provisional review, publish.
## 3. Log
- 11-01: import AAFV.BIN @0x06004000 DONE: F=1416 functions (funcinfo_AAFV.tsv, decomp_AAFV.c). 10 slices ghidra/naming/FV_1..10.c. No names applied yet (all FUN_).
- 11-01: names merged (mergeapply.sh), ApplyNames + export done; counts above.
- 11-01: overlay bases: MOVIE.BIN 0x060a6000 (basefind 138 votes), SELEPROG.BIN 0x05c78000 (cd_LoadSeleprogBin literal, VDP1 VRAM area), BACKUP.BIN 0x00200000 (cd_LoadBackupBin callers' arg). Imported as separate programs.
- 11-01: overlay import: MOVIE F=623, SELEPROG F=44, BACKUP F=3 (21 prologues; recheck).
- 11-01: review r1 (Sonnet, review/t_*_rv.txt -> all_*_rv.txt, backup hand_*.r0.txt) applied; 94 provisional left; FUN=0.
- 11-01: overlay names merged (naming/o_*.txt -> hand_<OVL>.txt): MOVIE 623 (264 prov), SELEPROG 44 (43), BACKUP 3 (2); FUN=0.
- 11-01: BACKUP recheck: 19 missed fns created (CreateAt at 4f22 prologues, log createat_BACKUP.log), named naming/o_BACKUP_2.txt; BACKUP 22 fn.

# Stoabrogga's patches<br>(Discontinued as of 2024-07-29)

Client experimentation; meant to be used together with [mod-stoabrogga](https://gitlab.com/opfesoft/mod-stoabrogga).

- Hearthstone cooldown 60s
- Ghost Wolf is usable indoors
- A few custom mounts/items/models
- Enable flying everywhere
- Increase height of the upper plane of the bounding box for flying (MFBO) in Draenei and Blood Elf areas (no "hiccups" anymore if flying there)

## Use full precision linear splines

The client only supports low precision linear splines (3 float values packed into one integer value). In order to use full precision (see worldserver parameter "UseFullPrecisionLinearSplines") a function of the client at 0x007152b0 has to be overwritten:

```
55              PUSH       EBP
8b ec           MOV        EBP,ESP
83 ec 0c        SUB        ESP,0xc
8d 4d fc        LEA        ECX,[EBP + -0x4]
51              PUSH       ECX
8b cb           MOV        ECX,EBX
e8 7f 61        CALL       0x0047b440
d6 ff
d9 45 fc        FLD        dword ptr [EBP + -0x4]
8d 4d f8        LEA        ECX,[EBP + -0x8]
51              PUSH       ECX
8b cb           MOV        ECX,EBX
e8 71 61        CALL       0x0047b440
d6 ff
d9 45 f8        FLD        dword ptr [EBP + -0x8]
8d 4d f4        LEA        ECX,[EBP + -0xc]
51              PUSH       ECX
8b cb           MOV        ECX,EBX
e8 63 61        CALL       0x0047b440
d6 ff
d9 45 f4        FLD        dword ptr [EBP + -0xc]
d9 5e 08        FSTP       dword ptr [ESI + 0x8]
d9 5e 04        FSTP       dword ptr [ESI + 0x4]
d9 1e           FSTP       dword ptr [ESI]
8b c6           MOV        EAX,ESI
8b e5           MOV        ESP,EBP
5d              POP        EBP
c3              RET
```

The new function is smaller than the old one, so the rest has to be filled up with "cc".

# xjb

## T1

### 简答题

1. `30*BX`
2. 1: `AX`, 2: 带符号
3. 1: `1200H`; 2: `100H`; 3: `0CDABH`; 4:`0CDABH`, 5:`0CDABH`, 6:`7856H`, 7:`0D3EFH`, 8: `CDABH`
4. `7CH, 3EH, 1FH`
5. AX: `230`, BX: `231`

### 程序填空题

1. 16进制转10进制并输出:
   1. `INT 21H`
   2. `CMP 39H`
   3. `DEC CH`
   4. `MOV SI 10`
   5. `ADD DX, 30H`

### 阅读程序

1. 把`SOURCE_BUFFER`复制到`DEST_BUFFER`
2. 读入一个串，保持原样并输出
3. 1: 把串中小写字母转为大写 2：`ER39*5867JGEEWFGHYUO9385`
4. |偏移地址|栈内容|
   |-------|------|
   |1F4H   |0022H |
   |1F6H   |001DH |
   |1F8H   |0013H |
   |1FAH   |000FH |
   |1FCH   |0000H |
   |1FEH   |0F80  |
   `SP (1F5H)`

### 程序设计题

``` ASM
; 1:
DATAS SEGMENT
    X dw 2767
    ONE DW 0
DATAS ENDS
CODES SEGMENT
    ASSUME CS:CODES,DS:DATAS,SS:STACKS
START:
    MOV AX,DATAS
    MOV DS,AX
    MOV AX, X
LP:
    SAR AX, 1
    JNC CHK
    INC ONE
CHK:
    OR AX, AX
    JNZ LP
    MOV AH,4CH
    INT 21H
CODES ENDS
    END START
--------------------------------------------------
; 2
DATAS SEGMENT
    CNT DW 0
DATAS ENDS
CODES SEGMENT
    ASSUME CS:CODES,DS:DATAS,SS:STACKS
START:
    MOV AX, DATAS
    MOV DS, AX

LP:
    MOV AH, 01H
    INT 21H
    CMP AL, '$'
    JE PRI
    CMP AL, 30H
    JL CAL
    CMP AL, 39H
    JG CAL
    JMP LP
CAL:
    INC CNT
    JMP LP

PRI:
    MOV DL, 0DH
    MOV AH, 02H
    INT 21H
    MOV DL, 0AH
    MOV AH, 02H
    INT 21H

    MOV CX, 0
    MOV AX, CNT
LP2:
    MOV DX, 0
    MOV BX, 10
    DIV BX
    PUSH DX
    INC CX
    OR AX, AX
    JNZ LP2
LP3:
    POP DX
    ADD DX, 30H
    MOV AH, 02
    INT 21H
    DEC CX
    JNZ LP3

    MOV AH, 4CH
    INT 21H
CODES ENDS
    END START
--------------------------------
; 3
DATAS SEGMENT
    STRING1 DB 'ASDFSADFASDFASDF'
    LEN1 DW $-STRING1
    STRING2 DB 'ADDFSADFASDFASDF'
    LEN2 DW $-STRING2
    MATCH DB 'MATCH$'
    NOMATCH DB 'NO MATCH$'
DATAS ENDS
CODES SEGMENT
    ASSUME CS:CODES,DS:DATAS,SS:STACKS
START:
    MOV AX, DATAS
    MOV DS, AX
    MOV ES, AX
    MOV AX, LEN1
    MOV BX, LEN2
    CMP AX, BX
    JE NXT
    JMP NS
NXT:
    LEA DI, STRING1
    LEA SI, STRING2
    MOV CX, LEN1
    CLD
    REPZ CMPSB
    OR CX, CX
    JZ EQR
    JMP NS
NS:
    LEA DX, NOMATCH
    JMP PRI
EQR:
    LEA DX, MATCH
PRI:
    MOV AH, 09H
    INT 21H
    MOV AH, 4CH
    INT 21H
CODES ENDS
    END START
```

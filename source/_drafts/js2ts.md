title: javascript转typescript
date: 2019-06-27 11:42:25
categories: 笔记
tags: [笔记, 技术, javascript, typescript]
---
### javascript转typescript
#### 语法树研究
##### 3D战警
- Toplevel
  - Defun
  - Var
    - *definitions*
      - VarDef
        - *name*
          - SymbolVar
            - *name*
            - *init*
            - *scope*
            - *thedef*
        - *value*
          - Assign
            - *left*
              - Dot
                - *expression*
                  - SymbolRef
                    - *fixed*
                    - *name*
                    - *scope*
                    - *thedef*
                - *property*
            - *operator*
            - *right*
  - SimpleStatement
    - *body*
      - Sequence
        - *expressions*
      - New
      - Assign
      - Call
   

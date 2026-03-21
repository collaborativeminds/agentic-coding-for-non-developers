# Förberedelser inför kursen — Agentic AI för icke-utvecklare

Välkommen! Nedan hittar du allt du behöver installera och konfigurera på din dator innan kursdagen. Instruktionerna är uppdelade per verktyg, med separata steg för macOS och Windows där det behövs.

Räkna med att hela förberedelsen tar ungefär 60–90 minuter. Stegen som tar mest tid är Docker Desktop (nedladdning och konto-skapande), GitHub-konfigurationen och Claude Code-inloggningen.

> **Viktigt:** Efter att du installerat ett nytt verktyg som ska köras i terminalen behöver du stänga och öppna terminalen igen innan kommandot blir tillgängligt. Detta beror på att terminalen behöver läsa in uppdaterade sökvägar (PATH) för att hitta det nya programmet.

> **Systemkrav:** Se till att din dator uppfyller minimikraven nedan innan du börjar med installationerna.

### Dator och operativsystem

- **macOS:** Vi rekommenderar en Mac med Apple Silicon-processor (M1, M2, M3, M4 eller M5) och senaste versionen av macOS (macOS 26).
- **Windows:** Vi rekommenderar Windows 11. Windows 10 kan fungera, men vi kan inte garantera att alla verktyg fungerar felfritt.

### Webbläsare

Vi rekommenderar att du har [Google Chrome](https://www.google.com/chrome/) installerad, då Claude Code har färdiga plugins för den. Andra webbläsare kan fungera, men under kursen utgår vi från Chrome.

### Lokal administratör

Du måste vara **lokal administratör** på din dator för att kunna installera alla verktyg som krävs. Kontakta din IT-avdelning i god tid om du inte har administratörsrättigheter.

> **Tips: Använd en lösenordshanterare.** Under förberedelserna kommer du skapa konton hos flera olika tjänster (GitHub, Claude, Docker m.fl.). Vi rekommenderar starkt att du använder en lösenordshanterare för att hålla ordning på alla inloggningar och generera starka, unika lösenord. Några bra alternativ: [1Password](https://1password.com/), [Bitwarden](https://bitwarden.com/) (gratis-alternativ), eller [Proton Pass](https://proton.me/pass).

---

## 1. Terminal

En terminal är programmet där du skriver kommandon för att styra din dator — till exempel för att installera verktyg, starta program, eller köra kod. Flera av stegen i den här guiden kräver att du kör kommandon i terminalen, och under kursen kommer vi använda den löpande.

### macOS: Ghostty

Vi rekommenderar **Ghostty** — en snabb och modern terminal. Ladda ner från [ghostty.org](https://ghostty.org/) och dra appen till din Applications-mapp.

> **Tips:** macOS har en inbyggd terminal-app som heter "Terminal" (i Applications > Utilities). Den fungerar också, men Ghostty ger en bättre upplevelse.

### Windows: Windows Terminal

Vi rekommenderar **Windows Terminal** — Microsofts moderna terminal-app. Den är förinstallerad på Windows 11. Om du använder Windows 10, ladda ner den gratis från [Microsoft Store](https://apps.microsoft.com/detail/9n0dx20hk701).

> **Tips:** Kör alltid kommandon i **Windows Terminal** (eller PowerShell), inte i den äldre "Kommandotolken" (cmd.exe).

### Så startar du Windows Terminal

Det finns flera sätt att öppna Windows Terminal:

1. **Via Start-menyn:** Klicka på Start-knappen (Windows-ikonen längst ner till vänster) och skriv `Terminal`. Klicka på **Terminal** i sökresultatet.
2. **Via tangentbordet:** Tryck på `Win + R`, skriv `wt` och tryck Enter.
3. **Via högerklick:** Högerklicka på Skrivbordet eller i en mapp och välj **Öppna i Terminal** (Windows 11).

När Terminal startar ser du ett fönster med mörk bakgrund och en blinkande markör — det är där du skriver kommandon.

> **Tips:** Du kan fästa Terminal i aktivitetsfältet genom att högerklicka på Terminal-ikonen och välja **Fäst i aktivitetsfältet**, så hittar du den snabbt nästa gång.

I resten av det här dokumentet betyder "öppna terminalen" att du startar Ghostty (macOS) eller Windows Terminal (Windows).

---

## 2. Homebrew — pakethanterare för macOS

Homebrew är en pakethanterare för macOS som gör det enkelt att installera och uppdatera utvecklingsverktyg direkt från terminalen. Istället för att leta upp och ladda ner installationsfiler från webbsidor kan du installera program med ett enda kommando. Flera av verktygen i den här guiden kan installeras via Homebrew, och många guider på nätet utgår från att du har det.

### Vad används Homebrew till?

Med Homebrew kan du till exempel köra `brew install <verktyg>` istället för att ladda ner <verktyg> manuellt. Det håller också dina installerade verktyg uppdaterade med kommandot `brew upgrade`.

### Installera Homebrew

Öppna terminalen och kör:

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Följ instruktionerna på skärmen — du kan behöva ange ditt Mac-lösenord. Installationen tar några minuter.

**Verifiera installationen:** Stäng terminalen, öppna den igen och kör:

```
brew --version
```

Du bör se ett versionsnummer (t.ex. `Homebrew 4.x.x`).

> **Observera:** Homebrew är bara tillgängligt för macOS. Windows-användare använder istället `winget` (förinstallerat i Windows 10/11) för liknande funktionalitet.

---

## 3. Node.js

Node.js är den plattform vi använder för att köra våra applikationer. TypeScript (språket vi skriver kod i) installeras i ett senare steg.

**macOS och Windows:**
Gå till [nodejs.org/en/download](https://nodejs.org/en/download). Scrolla ner till sektionen **"Or get a prebuilt Node.js® for..."** och välj ditt operativsystem:

- **macOS:** Välj **macOS** och klicka på **"macOS Installer (.pkg)"**
- **Windows:** Välj **Windows** och klicka på **"Windows Installer (.msi)"**

Välj den version som är markerad **LTS** (Long Term Support, för närvarande v24.14.0). De flesta moderna Apple-datorer kör ARM64-arkitektur, men om du har en äldre dator kan du behöva välja x64 istället. Kör installationsfilen och följ instruktionerna — standardinställningarna fungerar bra.

**Verifiera installationen:** Öppna terminalen och kör:

```
node --version
```

Du bör se ett versionsnummer (t.ex. `v24.x.x`).

---

## 4. Docker Desktop

Docker låter oss köra applikationer i isolerade miljöer, så kallade containers. Vi använder det för att enkelt starta databaser och andra tjänster.

> **Windows-användare:** Docker Desktop kräver att WSL 2 (Windows Subsystem for Linux) är aktiverat. Installationsprogrammet hjälper dig med detta, men om du stöter på problem finns det instruktioner i Dockers officiella guide: [docs.docker.com/desktop/setup/install/windows-install](https://docs.docker.com/desktop/setup/install/windows-install/)

**macOS och Windows:**
Ladda ner Docker Desktop från [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop/). Kör installationsfilen och följ instruktionerna.

Du behöver skapa ett gratis Docker-konto om du inte redan har ett — det gör du på samma sida.

**Verifiera installationen:** Öppna terminalen och kör:

```
docker --version
```

---

## 5. Kodeditor — Visual Studio Code eller Google Antigravity

Du behöver en kodeditor för att kunna se och navigera i koden som Claude Code genererar. Välj **en** av följande:

### Alternativ A: Visual Studio Code (VS Code)

Ladda ner från [code.visualstudio.com](https://code.visualstudio.com/). Välj rätt version för ditt operativsystem, kör installationsfilen och följ instruktionerna.

### Alternativ B: Google Antigravity

Ladda ner från [antigravity.google/download](https://antigravity.google/download). Välj rätt version för ditt operativsystem, kör installationsfilen och följ instruktionerna.

Båda alternativen fungerar lika bra för den här kursen.

---

## 6. Git

Git är det verktyg som håller koll på alla ändringar i koden — tänk på det som en avancerad versionshistorik.

**macOS:**
Öppna terminalen och kör:

```
brew install git
```

**Windows:**
Öppna terminalen och kör:

```
winget install --id Git.Git
```

### Konfigurera Git med ditt namn och e-post

När Git är installerat, öppna terminalen och kör följande tre kommandon (byt ut mot ditt namn och din e-postadress):

```
git config --global user.name "Ditt Namn"
git config --global user.email "din.email@foretaget.se"
git config --global push.autoSetupRemote true
```

---

## 7. GitHub-konto och GitHub CLI

GitHub är tjänsten där vi lagrar och samarbetar kring kod.

### Skapa ett GitHub-konto

Om du inte redan har ett, gå till [github.com](https://github.com/pricing) och skapa ett konto. Du behöver ett **Team-abonnemang** (från 4 USD/mån) — kontakta din chef eller IT-avdelning om du behöver hjälp med detta.

### Installera GitHub CLI

GitHub CLI (`gh`) är ett kommandoradsverktyg som låter dig arbeta med GitHub direkt från terminalen. Vi använder det under kursen för att bland annat skapa och hantera repositories.

**macOS:**
Om du har Homebrew installerat, öppna terminalen och kör:

```
brew install gh
```

Om du inte har Homebrew kan du ladda ner installationsfilen (.pkg) från [cli.github.com](https://cli.github.com/).

**Windows:**
Öppna terminalen och kör:

```
winget install --id GitHub.cli
```

Om kommandot inte fungerar kan du ladda ner installationsfilen (.msi) från [cli.github.com](https://cli.github.com/).

### Logga in med GitHub CLI

När GitHub CLI är installerat, öppna terminalen (stäng och öppna den igen om den redan var öppen) och kör:

```
gh auth login
```

Välj **GitHub.com**, följ instruktionerna i terminalen, och logga in via webbläsaren när du ombeds. När inloggningen är klar bör du se ett bekräftelsemeddelande i terminalen.

> Viktigt att tänka på med Git: Var noga med att aldrig placera ett git-repo i en mapp synkas med en tjänst som iCloud, Dropbox eller OneDrive. Det kan leda till problem med versionshanteringen.

---

## 8. Claude Code

Claude Code finns i två delar: en **desktop-app** (för det grafiska gränssnittet) och ett **CLI-verktyg** (kommandoradsverktyget vi använder i terminalen för att bygga applikationer).

### Claude-konto

Du behöver ett Claude-konto med minst **Pro-abonnemang** (från 20 USD/mån). Skapa ett konto eller uppgradera på [claude.ai](https://claude.ai/).

### Claude Desktop-app

Ladda ner från [claude.ai/download](https://claude.ai/download). Välj rätt version för ditt operativsystem, installera, och logga in med ditt Claude-konto.

### Claude Code CLI

**macOS:**
Öppna terminalen och kör:

```
curl -fsSL https://claude.ai/install.sh | bash
```

**Windows:**
Öppna terminalen och kör:

```
irm https://claude.ai/install.ps1 | iex
```

### Logga in med Claude Code CLI

När installationen är klar, stäng terminalen och öppna den igen. Kör sedan:

```
claude
```

Första gången du kör kommandot kommer du att guidas genom en inloggning i webbläsaren. Följ instruktionerna för att koppla CLI-verktyget till ditt Claude-konto.

Fullständig installationsguide finns på [docs.anthropic.com/en/docs/claude-code/setup](https://docs.anthropic.com/en/docs/claude-code/setup).

---

## 9. TypeScript (installeras via npm)

När Node.js är installerat (steg 2) kan du installera TypeScript. Öppna terminalen och kör:

```
npm install -g typescript
```

**Verifiera installationen:** Kör följande i terminalen:

```
tsc --version
```

---

## 10. Ytterligare CLI-verktyg för fullstack-utveckling

Dessa verktyg gör det möjligt för Claude Code att testa API:er, inspektera databaser, formatera kod och hantera konfigurationsfiler direkt från terminalen — utan att behöva byta kontext.

### Installera via npm (macOS och Windows)

| Verktyg            | Vad Claude Code använder det till                                                               |
| ------------------ | ----------------------------------------------------------------------------------------------- |
| **prettier**       | Formaterar genererad kod automatiskt till ett enhetligt stilformat                              |
| **eslint**         | Analyserar TypeScript/JavaScript-kod och flaggar fel mot projektets regler                      |
| **pm2**            | Startar och övervakar Node.js-processer; startar om dem automatiskt vid krascher                |
| **playwright**     | Automatiserar webbläsare för end-to-end-testning — Claude Code kan skriva och köra UI-tester    |
| **playwright-cli** | CLI-gränssnitt för Playwright — inspekterar sidor, spelar in tester och kör dem från terminalen |

```
npm install -g prettier eslint pm2 playwright @playwright/test
```

### Installera via Homebrew (macOS) / Winget (Windows)

| Verktyg       | Vad Claude Code använder det till                                                          |
| ------------- | ------------------------------------------------------------------------------------------ |
| **python**    | Kör Python-skript och används av många CLI-verktyg som ett beroende                        |
| **curl**      | Skickar HTTP-förfrågningar för att testa API:er och ladda ner resurser                     |
| **mongosh**   | Kommandoradsskal för MongoDB — inspekterar och felsöker databasen direkt                   |
| **psql**      | Motsvarigheten för PostgreSQL — kör SQL-frågor och granskar schema                         |
| **git-delta** | Förbättrar git-diffar med syntaxfärgning, så kodändringar blir lättare att läsa            |
| **jq**        | Bearbetar och filtrerar JSON-svar från API:er i terminalen                                 |
| **yq**        | Som jq men för YAML — läser och skriver konfigurationsfiler som `docker-compose.yml`       |
| **terraform** | Beskriver molninfrastruktur som kod; Claude Code kan generera och tillämpa konfigurationer |

**macOS:**

```
brew install python curl mongosh libpq git-delta jq yq terraform
brew link --force libpq
```

**Windows:**

```
winget install Python.Python.3 cURL.cURL MongoDB.Shell PostgreSQL.PostgreSQL dandavison.delta jqlang.jq MikeFarah.yq Hashicorp.Terraform
```

**Verifiera Python-installationen:** Stäng terminalen, öppna den igen och kör:

```
python3 --version
```

Du bör se ett versionsnummer (t.ex. `Python 3.x.x`).

> **Observera:** På Windows kan kommandot heta `python` istället för `python3`.

### Ladda ner PostgreSQL Docker-image

Under kursen använder vi PostgreSQL som databas via Docker. Kör följande kommando för att ladda ner den senaste officiella PostgreSQL-imagen i förväg (Kontrollera att Docker Desktop är igång innan du kör kommandot):

```
docker pull postgres
```

Detta laddar ner en färdigpaketerad databasmiljö som Docker kan starta direkt på din dator. Nedladdningen kan ta några minuter beroende på din internetanslutning.

---

## Checklista

Öppna terminalen och kör följande kommandon, ett i taget. Se till att alla ger ett versionsnummer eller förväntat resultat:

- [ ] `brew --version` visar ett versionsnummer (valfritt, endast macOS)
- [ ] `node --version` visar ett versionsnummer
- [ ] `docker --version` visar ett versionsnummer
- [ ] VS Code eller Antigravity startar utan problem
- [ ] `git --version` visar ett versionsnummer
- [ ] `gh auth status` visar att du är inloggad
- [ ] Claude Desktop-app är installerad och inloggad
- [ ] `claude` startar i terminalen och är kopplat till ditt konto
- [ ] `tsc --version` visar ett versionsnummer
- [ ] `python3 --version` visar ett versionsnummer

---

## Behöver du hjälp?

Om du stöter på problem med installationen, skicka ett mejl till erik.hellman@collaborativeminds.se så hjälper vi dig innan kursdagen.

# Workshop 1 — EUR/USD Indicator Lab
## 60-Minute Windows + Local AI Worksheet

## Goal

In roughly one hour, set up this workflow:

```text
EUR/USD daily data
        ↓
simple indicators
        ↓
simple statistical tests
        ↓
charts + results
        ↓
Git
        ↓
GitHub
```

Coding assistance will run locally using Ollama plus a local coding model and terminal coding agent.

No paid AI subscription is required.

This is a **research and learning project only**.

Do not connect a broker, use leverage, or place live trades.

---

# 1. Install the basic tools

Open **Windows Terminal** and choose **PowerShell**.

## Install Git

```powershell
winget install --id Git.Git -e
```

Close and reopen Windows Terminal, then check:

```powershell
git --version
```

## Install GitHub CLI

```powershell
winget install --id GitHub.cli -e
```

Restart Terminal if required, then check:

```powershell
gh --version
```

## Install `uv`

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

Close and reopen Windows Terminal, then check:

```powershell
uv --version
```

## Install Ollama

Install Ollama for Windows, then check:

```powershell
ollama --version
```

---

# 2. Install the local coding model

For the first experiment, use a relatively lightweight coding model:

```powershell
ollama pull qwen2.5-coder:7b
```

Test it:

```powershell
ollama run qwen2.5-coder:7b
```

Ask:

```text
Write one sentence explaining what a 20-day moving average is.
```

If it answers, the local model works.

Exit with `/bye` or `Ctrl+C`.

---

# 3. Create the research project

Choose a convenient location:

```powershell
cd $HOME
mkdir eurusd-indicator-lab
cd eurusd-indicator-lab
```

Initialise the Python project:

```powershell
uv init
```

Install the analysis packages:

```powershell
uv add yfinance pandas numpy matplotlib scipy statsmodels
```

Create folders:

```powershell
mkdir src
mkdir data
mkdir outputs
```

---

# 4. Initialise Git

```powershell
git init
```

Create `.gitignore`:

```powershell
@"
.venv/
__pycache__/
*.pyc
data/
outputs/
"@ | Set-Content .gitignore
```

Create the README:

```powershell
@"
# EUR/USD Indicator Lab

Small personal research project testing simple statistical indicators on daily EUR/USD data.

Research only. No broker connection or live trading.
"@ | Set-Content README.md
```

Commit:

```powershell
git add .
git commit -m "Initialise EURUSD indicator lab"
```

If Git asks for your identity, configure it once:

```powershell
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Then repeat the commit.

---

# 5. Connect GitHub

Log in:

```powershell
gh auth login
```

Choose:

```text
GitHub.com
HTTPS
Login with browser
```

Follow the browser instructions.

Check:

```powershell
gh auth status
```

Create a **private** repository:

```powershell
gh repo create eurusd-indicator-lab --private --source=. --remote=origin --push
```

Check:

```powershell
git remote -v
```

You now have:

```text
Windows PC
    ↕
   Git
    ↕
 GitHub
```

---

# 6. Start the local AI coding agent

From inside the project folder, launch the coding agent through Ollama:

```powershell
ollama launch opencode --model qwen2.5-coder:7b
```

If prompted, allow Ollama to configure OpenCode.

The coding agent can now read and edit files inside this project.

---

# 7. Give the AI the first task

Paste the following prompt into the coding agent:

```text
We are creating a small personal EUR/USD quantitative research project.

Create one Python program:

src/eurusd_test.py

Requirements:

DATA
1. Download DAILY EUR/USD data using yfinance ticker EURUSD=X.
2. Start from 2010-01-01.
3. Use Open, High, Low and Close.
4. Save the raw clean dataset to data/eurusd_daily.csv.

INDICATORS
Calculate:

1. daily percentage return
2. 20-day simple moving average
3. 50-day simple moving average
4. 14-day RSI
5. 20-day price momentum
6. 20-day rolling annualised volatility

FORWARD OUTCOMES
Calculate future EUR/USD returns over:

1 trading day
5 trading days
20 trading days

TEST THESE SIMPLE HYPOTHESES

A. Trend:
Compare subsequent returns when SMA20 > SMA50 versus SMA20 <= SMA50.

B. Momentum:
Split 20-day momentum into five equal-sized groups and report mean future returns for each group.

C. RSI:
Compare subsequent returns when RSI < 30, RSI between 30 and 70, and RSI > 70.

D. Volatility:
Split volatility into five groups and compare subsequent absolute 5-day returns.

RESULTS
Create outputs/indicator_results.csv with clearly labelled results.

CHARTS
Create:

outputs/eurusd_trend.png
showing price, SMA20 and SMA50.

outputs/eurusd_rsi.png
showing RSI with reference lines at 30 and 70.

Print a concise results summary to the terminal.

RESEARCH RULES
- no trading strategy
- no threshold optimisation
- no broker API
- no live trading
- no look-ahead leakage in indicators
- indicators must use only data available at that date
- future returns are outcomes only
- handle missing values correctly
- comment the code clearly
- use matplotlib, not seaborn

After creating the program, run it and fix any errors.
```

---

# 8. Run the program yourself

After the AI finishes:

```powershell
uv run python src\eurusd_test.py
```

Check:

```powershell
dir data
dir outputs
```

Expected files:

```text
data/
    eurusd_daily.csv

outputs/
    indicator_results.csv
    eurusd_trend.png
    eurusd_rsi.png
```

---

# 9. Inspect the first findings

Ask the local AI:

```text
Read outputs/indicator_results.csv.

Explain the results in plain English.

For each indicator tell me:

1. what relationship appears to be present;
2. whether it looks weak or potentially interesting;
3. how many observations it is based on;
4. whether the pattern might simply be noise;
5. what sensible follow-up test we could run.

Do not claim that any result is a profitable trading strategy.
```

Questions to consider:

### Trend

Does:

```text
SMA20 > SMA50
```

correspond to different subsequent EUR/USD returns?

### Momentum

Do stronger recent returns correspond to:

```text
stronger future returns
```

or:

```text
mean reversion?
```

### RSI

Do unusually low or high RSI observations show noticeably different subsequent behaviour?

### Volatility

Does high current volatility predict larger future movements?

---

# 10. Check what the AI changed

Run:

```powershell
git status
git diff
```

Only the source code and project configuration should normally be tracked.

Because `.gitignore` contains:

```text
data/
outputs/
```

the downloaded market data and generated results stay local.

---

# 11. Commit the first experiment

If everything works:

```powershell
git add .
git commit -m "Add first EURUSD indicator experiment"
git push
```

The GitHub repository now contains a reproducible record of the experiment.

---

# End-of-session checklist

```text
[ ] Git installed
[ ] GitHub CLI installed
[ ] GitHub authenticated
[ ] uv installed
[ ] Ollama installed
[ ] local coding model runs
[ ] private GitHub repo created
[ ] Python project created
[ ] EUR/USD data downloaded
[ ] SMA indicators calculated
[ ] RSI calculated
[ ] momentum calculated
[ ] volatility calculated
[ ] forward returns calculated
[ ] results CSV generated
[ ] charts generated
[ ] code committed
[ ] code pushed to GitHub
```

If those boxes are ticked, Workshop 1 is complete.

---

# What not to do today

Do not add:

```text
MACD
Bollinger Bands
neural networks
LSTMs
dozens of indicators
minute-level Forex data
parameter optimisation
MetaTrader
broker APIs
live trades
leverage
```

The purpose of Workshop 1 is to establish the **research pipeline**, not discover the perfect Forex strategy.

---

# Suggested Workshop 2

The next session can divide the data chronologically:

```text
2010–2021
DEVELOPMENT

2022–2024
VALIDATION

2025–present
UNTOUCHED TEST
```

Then ask:

> If an indicator looked interesting in the development data, did the same relationship survive in later unseen data?

That is the first important protection against overfitting.

---

# Normal workflow after Workshop 1

Every future session becomes:

```powershell
cd $HOME\eurusd-indicator-lab

git pull

ollama launch opencode --model qwen2.5-coder:7b
```

Tell the local AI what research question to implement.

Afterwards:

```powershell
uv run python src\eurusd_test.py

git diff
git status

git add .
git commit -m "Describe today's experiment"
git push
```

The full workflow is:

```text
QUESTION
   ↓
LOCAL AI
   ↓
PYTHON EXPERIMENT
   ↓
RESULT
   ↓
HUMAN REVIEW
   ↓
GIT
   ↓
GITHUB
```

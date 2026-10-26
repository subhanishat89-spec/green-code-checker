# Green Code Checker

Paste your code and find out how much energy and carbon it may cost, why, and how to make it greener.

**Live demo:** https://subhanishat89-spec.github.io/green-code-checker/

![Screenshot](screenshot.png)

## Features
- Green score from 0 to 100 with a grade from A to F
- Plain-English explanation: every finding shows **why** it wastes energy and a **greener fix**
- Score breakdown that starts at 100 and shows the points each problem cost
- Annotated code view that highlights risky lines and tags them
- Estimates for run time, CPU energy, CO₂ per run, CO₂ per 1 million runs and CO₂ per year
- Rough time complexity and memory estimate
- Growth table showing time and CO₂ at n = 10 up to 100,000
- Fun equivalents: phone charges and LED bulb hours
- Comparison with your last check, so you can see the effect of a fix
- Open a code file, load examples, copy the report, press Ctrl+Enter to check
- Responsive layout and automatic dark mode
- Your code never leaves your browser

## What it checks
| Check | Why it matters |
|---|---|
| Nested loops | Each loop level multiplies the work |
| Recursion with no cache | Calls can double at every level |
| Printing inside loops | Talks to the OS every time |
| String `+=` inside loops | Copies the whole string each time |
| New objects inside loops | Extra work for the garbage collector |
| File or network calls inside loops | The machine waits and wakes up again and again |
| `indexOf`, `includes`, `find` inside loops | A hidden loop inside a loop |
| Timers, sleep and endless loops | Keep the CPU awake for nothing |

## How it works
1. **Static analysis:** the code is scanned line by line. The tool tracks loop depth (using braces, or indentation for Python), finds functions that call themselves, and flags risky patterns inside loops.
2. **Scoring:** the score starts at 100. Each problem subtracts points and each good practice (such as caching) adds a few.
3. **Measuring (JavaScript only):** the code runs in a Web Worker and is stopped after 3 seconds. The median of several runs is used as the run time.
4. **Estimating (Python, C, Java):** these cannot run in the browser, so time is estimated from loop depth, the input size n, and an assumed cost per step.
5. **Carbon:** `energy (Wh) = time × CPU watts`, then `CO₂ = energy × grid carbon intensity`. You can change the watts, the grid value (default 475 gCO₂/kWh, about the world average) and the runs per day.

## Limitations
- These are estimates, not lab measurements.
- The score comes from pattern checks, so it can miss problems or raise false alarms.
- Only JavaScript is measured. Other languages are estimated.
- Memory is a rough guess, not a real measurement.

## Tech stack
HTML, CSS and vanilla JavaScript in a single `index.html`. Web Workers for safe timing, Google Fonts (Inter). No frameworks, no build step.

## Run locally
```bash
git clone https://github.com/subhanishat89-spec/green-code-checker.git
cd green-code-checker
```
Then open `index.html` in any modern browser.

## Future improvements
- Real measurements for Python and C using WebAssembly
- Detect loops inside `map`/`filter` chains more accurately
- Show energy for different CPUs and countries from a dropdown
- Export the report as PDF

## Author
Nishat Subha

Bugster CLI — AI UI Testing for Next.js with Playwright
[![Releases](https://img.shields.io/github/v/release/abrarahmed110/bugster-cli?label=Releases&style=for-the-badge&color=4c1)](https://github.com/abrarahmed110/bugster-cli/releases)

[![ai-testing](https://img.shields.io/badge/ai--testing-blue?style=flat-square)](https://github.com/topics/ai-testing)
[![automation](https://img.shields.io/badge/automation-yes-green?style=flat-square)](https://github.com/topics/automation)
[![cli-tool](https://img.shields.io/badge/cli--tool-lightgrey?style=flat-square)](https://github.com/topics/cli-tool)
[![debugging](https://img.shields.io/badge/debugging-orange?style=flat-square)](https://github.com/topics/debugging)
[![e2e-testing](https://img.shields.io/badge/e2e--testing-darkblue?style=flat-square)](https://github.com/topics/e2e-testing)
[![nextjs](https://img.shields.io/badge/next.js-black?style=flat-square&logo=next.js)](https://nextjs.org/)
[![playwright](https://img.shields.io/badge/playwright-282c34?style=flat-square&logo=playwright)](https://playwright.dev/)
[![ui-testing](https://img.shields.io/badge/ui--testing-purple?style=flat-square)](https://github.com/topics/ui-testing)
[![vercel](https://img.shields.io/badge/vercel-000000?style=flat-square&logo=vercel)](https://vercel.com/)
[![vibe-testing](https://img.shields.io/badge/vibe--testing-9cf?style=flat-square)](https://github.com/topics/vibe-testing)

Hero image  
![Playwright + Next.js + Vercel](https://playwright.dev/img/playwright-logo.svg)

Table of contents
- Overview
- Key features
- Quickstart (download & run)
- Install (binaries, npm, Docker)
- Usage (commands and examples)
- Config (project and CI)
- Integrations (Playwright, Next.js, Vercel)
- Debugging & logs
- Advanced patterns
- Contributing
- License
- Releases

Overview
Bugster CLI helps you test UIs with a clear, scriptable interface. Use it to run end-to-end tests, capture visual diffs, and automate checks in CI. It integrates with Playwright and Next.js, and it works well when you deploy with Vercel. Use it to run repeatable tests on PRs and to debug UI regressions with trace and snapshot output.

Key features
- E2E test runner that wraps Playwright flows.
- Visual snapshot capture and pixel-diff reports.
- AI-assisted test suggestion and selector hints.
- CLI-first workflow for local debug and CI runs.
- Native binary for Linux, macOS, Windows (download from Releases).
- Built-in report generator (HTML) for test runs.
- Support for Next.js static and server paths.
- Integrations: Playwright reporters, Vercel deploy previews.

Quickstart (download & run)
Go to the Releases page and download the binary for your OS:
https://github.com/abrarahmed110/bugster-cli/releases

Download the matching asset for your platform (for example: bugster-cli-linux, bugster-cli-macos, bugster-cli-win.exe). Make the binary executable and run it.

Example (macOS / Linux)
```bash
# download the binary from Releases, then:
chmod +x bugster-cli-macos
./bugster-cli-macos init
./bugster-cli-macos run --target=https://my-next-app.vercel.app
```

Example (Windows)
- Download bugster-cli-win.exe from the Releases page.
- Run the executable:
```powershell
.\bugster-cli-win.exe run --target=https://my-next-app.vercel.app
```

Install
Binaries (recommended for CI and local runs)
- Visit the Releases page and download the binary that matches your OS and architecture.
- Make the binary executable and place it on PATH or invoke it directly.
- The downloaded file needs to be executed to run the CLI.

NPM (if packaged)
If a Node package is available, install:
```bash
npm i -g bugster-cli
# or
npx bugster-cli run --target=https://your-app
```

Docker
You can run bugster in a container that contains browsers and Playwright dependencies.
```bash
docker run --rm -v $(pwd):/work -w /work ghcr.io/abrarahmed110/bugster-cli:latest bugster run --target=https://your-app
```

Usage
Commands follow a simple verb-noun structure.

Init
Create a project config and baseline snapshot directory.
```bash
bugster init
# creates bugster.config.json and snapshots/
```

Run
Run tests against a target URL or local server.
```bash
bugster run --target=https://staging.myapp.com --headless
```

Record
Record a new test flow. This starts a Playwright session and saves actions as a script.
```bash
bugster record --out=test-flows/login.flow.json
```

Compare
Run visual diffs against stored snapshots.
```bash
bugster compare --snapshots=./snapshots --target=https://preview.myapp.com
```

Report
Generate HTML report.
```bash
bugster report --input=results.json --output=report/
open report/index.html
```

Common flags
- --target: URL to test
- --headless / --headed: run browsers headless or visible
- --browser: chromium | firefox | webkit
- --concurrency: number of parallel sessions
- --trace: capture Playwright trace
- --snapshots: snapshot directory

Config (bugster.config.json)
A sample config:
```json
{
  "project": "my-next-app",
  "baseUrl": "http://localhost:3000",
  "browsers": ["chromium", "webkit"],
  "snapshotDir": "snapshots",
  "reportDir": "bugster-report",
  "concurrency": 2
}
```
Key fields
- baseUrl: local dev server URL.
- snapshotDir: where baseline images live.
- reportDir: HTML and JSON output.
- concurrency: parallel workers for E2E tests.

Integrations

Playwright
Bugster uses Playwright under the hood. You get playwright features: tracing, network intercepts, and browser contexts. Use Playwright selectors in your flows and scripts.

Next.js
Use bugster to run tests against dev, build, and preview URLs. You can configure Next.js static routes and API routes in bugster.config.json.

Vercel
Run bugster against Vercel preview URLs to validate pull requests. Pair bugster runs with Vercel deployments to gate UI regressions.

CI / GitHub Actions
Add a job to run tests on PRs. Example snippet:
```yaml
name: Bugster E2E
on: [pull_request]
jobs:
  e2e:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Download bugster
        run: |
          curl -L -o bugster-cli-linux https://github.com/abrarahmed110/bugster-cli/releases/download/latest/bugster-cli-linux
          chmod +x ./bugster-cli-linux
      - name: Run tests
        run: |
          ./bugster-cli-linux run --target=https://preview.myapp.vercel.app --headless --reportDir=./report
      - name: Upload report
        uses: actions/upload-artifact@v3
        with:
          name: bugster-report
          path: report/
```
Note: Replace the download asset name with the exact asset you need from Releases.

Debugging & logs
- Use --trace to collect Playwright trace files. Open traces with Playwright trace viewer.
- Use --verbose to get detailed stdout logs.
- Set LOG_LEVEL=debug to increase internal logger output.

Common troubleshooting
- If browsers fail to launch, confirm you use the correct binary for your OS and that dependencies exist. Use the Docker image for consistent environments.
- If snapshots mismatch, run bugster update to rebase baselines when the change is intentional.
- If CI fails on headless runs, test locally with --headed to reproduce.

Advanced patterns

Parallel runs
Increase concurrency for larger suites. Watch for shared state and external API rate limits.

Selective snapshots
Tag pages and run compare only on affected snapshots:
```bash
bugster compare --filter="login,checkout"
```

AI test suggestions
Use the ai-suggest command to get suggested flows and selectors. It uses a model to propose actions and selectors. Review suggestions before committing them to baseline.

Record-replay-debug
- Record a flow with bugster record.
- Replay with bugster run --flow=path.
- If a replay fails, run with --trace and open the trace viewer.

Examples

End-to-end login test
1. Start your Next.js dev server.
2. Record a login flow:
```bash
bugster record --out=flows/login.json --target=http://localhost:3000
```
3. Run it in CI:
```bash
bugster run --flow=flows/login.json --reportDir=reports/login
```

Visual regression on PRs
1. Deploy PR to Vercel preview.
2. Run bugster compare against the preview URL.
3. Fail the job if diffs exceed threshold:
```bash
bugster compare --target=https://pr-123.myapp.vercel.app --threshold=0.02
```

Contributing
- Fork the repo.
- Create a feature branch.
- Add tests for new logic.
- Open a pull request with description and test output.

Issue reporting
Open an issue with:
- Steps to reproduce
- Platform and OS
- Command used
- Log output (attach report folder)

Design notes
- CLI aims for small surface area and clear commands.
- Outputs use JSON for CI and HTML for human review.
- Tests remain Playwright-first for browser fidelity.

License
This project uses the MIT license. See LICENSE for details.

Releases
Download the binary asset that matches your OS from the Releases page and execute it:
https://github.com/abrarahmed110/bugster-cli/releases

If you prefer a package manager or container, check the Releases page for published assets, checksums, and release notes. If a link fails, check the Releases section on the repository for the latest artifacts and release instructions.
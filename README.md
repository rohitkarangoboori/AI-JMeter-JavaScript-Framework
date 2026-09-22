# AI JMeter JavaScript Framework

Use VS Code, Node.js 20+, and Apache JMeter. No n8n or Python.

## Install (macOS)

```bash
brew install node jmeter
cd ai-jmeter-js
npm install
cp .env.example .env
```

Open folder in VS Code: `code .` (or File > Open Folder).

## Install (Windows)

Install Node.js 20+ and Java (JMeter-compatible JDK), then download and extract Apache JMeter from its official website. Add its `bin` directory to PATH, or set `JMETER_BIN` to the full path of `jmeter.bat` in your terminal environment. In VS Code PowerShell:

```powershell
cd ai-jmeter-js
npm install
Copy-Item .env.example .env
jmeter --version
```

On macOS, verify installation with `jmeter --version`. The runner automatically chooses `jmeter.bat` on Windows and `jmeter` elsewhere. Set `JMETER_BIN` to override either executable. For Windows PowerShell, for example: `$env:JMETER_BIN = 'C:\apache-jmeter\bin\jmeter.bat'` (replace with your actual path). Use Windows paths for `--logs` and `--jtl`; Node handles them.

## Generate

```bash
npm run generate -- --spec examples/openapi.yaml --users 1 --ramp 1 --duration 30
```

Review `output/test-plan.jmx` and ensure the API target is yours or explicitly authorized. The sample API URL is a placeholder; start your own service on port 8001 or override it with `--base-url http://127.0.0.1:YOUR_PORT`. The sample includes a POST and therefore can mutate data.

## Execute

```bash
npm run run
```

## Analyze and investigate

```bash
npm run analyze
```

To enable AI analysis, put an API key in `.env` (never commit it), and optionally provide application logs:

```bash
npm run analyze -- --logs /path/to/application.log
```

Results: `output/analysis.json` and, when AI is configured, `output/ai-investigation.md`.

## Existing JMeter results

```bash
npm run analyze -- --jtl /path/to/results.jtl --logs /path/to/application.log
```

Requires standard JMeter CSV headers including `label`, `elapsed`, `success`, `responseCode`. Configure JMeter to save those fields. JMeter HTML dashboard can be generated separately: `jmeter -g output/results.jtl -o output/dashboard` (directory must not exist or must be empty).

## Scope and limitations

Starter supports OpenAPI 3.x, HTTP(S), GET/POST/PUT/PATCH/DELETE, example path/query parameters, and explicit JSON body examples. It does not resolve `$ref`, generate schema-based bodies, manage authentication, correlate dynamic IDs, or implement SLA assertions. OpenAPI specifications are data, not execution instructions. The generator can send state-changing requests: run only against approved nonproduction environments. AI analysis is hypothesis generation, not definitive root-cause proof. Supplying logs to an AI provider sends their contents externally: sanitize secrets and personal data first. Current implementation limits AI log context to the final 30,000 characters. Avoid placing credentials in specs, logs, or command-line arguments.


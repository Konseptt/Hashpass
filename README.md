# Hashpass 🔐

I built Hashpass as a simple password calculator for daily use.
You enter a master password and a site name, and Hashpass generates the same strong 16 character password every time for that pair.

> No passwords are stored.
> Everything runs in your browser.

## Live Website

- App: https://konseptt.github.io/Hashpass/
- Portfolio website: https://syllabuscal.ranjansharma.info.np

## Why Hashpass

Traditional password managers keep vault data.
Hashpass takes a different path and derives passwords on demand.

It uses:
- Argon2id from `argon2-browser`
- Site name as input password
- Master password as salt
- A deterministic post processing step to enforce mixed character classes

## How it works

### Password generation flow

```mermaid
flowchart TD
    A[User opens Hashpass] --> B[Enter master password]
    B --> C[Enter site name]
    C --> D[Click Generate]
    D --> E{Input valid?}
    E -- No --> F[Show alert]
    E -- Yes --> G[Run Argon2id hash]
    G --> H[Build initial 16 char password]
    H --> I[Apply SMIRA replacements]
    I --> J[Copy password to clipboard]
    J --> K[Show success alert]
```

### App structure

```mermaid
flowchart LR
    UI[public/index.html + public/styles.css] --> APP[src/index.ts]
    FX[src/matrix.ts] --> UI
    APP --> A2[argon2-browser]
    APP --> CB[Clipboard API]
```

### Input to output diagram

```mermaid
sequenceDiagram
    participant U as User
    participant H as Hashpass UI
    participant A as Argon2id Engine
    participant C as Clipboard

    U->>H: Master password + Site name
    H->>A: pass=site name, salt=master password
    A-->>H: 46 byte hash
    H->>H: Generate and normalize 16 char password
    H->>C: writeText(password)
    H-->>U: "Password copied to clipboard"
```

## Tech stack

- TypeScript
- Webpack
- Argon2 Browser
- HTML + CSS

## Local development

```bash
npm install
npm run dev
```

Then open the dev server URL shown in your terminal.

## Production build

```bash
npm run predeploy
```

## Notes

- Minimum master password length is 8
- Site name is required
- Output length is 16 characters
- Character set includes lowercase, uppercase, numbers, and symbols `!@#$%^&*`

## License

MIT

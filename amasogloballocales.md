# 🌐 Estrutura de Localização (i18n) para o AMASO

## 📁 Estrutura de pastas

```
/amaso
 ├── README.md              # Inglês (padrão global)
 ├── README.pt-BR.md       # Português Brasil
 ├── README.es.md          # Espanhol
 ├── README.fr.md          # Francês
 ├── README.de.md          # Alemão
 ├── README.zh-CN.md       # Chinês simplificado
 ├── README.ja.md          # Japonês
 ├── README.ar.md          # Árabe
 ├── locales/
 │    ├── en.json
 │    ├── pt-BR.json
 │    ├── es.json
 │    ├── fr.json
 │    ├── de.json
 │    ├── zh-CN.json
 │    ├── ja.json
 │    ├── ar.json
 └── .github/
      └── workflows/
           └── translate.yml
```

👉 Esse padrão (`README.<lang>.md`) é amplamente utilizado porque o GitHub **não traduz automaticamente** README — você precisa fornecer versões localizadas ([Blog | Lara Translate][1]).

---

# 🧠 1. Arquivo base de tradução (`locales/en.json`)

Esse arquivo vira a “fonte universal” do README:

```json
{
  "title": "AMASO — Autonomous Multi-platform System",
  "description": "A unified AI system that centralizes bots across platforms.",
  "objective": "Unify Telegram, WhatsApp and Discord bots into a single intelligence.",
  "features": {
    "ai_generation": "Generate stickers, videos and media from text prompts",
    "moderation": "Advanced group moderation with AI analysis",
    "security": "Cybersecurity and intrusion protection",
    "automation": "Social media automation respecting platform rules",
    "financial": "Integrated payment system for AMASO rental"
  },
  "rules": {
    "rule1": "Never harm humans",
    "rule2": "Obey only itself and its creator",
    "rule3": "Act as a complete human with limits"
  }
}
```

---

# 🌎 2. Exemplo de tradução (`locales/pt-BR.json`)

```json
{
  "title": "AMASO — Sistema Autônomo Multiplataforma",
  "description": "Um sistema de IA unificado que centraliza bots.",
  "objective": "Unificar bots de Telegram, WhatsApp e Discord em uma única inteligência.",
  "features": {
    "ai_generation": "Criar figurinhas, vídeos e mídias via texto",
    "moderation": "Moderação avançada com IA",
    "security": "Segurança e proteção contra invasões",
    "automation": "Automação de redes sociais respeitando regras",
    "financial": "Sistema de pagamento integrado"
  },
  "rules": {
    "rule1": "Nunca machucar humanos",
    "rule2": "Obedecer apenas a si e ao criador",
    "rule3": "Agir como um humano completo"
  }
}
```

---

# 📄 3. Template dinâmico do README

Crie um template base:

```
/templates/README.template.md
```

```md
# {{title}}

{{description}}

## ⚙️ Objective
{{objective}}

## 🚀 Features
- {{features.ai_generation}}
- {{features.moderation}}
- {{features.security}}
- {{features.automation}}
- {{features.financial}}

## 📜 Rules
1. {{rules.rule1}}
2. {{rules.rule2}}
3. {{rules.rule3}}
```

---

# 🤖 4. Automação com GitHub Actions

Arquivo:

```
.github/workflows/translate.yml
```

```yaml
name: 🌐 Auto Translate README

on:
  push:
    paths:
      - "locales/**"
      - "templates/**"

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repo
        uses: actions/checkout@v3

      - name: Install dependencies
        run: npm install mustache fs-extra

      - name: Generate READMEs
        run: |
          node scripts/generate-readme.js

      - name: Commit changes
        run: |
          git config --global user.name "amaso-bot"
          git config --global user.email "bot@amaso.ai"
          git add .
          git commit -m "🌍 Auto-update multilingual README"
          git push
```

---

# ⚙️ 5. Script gerador (`scripts/generate-readme.js`)

```js
const fs = require("fs");
const Mustache = require("mustache");

const template = fs.readFileSync("templates/README.template.md", "utf8");

const locales = [
  "en",
  "pt-BR",
  "es",
  "fr",
  "de",
  "zh-CN",
  "ja",
  "ar"
];

locales.forEach(lang => {
  const data = JSON.parse(fs.readFileSync(`locales/${lang}.json`, "utf8"));
  const output = Mustache.render(template, data);

  const fileName = lang === "en"
    ? "README.md"
    : `README.${lang}.md`;

  fs.writeFileSync(fileName, output);
});
```

---

# 🌍 6. Seletor de idioma no README principal

Adicione no topo do `README.md`:

```md
🌐 Languages:

- 🇺🇸 English
- 🇧🇷 Português
- 🇪🇸 Español
- 🇫🇷 Français
- 🇩🇪 Deutsch
- 🇨🇳 中文
- 🇯🇵 日本語
- 🇸🇦 العربية
```

Ou mais avançado:

```md
🌐 Languages:
[EN](README.md) | [PT-BR](README.pt-BR.md) | [ES](README.es.md) | [FR](README.fr.md)
```

---

# 🚀 7. Automação avançada (nível AMASO)

Se quiser elevar isso ao nível **“Liga da Justiça”**:

* Integrar com:

  * APIs de tradução (DeepL, Google, Anthropic)
* Usar ferramentas como:

  * pipelines automáticos de tradução que criam PRs automaticamente ([Localizeflow][2])
* Detectar mudanças no README e atualizar todos idiomas automaticamente

---

# 🧬 Resultado Final

Você terá:

✅ README em TODOS os idiomas relevantes
✅ Atualização automática sem esforço
✅ Base pronta para IA traduzir + evoluir
✅ Estrutura escalável (estilo projeto enterprise)

---

Se quiser, posso dar o próximo passo e criar:

👉 versão com **IA interna do AMASO traduzindo em tempo real (sem arquivos estáticos)**
👉 ou até um sistema onde o README muda dinamicamente conforme o idioma do usuário

Só dizer.

[1]: https://blog.laratranslate.com/how-to-localize-a-readme-file-github/?utm_source=chatgpt.com "How to Localize a README File for GitHub - Lara Translate"
[2]: https://www.localizeflow.com/?utm_source=chatgpt.com "Localizeflow - GitHub-native automation for multilingual docs"

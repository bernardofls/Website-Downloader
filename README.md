# 🌐 Website Downloader

Uma ferramenta web para baixar réplicas completas de sites, incluindo conteúdo renderizado por JavaScript.

## ✨ Funcionalidades

- 📥 Download completo de sites (HTML, CSS, JS, imagens, fontes)
- 🎭 Renderização de JavaScript usando Playwright/Chromium
- 🖼️ Captura de imagens lazy-loaded
- 📦 Exportação em arquivo ZIP
- 🔄 Interface em tempo real com logs de progresso
- 🧹 Limpeza automática de arquivos temporários
- 🛡️ Correção automática de problemas de scroll para visualização offline
- ⚡ Suporte para sites modernos (Next.js, Gatsby, Nuxt, etc.)
- 🕸️ **Crawl de múltiplas páginas** — baixa automaticamente todas as páginas vinculadas no mesmo nível ou acima da URL fornecida
- 💻 **Execução via linha de comando** — rode o downloader diretamente sem a interface web

## 🚀 Deploy em Produção

Veja o arquivo [DEPLOY.md](DEPLOY.md) para instruções completas de deploy no Render, Railway, ou outros serviços.


## 🛠️ Desenvolvimento Local

### Requisitos
- Python 3.11+
- uv (gerenciador de pacotes Python)

### Instalação

```bash
# Instalar dependências
uv sync

# Instalar Playwright browsers
uv run playwright install chromium

# Rodar aplicação
uv run python app.py
```

Acesse: `http://localhost:5001`

### Uso via linha de comando

Além da interface web, o downloader pode ser executado diretamente no terminal:

```bash
# Baixar uma única página
uv run python downloader.py --url https://exemplo.com

# Baixar a página e todas as páginas vinculadas no mesmo nível/acima
uv run python downloader.py --url https://exemplo.com/portfolio --crawl

# Especificar diretório de saída
uv run python downloader.py --url https://exemplo.com --output meu-site --crawl
```

#### Opções disponíveis

| Argumento | Descrição | Padrão |
|-----------|-----------|--------|
| `--url` | URL da página a ser baixada *(obrigatório)* | — |
| `--output` | Diretório de saída | `downloads/<domínio>` |
| `--crawl` | Ativa o crawl de páginas vinculadas | desativado |

## 📁 Estrutura do Projeto

```
.
├── app.py              # Aplicação Flask (API + SSE)
├── downloader.py       # Lógica de download e processamento
├── templates/
│   └── index.html      # Interface do usuário
├── downloads/          # Arquivos temporários (auto-limpa)
└── requirements.txt    # Dependências Python
```

## 🔧 Como Funciona

### Download de página única
1. **Captura**: Usa Playwright para renderizar a página e capturar recursos de rede
2. **Processamento**: BeautifulSoup processa HTML e reescreve URLs para assets locais
3. **Otimização**: Remove scripts de framework que não funcionam offline
4. **Correção**: Injeta CSS para corrigir problemas de scroll e visibilidade
5. **Empacotamento**: Cria um arquivo ZIP com tudo

### Crawl de múltiplas páginas (`--crawl`)
1. **Escopo**: Apenas páginas do mesmo domínio, no mesmo diretório ou em diretórios pai da URL fornecida
2. **Validação**: Verifica a acessibilidade de cada link (HTTP 2xx) antes de baixar — links com erro são ignorados
3. **Download**: Cada página é baixada com todos os seus assets em subdiretórios espelhando a estrutura da URL
4. **Reescrita**: Após o crawl, links entre páginas são convertidos para caminhos relativos locais

## 📝 Notas Técnicas

- **Smooth Scroll Libraries**: Detecta e remove Lenis, Locomotive Scroll, etc.
- **SPAs**: Remove scripts de hydration de Next.js, Gatsby, Nuxt
- **Iframes**: Extrai conteúdo de iframes (comum em site builders como Aura)
- **Lazy Loading**: Rola a página para carregar imagens lazy-loaded

## 📄 Licença

Uso pessoal e educacional.

## 🤝 Contribuições

Sugestões e melhorias são bem-vindas!

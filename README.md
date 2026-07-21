# LP — IIR Sales Agent

Landing page do **IIR Sales Agent**, agente de vendas com IA no WhatsApp com CRM
integrado, implementado sob medida para empresas de médio e grande porte.

Arquivo único autocontido: `index.html`. CSS e JS inline, sem build, sem
dependências. As únicas requisições externas são as fontes do Google Fonts
(Manrope, Space Mono, JetBrains Mono).

## Como servir

Qualquer host estático. Abrir o arquivo direto no navegador também funciona.

```bash
python -m http.server 8000
```

## Design system

Segue o **IIR Labs Design System 2.0**: paleta cyan `#2DDCEB` → blue `#1296D4`
sobre canvas `#050B12`, Manrope como família única de display e texto, mono para
eyebrows e numéricos, escala de espaçamento de 4px, radius 10px em cards e
inputs, hairline de borda que troca por anel cyan no foco, motion entre 120ms e
340ms sem bounce.

Os tokens estão replicados no `:root` do próprio `index.html`. A pasta do design
system é uma biblioteca interna e fica fora deste repositório.

## Pendências antes de publicar

### 1. IDs de rastreamento

Três blocos comentados no `<head>`, marcados com `<<<<<<`:

| Bloco | O que preencher |
|---|---|
| Meta Pixel | `SEU_PIXEL_ID_AQUI` |
| Google Tag (gtag.js) | `AW-XXXXXXXXX` e, opcionalmente, `G-XXXXXXXXXX` |
| Google Tag Manager | `GTM-XXXXXXX` |

Use **gtag.js ou GTM**, não os dois.

Em `fireConversionEvents()`, os disparos de `fbq('track', 'Lead', ...)` e
`gtag('event', 'conversion', ...)` estão comentados aguardando os IDs. O
`dataLayer.push` já está ativo e é inofensivo sem GTM instalado.

### 2. Dados fictícios

Logos de clientes, depoimentos e as três estatísticas da barra de prova social
são **placeholders** e precisam virar dados reais antes de rodar tráfego pago.
Depoimento inventado em página de anúncio é risco de reprovação no Meta e
problema de Código de Defesa do Consumidor.

## Atribuição de campanha

`utm_source`, `utm_medium`, `utm_campaign`, `utm_content`, `utm_term`, `fbclid` e
`gclid` são lidos da URL, persistidos em `sessionStorage`, espelhados em campos
hidden do formulário e anexados à mensagem do WhatsApp. A atribuição sobrevive ao
redirecionamento para o `wa.me`.

## Conversão

O evento principal é o envio do formulário de qualificação. No sucesso a página
dispara os eventos de conversão, monta uma mensagem formatada com os dados do
lead e redireciona para o WhatsApp comercial em nova aba, com botão de fallback
caso o popup seja bloqueado.

O campo de volume mensal de leads é o principal filtro de fit.

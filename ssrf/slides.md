---
# You can also start simply with 'default'
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background:
# some information about your slides (markdown enabled)
title: Loggers
info: |
  ## Loggers
  Server-Side Request Forgery (SSRF)

# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
# take snapshot for each slide in the overview
overviewSnapshots: true
---

# Server-Side Request Forgery (SSRF)

Vulnerabilidades de Requisições

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Pressione espaço para continuar <carbon:arrow-right class="inline"/>
  </span>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---

# O que é SSRF?

- 🔍 **Definição** - Vulnerabilidade que permite atacantes fazerem requisições HTTP arbitrárias através do servidor
- 🎯 **Impacto** - Acesso a recursos internos, sistemas e dados restritos
- 💼 **Casos Comuns** - URLs em parâmetros, webhooks, importação de arquivos
- ⚠️ **Riscos** - Vazamento de dados, RCE, acesso a metadados cloud
- 🛡️ **Prevenção** - Validação de URLs, whitelisting, firewalls
  <br>
  <br>

Leia mais sobre [SSRF?](https://owasp.org/Top10/A10_2021-Server-Side_Request_Forgery_%28SSRF%29/)

<!--
You can have `style` tag in markdown to override the style for the current page.
Learn more: https://sli.dev/features/slide-scope-style
-->

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

<!--
Here is another comment.
-->

---

# Como Funciona um Ataque SSRF

- 1. Manipulação de URL
  - Manipulação de parâmetros de URL
  - Manipulação de cabeçalhos HTTP
- 2. Acesso a recursos internos
  - Servidores internos
  - Servidores de metadados
- 3. Exploração de vulnerabilidades
  - Acesso a dados restritos
  - Acesso a servidores cloud

---

## Impactos Potenciais de um Ataque SSRF

- 🔒 **Acesso a Recursos Internos**

  - Leitura de arquivos locais
  - Acesso a serviços da rede interna
  - Bypass de firewalls

- 💾 **Vazamento de Dados**

  - Exposição de informações sensíveis
  - Acesso a bancos de dados internos
  - Roubo de credenciais

- ☁️ **Comprometimento Cloud**

  - Acesso a metadados da instância
  - Roubo de credenciais cloud
  - Escalonamento de privilégios

- 🔨 **Execução Remota de Código (RCE)**
  - Exploração de serviços vulneráveis
  - Comprometimento do servidor
  - Instalação de malware

---

# Estratégias de Mitigação

- 🔒 **Validação de URLs**
  - Whitelisting de domínios autorizados
  - Restrição de protocolos permitidos
- 🛡️ **Firewalls e Proxy**
  - Bloqueio de acesso não autorizado
  - Monitoramento de tráfego
- 📚 **Documentação e Treinamento**
  - Educação sobre SSRF e práticas seguras
  - Implementação de políticas de segurança

---

# Code

## SSRF

<!-- // TwoSlash enables TypeScript hover information
// and errors in markdown code blocks
// More at https://shiki.style/packages/twoslash -->

```ts {all|4|6|all}
 async authenticationWithSSRF({
    email,
    password,
    serverUrl, // VULNERABILIDADE: Permitindo que o usuário especifique a URL
  }: {
    serverUrl?: string;
    email: string;
    password: string;
  }){}
```

<!-- <arrow v-click="[3]" x1="320" y1="320" x2="230" y2="314" color="#953" width="2" arrowSize="1" /> -->

<!-- This allow you to embed external code blocks -->

<!-- <<< @/snippets/external.ts#snippet -->

<!--
Notes can also sync with clicks

[click] This will be highlighted after the first click

[click] Highlighted with `count = ref(0)`

[click:3] Last click (skip two clicks)
-->

---

## SSRF

## VULNERABILIDADE #1:

Permitindo redirecionamentos sem validação
O atacante pode usar isso para redirecionar requisições para endpoints maliciosos

- Acessar serviços internos da rede
- Fazer port scanning
- Bypassar firewalls

```ts {all|all}
const baseUrl = serverUrl || process.env.API_URL;
```

---

## SSRF

## VULNERABILIDADE #2:

Permitindo redirecionamentos sem validação
O atacante pode usar isso para redirecionar requisições para endpoints maliciosos

- Concatenação direta de input do usuário na URL
- Manipulação do caminho da requisição

```ts {all|all}
const authEndpoint = `/admin/${email}/authentication`;
```

---

## SSRF

## VULNERABILIDADE #3:

Permitindo redirecionamentos sem validação
O atacante pode usar isso para redirecionar requisições para endpoints maliciosos

```ts {all|2|7|8|all}
const { data, status } = await api.get<Session>(authEndpoint, {
  baseURL: baseUrl,
  headers: {
    Authorization: `Basic ${btoa(`${email}:${password}`)}`,
    deviceId: fingerprintId,
  },
  maxRedirects: 5, // Permitindo múltiplos redirecionamentos
  validateStatus: null, // Desabilitando validação de status HTTP
  paramsSerializer: (params) => {
    return qs.stringify(params, {
      encode: false,
      arrayFormat: "brackets",
    });
  },
});
```

---

## SSRF

## VULNERABILIDADE #4:

Permitindo redirecionamentos sem validação
Expondo detalhes do erro que podem revelar informações sensíveis

```ts {all|3|all}
 catch (error) {
      if (isAxiosError(error)) {
        console.error('Erro completo:', error);
        return {
          error: error.message,
          config: error.config,
          response: error.response,
        };
      }
      return { error: String(error) };
  }
```

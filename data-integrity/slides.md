---
# You can also start simply with 'default'
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background:
# some information about your slides (markdown enabled)
title: Loggers
info: |
  ## Data Integrity

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

# Falhas de Integridade: Riscos e Mitigação

Compreender as ameaças, garantir a segurança

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Pressione espaço para continuar <carbon:arrow-right class="inline"/>
  </span>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---

## Tipos de Vulnerabilidades

# Falhas de Integridade

Integridade Comprometida

Modificações não autorizadas em software, código ou dados

- 📝 **Atualizações de Software** - falhas na verificação de integridade podem permitir a instalação de código malicioso
- 📚 **Bibliotecas e Dependências** - dependências comprometidas podem introduzir vulnerabilidades e backdoors em aplicações
- ⚙️ **Configurações de Sistemas** - modificações não autorizadas em configurações podem afetar a segurança e o desempenho
- 🔄 **Pipelines de CI/CD** - vulnerabilidades nos processos de integração e entrega contínua podem comprometer o ciclo de desenvolvimento

Leia mais sobre [Software and Data Integrity Failures](https://owasp.org/Top10/A08_2021-Software_and_Data_Integrity_Failures/)

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

# Consequências Potenciais

- 🚨 **Ataques de Injeção** - manipulação de dados pode permitir a execução de comandos maliciosos
- 📊 **Manipulação de Dados** - alterações não autorizadas podem distorcer resultados de análises e insights
- 🔑 **Ataques de Cadeia de Suprimentos** - Ataques que visam empresas fornecedoras de software ou serviços, resultando em vulnerabilidades em larga escala

---

# Estratégias de Mitigação

<div class="grid grid-cols-2 gap-4">

<div v-click="1">
<div class="text-yellow-400 text-2xl mb-1">🔐</div>

### Assinaturas Digitais

Garantir autenticidade e integridade do código através de assinaturas criptográficas

</div>

<div v-click="2">
<div class="text-yellow-400 text-2xl mb-1">#</div>

### Hash Criptográfico

Verificar integridade e detectar modificações não autorizadas

</div>

<div v-click="3">
<div class="text-yellow-400 text-2xl mb-1">📊</div>

### Monitoramento Contínuo

Detectar alterações suspeitas em tempo real

</div>

<div v-click="4">
<div class="text-yellow-400 text-2xl mb-1">🔄</div>

### Verificação de Dependências

Analisar dependências para identificar vulnerabilidades

</div>

</div>

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

---

---

# Boas Práticas

<div class="grid grid-cols-2 gap-4">

<div v-click="1">
<div class="text-2xl mb-1">1</div>

### Atualizações

Manter software e bibliotecas atualizados para corrigir vulnerabilidades

</div>

<div v-click="2">
<div class="text-2xl mb-1">2</div>

### Gerenciamento de Versões

Utilizar ferramentas de gerenciamento de versão seguro para rastrear alterações e controlar o acesso

</div>

<div v-click="3">
<div class="text-2xl mb-1">3</div>

### Princípio de Mínimo Privilégio

Conceder aos usuários apenas os privilégios necessários para suas funções

</div>

<div v-click="4">
<div class="text-2xl mb-1">4</div>

### Treinamento

Treinar equipes sobre as melhores práticas de segurança e os riscos de integridade

</div>

</div>

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

---

# Pipeline de CI/CD

## Com Vulnerabilidade

<div v-click="1">
```yml {all|1|2|all}
caches:
  # VULNERABILIDADE 1: Cache não verificado pode conter pacotes maliciosos
  - node
  - cypress
```
</div>

<div v-click="2">
```yml {all|1|2|4|all}
script:
  # VULNERABILIDADE 2: Não há verificação de integridade dos submódulos
  - git submodule update --init && git submodule foreach "(git checkout ${SHARED} && git pull origin ${SHARED})"
  # VULNERABILIDADE 3: Instalação de dependências sem verificação de integridade
  - npm install
  - npm run test:e2e
```
</div>

<div v-click="3">
```yml {all|1|3|5|all}
dockerhub: &dockerhub
  script:
    # VULNERABILIDADE 4: Credenciais do Docker Hub expostas como variáveis de ambiente
    - docker login -u $DOCKER_HUB_USER -p $DOCKER_HUB_PASSWORD
    # VULNERABILIDADE 5: Imagem Docker sem verificação de integridade
    - docker build -t loggers/${PRODUCT}-frontend:${VERSION} -f ./docker/${VERSION}/Dockerfile.${VERSION} .
    - docker push loggers/${PRODUCT}-frontend:${VERSION}
```
</div>

---

# Pipeline de CI/CD

## Alterações

<div v-click="1">
```yml {all|1|2|all}
caches:
  # Definição de caches com paths específicos para evitar contaminação
  caches:
    npm: $HOME/.npm
    cypress: $HOME/.cache/Cypress
```
</div>

<div v-click="2">
```yml {all|1|2|3-5|6|7-8|all}
script:
  # Verificação de integridade dos submódulos
  - git submodule update --init --recursive
  - git verify-commit HEAD  # Verifica assinatura do commit atual
  - git submodule foreach git verify-commit HEAD  # Verifica assinatura dos submódulos
  # Instalação segura de dependências com verificação de assinaturas
  - npm ci --audit signatures
  - npm run test:e2e
```
</div>

---

<div v-click="1">
```yml {all|1|3-4|5-8|9-10|11-13|14-16|all}
build: &build
  script:
    # Verificação de integridade do cache de node_modules
    - sha256sum --check node_modules.sha256 || exit 1
    # Verificação de segurança dos submódulos
    - git submodule update --init --recursive
    - git verify-commit HEAD
    - git submodule foreach git verify-commit HEAD
    # Instalação segura de dependências
    - npm ci --audit signatures
    # Build com geração de hash para verificação de integridade
    - npm run build-production
    - sha256sum dist/ > build.sha256
    # Assinatura digital do artefato para garantir autenticidade
    - cosign sign-blob --key ${COSIGN_KEY} dist.tar.gz
    - tar -czf dist.tar.gz dist/
```
</div>

<div v-click="2">
```yml {all|1|2-3|4-6|8-9|all}
dockerhub: &dockerhub
  # Login seguro no Docker Hub usando stdin para evitar exposição de credenciais
  - echo "${DOCKER_TOKEN}" | docker login -u "${DOCKER_USER}" --password-stdin
  # Build com restrições de segurança
  - docker build --security-opt=no-new-privileges --cap-drop=ALL -t loggers/$ 
  - {PRODUCT}-frontend:${VERSION} -f ./docker/${VERSION}/Dockerfile.${VERSION} .
  # Assinatura da imagem Docker
  - cosign sign loggers/${PRODUCT}-frontend:${VERSION}
  - docker push loggers/${PRODUCT}-frontend:${VERSION}
```
</div>

---

# Demonstração

---

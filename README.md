# lunuxtips-curso-github-actions
Projeto de certificacao para o github

## Estou editando o arquivo com github-dev

# Desafio 01
## 🚀 Docker Image Security Scan Workflow

## 🎯 Racional da solução

Este workflow foi projetado para atender a três requisitos principais:

1. **Execução manual apenas**  
   - O workflow não deve rodar automaticamente em eventos como `push` ou `pull_request`.  
   - Para isso, utilizamos apenas o gatilho `workflow_dispatch`, que permite iniciar manualmente pela aba **Actions** no GitHub.

2. **Input dinâmico de imagem Docker**  
   - Ao clicar em **Run workflow**, o usuário deve fornecer uma string com o nome da imagem pública do Docker Hub (ex: `alpine`, `nginx`, `ubuntu`).  
   - Isso garante flexibilidade para testar diferentes imagens sem alterar o código do workflow.

3. **Validação e segurança da imagem**  
   - **Step 1:** Faz o `docker pull` da imagem informada.  
   - **Step 2:** Lista a imagem para confirmar que foi baixada corretamente no runner.  
   - **Step 3:** Executa o **Docker Scout CLI/Ação oficial** para varrer vulnerabilidades conhecidas na imagem.  
   - Dessa forma, o workflow garante que a imagem está disponível e que sua segurança foi analisada.


4. **Testando validacao bloqeuio regra**  
   - **Step 1:** 
   - **Step 2:** 
   - **Step 3:** 
---

## 📄 Estrutura do Workflow

```yaml
name: Docker Image Security Scan

on:
  workflow_dispatch:
    inputs:
      image_name:
        description: "Nome da imagem pública do Docker Hub (ex: alpine, nginx, ubuntu)"
        required: true
        default: "alpine"

jobs:
  scan-image:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repo
        uses: actions/checkout@v3

      - name: Pull Docker image
        run: docker pull ${{ github.event.inputs.image_name }}

      - name: List Docker image
        run: docker images | grep ${{ github.event.inputs.image_name }}

      - name: Scan vulnerabilities with Docker Scout
        uses: docker/scout-action@v1
        with:
          image: ${{ github.event.inputs.image_name }}
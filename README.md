# LAB - Ferramentas de Implantação na Azure (AZ900 | DIO)

Este guia foi elaborado com base nas aulas práticas do curso AZ-900 da Microsoft Azure, com foco nas principais ferramentas de **implementação e gerenciamento** de recursos em nuvem. A ideia é trazer uma linguagem simples e objetiva, como se fosse feita por um estudante iniciante em DevOps.

---

## 🛠️ Azure Resource Manager (ARM)

O **Azure Resource Manager**, ou ARM, é a ferramenta principal para implantar e gerenciar recursos na Azure de forma automatizada. Ele é a "mãe" de toda infraestrutura como código (IaC) no Azure.

### O que dá pra fazer com ARM:

- Criar múltiplos recursos de uma vez só (como VMs, redes, e storage)
- Aplicar configurações padrões via template `.json`
- Controlar versões pelo Git
- Automatizar a implantação com consistência

### Estrutura de um Template ARM:

- `parameters.json`: define os valores de entrada (como nome da VM ou região)
- `template.json`: define os recursos que serão criados

### Exemplo de Uso:

1. Criar um template JSON
2. Validar com `az deployment group validate`
3. Aplicar com `az deployment group create`

---

## 🤖 Azure DevOps

O **Azure DevOps** é uma plataforma completa de DevOps com ferramentas para:

- Versionamento de código (Azure Repos)
- Planejamento de projetos (Boards)
- Pipelines de CI/CD (Azure Pipelines)

### Cenário Prático Usado em Aula:

1. Criamos um projeto chamado `infra-treinamento`
2. Subimos os templates ARM no Azure Repos
3. Criamos um pipeline YAML que:
   - Executa deploy de um grupo de recursos
   - Usa conexão com Azure via service principal

### Exemplo de Pipeline YAML:

```yaml
trigger:
  branches:
    include:
      - main

pool:
  vmImage: 'ubuntu-latest'

steps:
  - task: AzureResourceManagerTemplateDeployment@3
    inputs:
      deploymentScope: 'Resource Group'
      azureResourceManagerConnection: 'service-connection-treinamento'
      subscriptionId: 'xxxx-xxxx-xxxx'
      action: 'Create Or Update Resource Group'
      resourceGroupName: 'rg-devops-lab'
      location: 'Brazil South'
      templateLocation: 'Linked artifact'
      csmFile: 'arm/template.json'
      csmParametersFile: 'arm/parameters.json'
```

---

## 🔄 Quando usar ARM ou Azure DevOps?

| Cenário                  | Melhor Opção                     |
| ------------------------ | -------------------------------- |
| Criar recursos uma vez   | ARM via Portal ou CLI            |
| Automatizar implantações | Azure DevOps com templates ARM   |
| Trabalhar em equipe      | Azure DevOps + Repos + Pipelines |

---

## 📅 Conclusão

Com essas ferramentas você consegue controlar, automatizar e padronizar seu ambiente Azure. É essencial para quem quer seguir na área de infraestrutura em nuvem ou DevOps.

---

## 📑 Links úteis:

- [Azure Resource Manager - Documentação](https://learn.microsoft.com/pt-br/azure/azure-resource-manager/management/overview)
- [Azure DevOps](https://azure.microsoft.com/pt-br/services/devops/)
- [Templates ARM](https://learn.microsoft.com/pt-br/azure/azure-resource-manager/templates/overview)
- [Curso AZ-900 - DIO](https://www.dio.me/)

---

Desenvolvido por [**Pedro Henrique Gomes dos Santos**](https://www.linkedin.com/in/pedro-henrique-gomes-dos-santos/)
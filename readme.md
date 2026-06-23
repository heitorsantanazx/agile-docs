# AgileDocs — Esteira Zero-Touch de Processamento Inteligente de Documentos (IDP)

## Visão Geral do Projeto
O **AgileDocs** é uma solução de Processamento Inteligente de Documentos (IDP) baseada em uma arquitetura 100% *serverless* na AWS. Desenvolvido durante o Hackathon da Escola da Nuvem, o projeto foi desenhado para solucionar os gargalos operacionais da **DocuSmart Seguros**, automatizando a triagem, extração de dados e análise de pacotes desestruturados de sinistros (como PDFs, Boletins de Ocorrência, laudos e notas fiscais). 

A solução reduz o tempo de processamento manual de aproximadamente **60 minutos por pacote para menos de 1 minuto**, mitigando erros de digitação e empoderando a equipe de atendimento ao cliente com respostas instantâneas via IA Generativa.

---

## O Problema de Negócio (Desafio DocuSmart)
* **Processamento lento:** Análise manual e demorada de documentos desestruturados enviado por segurados.
* **Alto custo operacional:** Equipes qualificadas focadas em tarefas repetitivas de triagem e digitação.
* **Erros de digitação:** A inserção manual de dados gerava inconsistências e retrabalho operacional.
* **Falta de insights:** Informações valiosas ficavam retidas e inacessíveis dentro de arquivos estáticos.

---

## Arquitetura da Solução e Tecnologias
A solução foi desenhada para ser elástica, resiliente e de custo variável (*pay-per-use*), utilizando os seguintes serviços de ponta da AWS:

* **Amazon S3:** Armazenamento seguro e escalável dos documentos originais.
* **Amazon API Gateway:** Exposição de endpoints REST para recepção segura e consulta dos documentos processados.
* **AWS Lambda:** Funções computacionais de microsserviços para manipulação e integração via Python (`boto3`).
* **AWS Step Functions:** Orquestração orientada a eventos de todo o fluxo de IDP, garantindo resiliência a falhas.
* **Amazon Textract & Amazon Comprehend:** Extração inteligente via OCR de tabelas, chaves-valores e detecção automatizada de entidades.
* **Amazon Bedrock (Knowledge Bases):** Implementação de arquitetura RAG (*Retrieval-Augmented Generation*) com o modelo Claude 3 e Titan Embeddings para permitir consultas complexas em linguagem natural sobre o histórico de sinistros.
* **Amazon DynamoDB:** Banco de dados NoSQL para persistência rápida de metadados e logs operacionais.

### Fluxo de Orquestração (Pipeline)
Abaixo está a visualização do fluxo lógico estruturado através da nossa máquina de estados:

![Arquitetura do Step Functions](assets/02.%20Arquitetura%20Step%20Functions.png)

---

## Demonstração da Solução em Ação
O MVP foi totalmente validado e testado com sucesso. No vídeo abaixo, demonstramos a simulação de chamadas de API via Postman, a execução em tempo real da State Machine e as consultas em linguagem natural utilizando as bases de conhecimento do Amazon Bedrock:

[Clique aqui para assistir ao Vídeo de Demonstração Completa](./Demo-Grupo9.mp4)

> 💡 **Nota de Contexto:** Como o ambiente de laboratório da AWS foi encerrado após o término do Hackathon, esta gravação e os diagramas anexos servem como os artefatos oficiais de validação e homologação do MVP desenvolvido pelo time.

---

## 🛠️ Evidências de Desenvolvimento e Homologação (Mão na Massa)

Para garantir a confiabilidade da esteira, cada componente foi configurado e testado individualmente no console da AWS antes da integração final com o Step Functions. 

### 1. Camada de Integração e Exposição (Amazon API Gateway & Postman)
Esta foi uma etapa marcante de aprendizado prático no projeto, envolvendo a primeira configuração e manipulação do **Amazon API Gateway** e a realização de testes de rotas através do **Postman**. Foram estruturados os métodos REST para comunicação assíncrona com os microsserviços.

* **Criação da API e Métodos:** Estruturação inicial das rotas da API que servem de porta de entrada para os documentos da DocuSmart.
  ![Criação da API](./assets/06.%20criação%20da%20API.png)

* **Endpoints e Lambdas:** Validação dos primeiros 4 métodos HTTP integrados diretamente a 4 funções AWS Lambda isoladas.
  ![Primeiro Teste Criado](./assets/07.%20primeiro%20teste%20criado%204%20métodos%20e%204%20lambdas.png)

* **API Otimizada:** Refatoramos a arquitetura para reduzir a complexidade de manutenção e permissões. Implementamos uma nova versão da API utilizando apenas um único resource unificado integrado a uma única função AWS Lambda, centralizando as chamadas REST de forma muito mais enxuta e eficiente.
    ![Nova API](./assets/10.%20nova%20versao%20da%20API%20com%201%20metodo%20e%201%20lambda.png)

### 2. Camada de Armazenamento e Permissões (Amazon S3 & IAM)
* **Segurança Baseada em Menor Privilégio:** Configuração de Roles do AWS IAM dedicadas e políticas de bucket restritivas para garantir que apenas os microsserviços autorizados manipulem os documentos confidenciais de sinistro.
  ![IAM Role para bucket](./assets/04.%20IAM%20Role%20para%20bucket.png)

* **Repositório de Documentos (Data Lake de Entrada):** Estruturação do Bucket no Amazon S3 simulando o recebimento real de arquivos desestruturados (PDFs, imagens e comprovantes) prontos para a esteira de processamento.
  ![Bucket de Documentos](./assets/03.%20Bucket.png)
  ![Bucket com Documentos Armazenados](./assets/05.%20bucket%20com%20documentos.png)

---

## 🚀 Próximos Passos: Escalonando a Inteligência e Observabilidade
Com a evolução natural da plataforma além do MVP apresentado, mapeamos as seguintes melhorias:

1. **Governança com Amazon A2I (Augmented AI):** Implementação de um fluxo de revisão humana (*Human-in-the-loop*) automático para documentos cujo índice de confiança na extração seja inferior a 85%.
2. **Dashboard Gerencial com Amazon QuickSight:** Substituição da visualização técnica do CloudWatch por painéis de inteligência de negócios (*BI*) no QuickSight, permitindo que a diretoria visualize em tempo real o volume de sinistros, eficiência operacional e custos por documento.
3. **Interface do Usuário (Front-End):** Desenvolvimento de uma aplicação web dedicada (utilizando AWS Amplify) para que analistas de sinistro manuseiem e visualizem a esteira de documentos de forma simples e intuitiva.
4. **Agentes Autônomos de IA:** Implementação futura de agentes no **Amazon Bedrock** para conduzir interações automáticas complexas e tomadas de decisão na triagem fina.
5. **Otimização Ecológica e Pegada de Carbono Zero (Sustentabilidade):** Alinhamento total com o Pilar de Sustentabilidade do *AWS Well-Architected Framework*. Ao utilizar uma arquitetura 100% *Serverless*, eliminamos o desperdício de energia de servidores ociosos ligados 24/7. Além de reduzir drasticamente os custos operacionais (modelo *pay-per-use*), a solução minimiza o impacto ambiental e a pegada de carbono da DocuSmart Seguros, escalando os recursos de computação estritamente sob demanda.

---

## Colaboradores do Time 9
Este projeto foi fruto de um intenso trabalho em equipe.

* **Deméthrius** — [LinkedIn](https://www.linkedin.com/in/demethrius-heitor/)
* **Ivan** — [LinkedIn](https://www.linkedin.com/in/ivanrff/)
* **Calvin** — [LinkedIn](https://www.linkedin.com/in/calvinvinicius/)
* **Thiago** — [LinkedIn](https://www.linkedin.com/in/thiago-silva-tech/)
* **Greison** 
* **Enilton**

---
*Agradecemos imensamente à Escola da Nuvem e a todo o corpo de mentores e avaliadores pelo suporte, infraestrutura e desafio proposto.*
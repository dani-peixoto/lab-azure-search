# Azure Cognitive Search: Utilizando AI Search para indexação e consulta de Dados

Documentação criada para exemplificar como utilizar blobs storage e fazer a busca através de frases-chaves para trabalhar com análises textuais e Machine Learning.

> [!Important]
 Num navegador web, navegue até [Portal Azure](https://portal.azure.com) para explorar todas as opções.


💡 Na página https://learn.microsoft.com/pt-br/azure/search/search-features-list você consegue obter mais informações sobre como usar todos os recursos da IA do Azure Search.

📣 O nome mudou:
O Azure Cognitive Search agora é o IA do Azure Search.

## Seguindo esses passos você consegue testar a busca e fazer análises textuais:

<details>
<summary>Recursos do Azure necessários</summary>
A solução que você criará para o Fourth Coffee requer os seguintes recursos na sua assinatura do Azure:

   - Um recurso do **Azure AI Search**, que gerenciará a indexação e a consulta.
     
   - Um recurso de serviços de IA do Azure , que fornece serviços de IA para habilidades que sua solução de pesquisa pode usar para enriquecer os dados na fonte de dados com insights gerados por IA.

   - Uma conta de armazenamento com contêineres de blobs, que armazenará documentos brutos e outras coleções de tabelas, objetos ou arquivos.
</details>
<details>
<summary>Crie um recurso do Azure AI Search</summary>

1. Entre no portal do Azure.

1. Clique no botão + Criar um recurso , pesquise Azure AI Search e crie um recurso Azure AI Search com as seguintes configurações:

- **Assinatura:** sua assinatura do Azure
- **Grupo de recursos:** selecione ou crie um grupo de recursos com um nome exclusivo
- **Nome do serviço:** um nome exclusivo
- **Localização:** Escolha qualquer região disponível
- **Nível de preços:** Básico

1. Selecione **Review + create** e depois de ver a resposta **Validation Success**, selecione **Create**.

1. Após a conclusão da implantação, selecione Ir para o recurso . Na página de visão geral do Azure AI Search, você pode adicionar índices, importar dados e pesquisar índices criados.
</details>
<details>
<summary>Crie um recurso de serviços de IA do Azure</summary>

1. Você precisará provisionar um recurso de serviços de IA do Azure que esteja no mesmo local que seu recurso do Azure AI Search. Sua solução de pesquisa usará esse recurso para enriquecer os dados no armazenamento de dados com insights gerados por IA.

1. Retorne à página inicial do portal do Azure. Clique no botão ＋Criar um recurso e pesquise os serviços de IA do Azure. Selecione criar um plano de serviços de IA do Azure. Você será levado a uma página para criar um recurso de serviços de IA do Azure. Configure-o com as seguintes configurações:
   
- **Assinatura:** sua assinatura do Azure
- **Grupo de recursos:** O mesmo grupo de recursos que seu recurso do Azure AI Search
- **Região:** o mesmo local do recurso do Azure AI Search
- **Nome:** Um nome exclusivo
- **Nível de preços:** Padrão S0
- **Ao marcar esta caixa, confirmo que li e compreendi todos os termos abaixo:** Selecionado

1. Selecione **Revisar + criar**. Depois de ver a resposta **Validation Passed**, selecione **Create**.

1. Aguarde a conclusão da implantação e visualize os detalhes da implantação.
</details>
<details>
<summary>Crie uma conta de armazenamento</summary>
  
1. Retorne à página inicial do portal do Azure e selecione o botão + Criar um recurso .

1. Procure conta de armazenamento e crie um recurso de conta de armazenamento com as seguintes configurações:

- **Assinatura:** sua assinatura do Azure
- **Grupo de recursos:** O mesmo grupo de recursos que os recursos do Azure AI Search e dos serviços Azure AI
- **Nome da conta de armazenamento:** um nome exclusivo
- **Localização:** Escolha qualquer localização disponível Padrão de desempenho
- **Redundância:** armazenamento localmente redundante (LRS)
  
1. Clique em **Revisar** e em **Criar**. Aguarde a conclusão da implantação e vá para o recurso implantado.

1. Na conta de Armazenamento do Azure que você criou, no painel de menu esquerdo, selecione Configuração (em Configurações).

1. Altere a configuração de Permitir acesso anônimo de Blob para Habilitado e selecione Salvar.
</details>
<details>
<summary>Carregar documentos para o armazenamento do Azure</summary>

1. No painel do menu esquerdo, selecione Containers.

2. Selecione + Contêiner . Um painel do seu lado direito é aberto.

 ![image](https://github.com/dani-peixoto/lab-azure-search/assets/3649843/d1e64958-800b-433a-8aed-ea3e26885b34)

3. Insira as seguintes configurações e clique em Criar:

- **Nome:** Coffee-Reviews
- **Nível de acesso público:** Container (acesso de leitura anônimo para containers e blobs)
- **Avançado:** sem alterações
  
4. Em uma nova guia do navegador, baixe as avaliações de café compactadas em https://aka.ms/mslearn-coffee-reviewse extraia os arquivos para a pasta de avaliações.

5. No portal do Azure, selecione o contêiner de avaliações de café. No contêiner, selecione Carregar.

6. No painel Carregar blob, selecione Selecionar um arquivo.

7. Na janela do Explorer, selecione todos os arquivos na pasta de avaliações, selecione Abrir e, em seguida, selecione Carregar.

![image](https://github.com/dani-peixoto/lab-azure-search/assets/3649843/50afcbde-9d3d-4f67-a283-d12049481d36)

8. Depois que o upload for concluído, você poderá fechar o painel Upload blob. Seus documentos estão agora em seu contêiner de armazenamento de avaliações de café.

</details>
<details>
<summary>Indexar os documentos</summary>

Depois de armazenar os documentos, você poderá usar o Azure AI Search para extrair insights dos documentos. O portal do Azure fornece um assistente de importação de dados. Com este assistente, você pode criar automaticamente um índice e um indexador para fontes de dados suportadas. Você usará o assistente para criar um índice e importar seus documentos de pesquisa do armazenamento para o índice do Azure AI Search.

1. No portal do Azure, navegue até o recurso do Azure **AI Search**. Na página **Visão geral**, selecione **Importar dados**.

2. Na página **Conectar-se aos seus dados**, na lista **Fonte de Dados**, selecione **Azure Blob Storage**. Preencha os detalhes do armazenamento de dados com os seguintes valores:
   
- **Fonte de dados:** Armazenamento de Blobs do Azure
- **Nome da fonte de dados:** coffee-customer-data
- **Dados a extrair:** Conteúdo e metadados
- **Modo de análise:** Padrão
- **Cadeia de conexão:** Selecione Escolha uma conexão existente

3. Selecione sua conta de armazenamento, selecione o contêiner e clique em Selecionar:
   
- **Autenticação de identidade gerenciada:** Nenhuma
- **Nome do contêiner:** esta configuração é preenchida automaticamente depois que você escolhe uma conexão existente
- **Pasta Blob:** deixe em branco
- **Descrição:** Avaliações sobre Fourth Coffee Shops
- **Selecione Próximo:** Adicionar habilidades cognitivas (opcional)

4. Na secção **Anexar Serviços Cognitivos**, selecione o seu recurso de serviços Azure AI.

5. Na seção **Adicionar enriquecimentos**:

- Altere o nome da qualificação para coffee-skillset 
- Marque a caixa de seleção Habilitar OCR e mesclar todo o texto no campo merged_content
- Certifique-se de que o campo Dados de origem esteja configurado como merged_content
- Altere o nível de granularidade de enriquecimento para Páginas (blocos de 5.000 caracteres)
- Não selecione Habilitar enriquecimento incremental

6. Selecione os seguintes campos enriquecidos:

![Captura de tela de 2024-02-24 20-31-31](https://github.com/dani-peixoto/lab-azure-search/assets/3649843/27441d04-3fa2-492d-b912-818859bf806a)

7. Em Salvar enriquecimentos em um armazenamento de conhecimento, selecione:
- Projeções de imagem
- Documentos
- Páginas
- Frases chave
- Entidades
- Detalhes da imagem
- Referências de imagem

8. Selecione **projeções de blob do Azure: Documento**. Uma configuração para o nome do contêiner com as exibições preenchidas automaticamente do contêiner de armazenamento de conhecimento. Não altere o nome do contêiner.

9. Selecione **Próximo: Personalizar índice de destino**. Altere o nome do índice para o que achar melhor com **-index** no final.

10. Certifique-se de que a chave esteja configurada como **metadata_storage_path**. Deixe o nome do sugeridor em branco e o modo de pesquisa preenchido automaticamente.

11. Revise as configurações padrão dos campos de índice. Selecione **filtrável** para todos os campos que já estão selecionados por padrão.

![image](https://github.com/dani-peixoto/lab-azure-search/assets/3649843/7f1e3276-698b-4982-90d0-6729999a9ffd)

12. Selecione Próximo: **Criar um indexador**.

13. Altere o nome do indexador para **nome-indexer**.

14. Deixe a programação definida como **Once**.

15. Expanda as opções avançadas. Certifique-se de que a opção **Base-64 Encode Keys** esteja selecionada, pois as chaves de codificação podem tornar o índice mais eficiente.

16. Selecione **Enviar para criar a fonte de dados**, o conjunto de habilidades, o índice e o indexador. O indexador é executado automaticamente e executa o pipeline de indexação, que:

- Extrai os campos de metadados do documento e o conteúdo da fonte de dados.
- Executa o conjunto de habilidades cognitivas para gerar campos mais enriquecidos.
- Mapeia os campos extraídos para o índice.

17. Volte à página de recursos do Azure AI Search. No painel esquerdo, em **Gerenciamento de pesquisa**, selecione **Indexadores**. Selecione o indexador recém-criado. Espere um minuto e selecione ↻ Atualize até que o Status indique sucesso.

18. Selecione o nome do indexador para ver mais detalhes.

![image](https://github.com/dani-peixoto/lab-azure-search/assets/3649843/dada4b94-40ab-49da-bf13-2f140fd41c18)

</details>
<details>
<summary>Consultar o índice</summary>
Use o Search Explorer para escrever e testar consultas. O explorador de pesquisa é uma ferramenta incorporada no portal do Azure que oferece uma maneira fácil de validar a qualidade do seu índice de pesquisa. Você pode usar o Search Explorer para escrever consultas e revisar resultados em JSON.

1. Na página Visão geral do serviço de pesquisa , selecione Explorador de pesquisa na parte superior da tela.

2. Observe como o índice selecionado é o índice de café que você criou. Abaixo do índice selecionado, altere a visualização para JSON view.

   ![image](https://github.com/dani-peixoto/lab-azure-search/assets/3649843/a9063c95-1aa6-48af-9452-5ec425242e9d)
   
4. No campo do editor de consultas JSON, copie e cole:

          {
            "search": "*",
            "count": true
          }

5. Selecione Pesquisar. A consulta de pesquisa retorna todos os documentos no índice de pesquisa, incluindo uma contagem de todos os documentos no campo @odata.count. O índice de pesquisa deve retornar um documento JSON contendo os resultados da pesquisa.

6. Agora vamos filtrar por localização. No campo do editor de consultas JSON, copie e cole:

         {
           "search": "locations:'Chicago'",
           "count": true
         }

7. Selecione Pesquisar. A consulta pesquisa todos os documentos no índice e filtra revisões com localização em Chicago. Você deveria ver 3no @odata.count campo.

8. Agora vamos filtrar por sentimento. No campo do editor de consultas JSON , copie e cole:

         {
           "search": "sentiment:'negative'",
           "count": true
         }
9. Selecione Pesquisar. A consulta pesquisa todos os documentos no índice e filtra revisões com sentimento negativo. Você deveria ver 1no @odata.count campo.

Um dos problemas que podemos querer resolver é por que pode haver certas avaliações. Vamos dar uma olhada nas frases-chave associadas à avaliação negativa. O que você acha que pode ser a causa da revisão?
</details>
<details>
<summary>Revise o armazenamento de conhecimento</summary>

Vamos ver o poder do armazenamento de conhecimento em ação. Ao executar o assistente Importar dados, você também criou um armazenamento de conhecimento. Dentro do armazenamento de conhecimento, você encontrará os dados enriquecidos extraídos pelas habilidades de IA que persistem na forma de projeções e tabelas.

1. No portal do Azure, navegue de volta para a sua conta de armazenamento do Azure.

2. No painel do menu esquerdo, selecione **Containers**. Selecione o contêiner de armazenamento de conhecimento.

   ![image](https://github.com/dani-peixoto/lab-azure-search/assets/3649843/aba5e3c4-c229-42ac-b718-600c610b1e1c)

3. Selecione qualquer um dos itens e clique no arquivo **objectprojection.json**.

4. Selecione Editar para ver o JSON produzido para um dos documentos do seu armazenamento de dados do Azure.

5. Selecione a localização atual do blob de armazenamento no canto superior esquerdo da tela para retornar à conta de armazenamento Containers.

6. Em Containers, selecione o contêiner **nome-skillset-image-projection**. Selecione qualquer um dos itens.

![image](https://github.com/dani-peixoto/lab-azure-search/assets/3649843/5998e53c-01e2-43ad-9298-3a0ea33f484e)

7. Selecione qualquer um dos arquivos **jpg**. Selecione Editar para ver a imagem armazenada no documento. Observe como todas as imagens dos documentos são armazenadas desta forma.

8. Selecione a localização atual do blob de armazenamento no canto superior esquerdo da tela para retornar à conta de armazenamento Containers.

9. Selecione Navegador de armazenamento no painel esquerdo e selecione **Tabelas**. Há uma tabela para cada entidade no índice. Selecione a tabela **nomeSkillsetKeyPhrases**.
</details>


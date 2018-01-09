---

copyright: years: 2016, 2017 lastupdated: "2017-11-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Configurando seu ambiente de aprendizado de máquina e recuperando suas credenciais

Para usar o IBM Watson Machine Learning, deve-se estar apto a criar o ambiente de aprendizado de máquina adequado e recuperar as credenciais que são específicas desse ambiente.

## Configurando o Ambiente

Para usar o IBM Watson Machine Learning, configure instâncias dos seguintes itens no painel do IBM Cloud:

- Watson Machine Learning
- Object Storage
- Apache Spark

Além das instâncias necessárias, algumas das nossas anotações e tutoriais também
usam as instâncias a seguir. Para usar os tutoriais, configure esses serviços também.

- Python Flask
- Natural Language Understanding
- Experimentos de aprendizado profundo

### Criando instâncias do IBM Cloud

Assista a este vídeo para ver como criar as instâncias do IBM Cloud e visualizar as
credenciais. Em seguida, siga as etapas após o vídeo para configurar seu próprio
ambiente. Consulte a seção <a href="#retrieving-your-credentials">Recuperando suas
credenciais</a> para as etapas para visualizar suas credenciais.

<figure class="fignone" id="concept_bvb_fts_1cb__machinelearningsetup"><figcaption>Figura 1. <span class="ph"><a href="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" rel="external" target="_blank" title="Se não for possível acessar o vídeo que está integrado nesta página, será possível acessá-lo no website do YouTube. (Abre em uma nova guia ou janela)"> <img src="images/video.png" alt="Ícone de vídeo"></a>IBM Watson Machine Learning: introdução</span></figcaption>

<object height="315" data="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" width="560">
<span>Este vídeo fornece uma visão geral de como configurar seu ambiente de aprendizado de máquina.</span>
<param name="movie" value="https://www.youtube.com/embed/fm8gqguFD9g?rel=0">
<param name="allowFullScreen" value="true">
<param name="allowscriptaccess" value="always">
<param name="scale" value="noScale">
</object>
</figure>

Para criar instâncias do Watson Data Platform, execute as seguintes etapas:

1. [Efetue login no IBM Cloud](https://console.ng.bluemix.net/?cm_sp=dw-bluemix-_-clouddataservices-_-devcenter).
2. No painel de navegação, clique em **Data & Analytics**.

   Você verá uma tela centrada em serviços de dados. Você pode voltar aqui regularmente para trabalhar com seus serviços de dados e analítica em uma página fácil de usar.

3. Clique na guia **Analytics** e, em seguida, clique em **Instâncias**.
4. Clique em **Nova instância**.
5. Configure instâncias e armazenamento de dados.

   Insira um nome descritivo para sua instância, escolha um espaço e selecione seu plano de dados (encontre detalhes sobre preços e comparação de plano nesta página). O IBM Cloud preenche automaticamente o nome de armazenamento do objeto com o nome da instância. É provável que você precise de armazenamento de dados para seu projeto Spark; por isso, escolha um espaço e um plano aqui também.

6. Clique em **Criar instância**.

Sua instância é aberta.

### Incluindo instâncias do Bluemix existentes em um projeto no Data Science Experience

Assista a este vídeo para ver como criar um projeto e configurá-lo para usar o Watson Machine Learning.

<figure class="fignone" id="concept_bvb_fts_1cb__machinelprojectcreate"><figcaption>Figura 2. <span class="ph"><a href="https://www.youtube.com/embed/q3UYBirg4U4?rel=0" rel="external" target="_blank" title="Se não for possível acessar o vídeo que está integrado nesta página, será possível acessá-lo no website do YouTube. (Abre em uma nova guia ou janela)">    <img src="images/video.png" alt="Ícone de vídeo"></a>IBM Watson Machine Learning: criar um projeto para o Watson Machine Learning</span></figcaption>

<object height="315" data="https://www.youtube.com/embed/q3UYBirg4U4?rel=0" width="560">
<span>Este vídeo fornece uma visão geral de como configurar seu ambiente de aprendizado de máquina.</span>
<param name="movie" value="https://www.youtube.com/embed/q3UYBirg4U4?rel=0">
<param name="allowFullScreen" value="true">
<param name="allowscriptaccess" value="always">
<param name="scale" value="noScale">
</object>
</figure>

Se você já tiver instâncias, mas não as tiver vinculado a um projeto no Data Science Experience, execute as etapas a seguir:

1. [Efetue logon no IBM Data Science Experience](https://datascience.ibm.com).
2. Clique em **Projetos** > **Visualizar todos os projetos**.
3. Clique na guia **Configurações**.
4. Para incluir um serviço, no painel **Serviços associados**, clique em **incluir serviços associados**, selecione um serviço, preencha as informações de configuração e clique em **Salvar**.
5. Para incluir um token de acesso, execute as etapas a seguir:
   6. No painel **Tokens de acesso**, clique em **criar novo token**.
   7. Na caixa **Nome**, digite um nome.
   8. Na caixa **Função de acesso para o projeto**, selecione uma função, **Visualizador** ou **Editor**.
   9. Clique em ** Adicionar**.

6. Para conectar-se a um repositório GitHub, na **URL do repositório**, digite o local do repositório. Se você não tiver um token de acesso pessoal já configurado para acesso do GitHub, deverá criá-lo agora clicando em **Configurações**. Quando tiver concluído, clique em **Incluir**.


## Recuperando suas credenciais

Para usar instâncias, serviços e modelos do Bluemix em anotações e aplicativos, deve-se estar apto a inserir credenciais, tokens e terminais de pontuação no código que é usado para processamento.

1. [Efetue login no Bluemix](https://console.ng.bluemix.net/?cm_sp=dw-bluemix-_-clouddataservices-_-devcenter).
2. Role para a seção **Serviços** e clique no nome do serviço cujas credenciais você deseja recuperar.
3. Na área de janela de navegação, clique em **Credenciais de serviço**.
4. Na lista de credenciais, clique em **Visualizar credenciais**.
5. Se nenhuma credencial existir para esse serviço, será possível criá-las clicando em **Criar credenciais**.
6. Para copiar as credenciais, clique no ícone Copiar.

Dependendo do serviço, você pode ter campos diferentes, como `username` e `password`. Copie os valores conforme eles aparecem para usá-los em seu código.

## Saiba mais

[Um tutorial rápido de aprendizado profundo](https://www.ibm.com/blogs/watson/2016/10/quick-deep-learning-tutorial/)


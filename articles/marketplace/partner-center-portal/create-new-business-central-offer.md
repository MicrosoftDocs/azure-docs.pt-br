---
title: Criar uma oferta do Dynamics 365 Business central no Microsoft AppSource
description: Como criar uma oferta do Dynamics 365 for Business central no Microsoft AppSource. Liste ou venda sua oferta no AppSource ou por meio do programa CSP (provedor de soluções na nuvem).
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: navits09
ms.author: navits
ms.date: 12/02/2020
ms.openlocfilehash: 99c36354334701dcd4c7c30c5fd5039c13885525
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/19/2021
ms.locfileid: "97358700"
---
# <a name="create-a-dynamics-365-for-business-central-offer"></a>Criar uma oferta do Dynamics 365 for Business central

Este artigo descreve como criar uma nova oferta do Dynamics 365 Business central. [O Microsoft Dynamics 365 Business central](https://dynamics.microsoft.com/business-central) é um serviço de ERP (planejamento de recursos empresariais) que lida com uma ampla variedade de processos de negócios, incluindo finanças, operações, Cadeia de fornecedores, CRM e gerenciamento de projetos e comércio eletrônico. Os pacotes Premium também têm suporte para a fabricação e o modelo de implantação clássico. Todas as ofertas do Dynamics 365 Business Central devem passar pelo nosso processo de certificação.

Antes de começar, [crie uma conta do Marketplace comercial no Partner Center](create-account.md) se você ainda não tiver feito isso. Verifique se sua conta está inscrita no programa do marketplace comercial.

>[!NOTE]
> Depois que uma oferta for publicada, as edições na oferta só serão atualizadas no Partner Center e na loja online depois que você reenviar a oferta para publicação.

## <a name="create-a-new-offer"></a>Criar uma oferta

1. Entre no [Partner Center](https://partner.microsoft.com/dashboard/home).
2. No menu de navegação esquerdo, selecione **Marketplace Comercial** > **Visão geral**.
3. Na página Visão geral, selecione **+ Nova oferta** > **Dynamics 365 Business Central**.

    ![Ilustra o menu de navegação à esquerda.](./media/new-offer-dynamics-365-business-central.png)

## <a name="new-offer"></a>Nova oferta

Insira uma **ID da oferta**. Esse é um identificador exclusivo para cada oferta em sua conta.

- Essa ID é visível para os clientes no endereço da Web para a oferta do Marketplace e nos modelos do Azure Resource Manager, se aplicável.
- A ID da oferta combinada com a ID do Publicador deve ter menos de 40 caracteres de comprimento.
- Use apenas letras minúsculas e números. Ele pode incluir hifens e sublinhados, mas sem espaços. Por exemplo, se sua ID de editor for `testpublisherid` e você inserir **Test-offer-1**, o endereço Web da oferta será `https://appsource.microsoft.com/product/dynamics-365/testpublisherid.test-offer-1` .
- Essa ID não pode ser alterada depois que você seleciona **criar**.

Insira um **Alias da oferta**. Esse é o nome usado para a oferta no Partner Center.

- Esse nome não é usado no Marketplace e é diferente do nome da oferta e de outros valores mostrados aos clientes.
- Esse nome não pode ser alterado depois que você seleciona **criar**.

Selecione **Criar** para gerar a oferta e continuar.

## <a name="offer-setup"></a>Configuração da oferta

### <a name="alias"></a>Alias

Insira um nome descritivo que usaremos para se referir a essa oferta somente no Partner Center. Esse nome (preenchido previamente com o que foi inserido quando você criou a oferta) não será usado no Marketplace e será diferente do nome da oferta mostrado aos clientes. Se você quiser atualizar o nome da oferta posteriormente, vá para a página de [listagem da oferta](#offer-listing) .

### <a name="setup-details"></a>Detalhes de configuração

Selecione o **tipo de pacote** que se aplica à sua oferta:

- Um aplicativo **complementar** estende a experiência e a funcionalidade existente do Dynamics 365 Business central. Para obter detalhes, consulte [Aplicativos de complemento](/dynamics365/business-central/dev-itpro/developer/readiness/readiness-add-on-apps).
- Um aplicativo **Connect** pode ser usado no cenário em que deve ser estabelecido uma conexão ponto a ponto entre o Dynamics 365 Business central e um serviço ou solução de terceiros. Para obter detalhes, consulte [Aplicativos de conexão](/dynamics365/business-central/dev-itpro/developer/readiness/readiness-connect-apps).

Para **saber como você deseja que clientes potenciais interajam com esta oferta de listagem?**, selecione a opção que você deseja usar para esta oferta.

- **Obtenha-o agora (gratuito)** – liste sua oferta aos clientes gratuitamente.
- **Avaliação gratuita (listagem)** – liste sua oferta aos clientes com um link para uma avaliação gratuita. As avaliações gratuitas de listagem de ofertas são criadas, gerenciadas e configuradas pelo seu serviço e não têm assinaturas gerenciadas pela Microsoft.

    > [!NOTE]
    > Os tokens que seu aplicativo receberá por meio do link de avaliação só podem ser usados para obter informações do usuário por meio do Azure AD (Azure Active Directory) para automatizar a criação da conta em seu aplicativo. A autenticação com contas Microsoft usando esse token não é um procedimento compatível.

- **Entre em contato comigo** – colete informações de contato do cliente conectando seu sistema CRM (gerenciamento de relacionamento com o cliente). Será solicitado ao cliente que forneça permissão para compartilhar as respectivas informações. Esses detalhes do cliente, juntamente com o nome da oferta, a ID e a origem do Marketplace onde encontraram sua oferta, serão enviados para o sistema de CRM que você configurou. Para obter mais informações sobre como configurar o CRM, confira [Clientes potenciais](#customer-leads).

### <a name="test-drive"></a>Test drive

Um test drive é uma ótima maneira de demonstrar sua oferta para clientes potenciais oferecendo a eles a opção de experimentar antes de comprar, o que resulta no aumento da conversão e na geração de clientes potenciais altamente qualificados. Para saber mais, comece com [o que é Test Drive](../what-is-test-drive.md).

Para habilitar um test drive por um período de tempo fixo, marque a caixa de seleção **Habilitar um test drive**. Para remover o test drive de sua oferta, desmarque essa caixa de seleção.

### <a name="customer-leads"></a>Clientes potenciais

[!INCLUDE [Connect lead management](./includes/connect-lead-management.md)]

Para obter mais informações, confira [Visão geral de gerenciamento de clientes potenciais](./commercial-marketplace-get-customer-leads.md).

Selecione **Salvar rascunho** antes de continuar.

## <a name="properties"></a>Propriedades

Essa página permite que você defina as categorias e os setores usados para agrupar sua oferta no Azure Marketplace, sua versão do aplicativo e os contratos legais que dão suporte à sua oferta.

### <a name="categories"></a>Categorias

Selecione categorias e subcategorias para posicionar sua oferta nas áreas de pesquisa do Marketplace apropriadas. Descreva como sua oferta dá suporte a essas categorias na descrição da oferta. Selecione:

- Pelo menos uma e até duas categorias, incluindo uma categoria primária e uma secundária (opcional).
- Até duas subcategorias para cada categoria primária e/ou secundária. Se nenhuma subcategoria for aplicável à sua oferta, selecione **não aplicável**.

Veja a lista completa de categorias e subcategorias nas [melhores práticas de listagem de ofertas](../gtm-offer-listing-best-practices.md).

### <a name="industries"></a>Indústrias

[!INCLUDE [Industry Taxonomy](includes/industry-taxonomy.md)]

### <a name="app-version"></a>Versão do aplicativo

Insira o número de versão da sua oferta. Os clientes verão essa versão listada na página de detalhes da oferta.

### <a name="terms-and-conditions"></a>Termos e condições

Forneça seus próprios termos e condições legais aqui. Você também pode fornecer o endereço em que os termos e condições podem ser encontrados. Os clientes precisarão aceitar esses termos antes de poderem testar a oferta.

Selecione **Salvar rascunho** antes de continuar.

## <a name="offer-listing"></a>Listagem de ofertas

Esta página permite definir detalhes da oferta, como nome da oferta, descrição, links e contatos.

> [!NOTE]
> Forneça detalhes de listagem de oferta somente em um idioma. Eles não precisam estar em inglês, desde que a descrição da oferta inicie com a frase: “Este aplicativo só está disponível em [idioma, exceto inglês].” Também é aceitável fornecer uma URL de *Link útil* para oferecer conteúdo em um idioma diferente daquele usado no conteúdo de listagem da oferta.

Aqui está um exemplo de como as informações de oferta aparecem no Microsoft AppSource (os preços listados são apenas para fins de exemplo e não têm a finalidade de refletir os custos reais):
<!-- update screen? -->
:::image type="content" source="media/example-d365-business-central.png" alt-text="Ilustra como essa oferta aparece em Microsoft AppSource.":::

#### <a name="call-out-descriptions"></a>Descrições de chamada

1. Logotipo
2. Produtos
3. Categorias
4. Endereço de suporte (link)
5. Termos de uso
6. Política de privacidade
7. Nome da oferta
8. Resumo
9. Descrição
10. Capturas de tela/vídeos

### <a name="marketplace-details"></a>Detalhes do marketplace

O **nome** que você digitar aqui será mostrado aos clientes como o título da sua listagem de ofertas. Esse campo é preenchido previamente com o texto que você inseriu para o **Alias da oferta** quando você criou a oferta em questão, mas você pode alterar esse valor. Esse nome pode ser marcado (e você pode incluir os símbolos de marca registrada ou de copyright). O nome não pode ter mais de 50 caracteres e não pode incluir emojis.

Forneça uma breve descrição da sua oferta, até 100 caracteres, para o **Resumo dos resultados da pesquisa**. Essa descrição pode ser usada nos resultados da pesquisa do Marketplace.

[!INCLUDE [Long description-1](./includes/long-description-1.md)]

[!INCLUDE [Long description-2](./includes/long-description-2.md)]

[!INCLUDE [Rich text editor](./includes/rich-text-editor.md)]

Opcionalmente, você pode inserir até três **palavras-chave de pesquisa** para ajudar os clientes a localizar sua oferta no Marketplace. Para obter melhores resultados, use essas palavras-chave em sua descrição também.

Se você quiser permitir que os clientes saibam em quais **produtos seu aplicativo funciona**, insira até três nomes de produto.

### <a name="helpprivacy-urls"></a>URLs de ajuda/privacidade

Insira o **link de ajuda para seu aplicativo** (URL) em que os clientes podem saber mais sobre sua oferta. A URL da ajuda não pode ser a mesma que a URL de suporte.

Insira o **link de política de privacidade** (URL) para a política de privacidade da sua organização. Você é responsável por garantir que seu aplicativo esteja em conformidade com as leis e os regulamentos de privacidade e por fornecer uma política de privacidade válida.

### <a name="contact-information"></a>Informações de contato

Forneça o nome, o email e o número de telefone de um **Contato de suporte** e de um **Contato de engenharia**. Essas informações não são mostradas aos clientes, mas estarão disponíveis para a Microsoft e podem ser fornecidas aos parceiros CSP.

Na seção **Contato de suporte**, forneça a **URL de suporte** em que os parceiros do CSP podem encontrar suporte para sua oferta. A URL de suporte não pode ser igual à URL da ajuda.

### <a name="supporting-documents"></a>Documentos de suporte

Forneça pelo menos um (e até três) documentos de marketing relacionados aqui, como white papers, folhetos, listas de verificação ou apresentações, em formato PDF.

### <a name="marketplace-media"></a>Mídia do marketplace

Forneça logotipos e imagens que serão usados ao mostrar sua oferta aos clientes. O logotipo precisa estar no formato PNG.

[!INCLUDE [logo tips](../includes/graphics-suggestions.md)]

>[!Note]
>Se você está enfrentando um problema ao carregar arquivos, verifique se sua rede local não bloqueia o serviço https://upload.xboxlive.com que é usado pelo Partner Center.

#### <a name="logos"></a>Logotipos

Forneça um arquivo PNG para o logotipo de tamanho **grande** . O Partner Center usará isso para criar outros tamanhos necessários. Você pode, opcionalmente, substituir isso por uma imagem diferente mais tarde.

Esses logotipos são usados em locais diferentes na listagem:

[!INCLUDE [logos-appsource-only](../includes/logos-appsource-only.md)]

[!INCLUDE [Logo tips](../includes/graphics-suggestions.md)]

#### <a name="screenshots"></a>Capturas de tela

Adicione capturas de tela que mostram como sua oferta funciona. Pelo menos três capturas de tela são necessárias e você pode adicionar até cinco. Todas as capturas de tela devem ser 1280 x 720 pixels e no formato PNG.

#### <a name="videos"></a>vídeos

Opcionalmente, você pode adicionar até quatro vídeos que demonstram sua oferta. Os vídeos devem ser hospedados em um site externo. Para cada um, insira o nome do vídeo, seu endereço e uma imagem em miniatura do vídeo (1280 x 720 pixels).

Para obter recursos adicionais de listagem do Marketplace, confira [Práticas recomendadas para listagens de ofertas do Marketplace](../gtm-offer-listing-best-practices.md).

Selecione **Salvar rascunho** antes de continuar.

## <a name="availability"></a>Disponibilidade

Essa página permite que você defina onde e como tornar sua oferta disponível.

### <a name="markets"></a>Mercados

Para especificar os mercados em que sua oferta deve estar disponível, selecione **Editar mercados** para exibir a janela pop-up de **seleção de mercado** .

Selecione pelo menos um mercado. Escolha **selecionar tudo** para disponibilizar sua oferta em todos os possíveis mercados ou selecione apenas os mercados específicos que desejar. Quando terminar, selecione **Salvar**.

Suas seleções aqui se aplicam somente a novas aquisições; Se alguém já tiver seu aplicativo em um determinado mercado e você remover esse mercado posteriormente, as pessoas que já têm a oferta desse mercado poderão continuar a usá-lo, mas nenhum novo cliente nesse mercado poderá obter sua oferta.

> [!IMPORTANT]
> É sua responsabilidade atender aos requisitos legais locais, mesmo que esses requisitos não estejam listados aqui ou no Partner Center. Mesmo que você selecione todos os mercados, leis locais, restrições ou outros fatores podem impedir que determinadas ofertas sejam listadas em alguns países e regiões.

### <a name="preview-audience"></a>Público-alvo de versão prévia

Antes de publicar sua oferta como ativa para a oferta de Marketplace mais ampla, primeiro você precisará disponibilizá-la para um **público-alvo de versão prévia** limitado. Insira um **Ocultar chave** (qualquer cadeia de caracteres que use apenas letras minúsculas e/ou números) aqui. Os membros do seu público-alvo de versão prévia podem usar esse "ocultar chave" como um token para ver uma versão prévia da sua oferta no Marketplace.

Em seguida, quando estiver pronto para disponibilizar sua oferta e remover a restrição de versão prévia, você precisará remover o **ocultar chave** e publicar novamente.

Selecione **Salvar rascunho** antes de continuar.

## <a name="technical-configuration"></a>Configurações técnicas

Esta página define os detalhes técnicos usados para se conectar à sua oferta. Essa conexão nos permitirá provisionar sua oferta para o cliente final se ele optar por adquiri-la.

### <a name="file-upload"></a>Upload de arquivos

Se você tiver selecionado anteriormente **Adicionar**, onde você carregará o arquivo de pacote da sua oferta, juntamente com os arquivos de pacote para qualquer extensão na qual ele tenha dependências.

#### <a name="extension-package-file"></a>Arquivo de pacote de extensão

Carregue o arquivo de pacote de extensão (.app) da sua oferta.

#### <a name="library-extension-package-file"></a>Arquivo de pacote de extensão de biblioteca

Será necessário caso sua oferta deva ser instalada junto com outra extensão que não será publicada no marketplace. Nesse caso, carregue seu arquivo .app aqui.

>[!NOTE]
>O arquivo de pacote de dependência não é mais usado. Em vez disso, carregue um arquivo de pacote de extensão de biblioteca.

Selecione **Salvar rascunho** antes de continuar.

<!-- ## Test drive technical configuration

This page lets you set up a demonstration ("test drive") that allows customers to try your offer before purchasing it. Learn more in [What is test drive](../what-is-test-drive.md).

To enable a test drive, select the **Enable a test drive** check box on the [Offer setup](#test-drive) tab. To remove test drive from your offer, clear this check box.

When you've finished setting up your test drive, select **Save draft** before continuing.
-->
## <a name="supplemental-content"></a>Conteúdo complementar

Esta página permite que você forneça informações adicionais para nos ajudar a validar sua oferta. Essas informações não são mostradas aos clientes nem publicadas no marketplace.

### <a name="target-release"></a>Versão de destino

Indique qual versão do Microsoft Dynamics Business central sua solução tem como destino: **atual**, **próxima principal** ou **secundária seguinte**. Essas informações permitem que testemos sua solução adequadamente.

### <a name="supported-editions"></a>Edições com suporte

Caso sua oferta exija a edição Premium do Microsoft Dynamics 365 Business Central, selecione apenas **Premium**. Caso contrário, selecione **Essentials** e **Premium**.

### <a name="key-usage-scenario"></a>Principais cenários de uso

Carregue um arquivo PDF que liste os principais cenários de uso da sua oferta. Todos os cenários listados aqui poderão ser verificados por nossa equipe de validação antes de aprovarmos sua oferta do marketplace.

### <a name="test-accounts"></a>Contas de teste

Caso uma conta de teste seja necessária para que nossa equipe de certificação examine sua oferta corretamente, carregue um arquivo .pdf, .doc ou .docx com suas **Contas de teste** informações.

### <a name="app-tests-automation"></a>Automação de testes do aplicativo

Caso sua oferta seja um aplicativo de complemento, você deverá carregar um arquivo de **Automação de testes de aplicativo** (.app). Esse arquivo não é aplicável para aplicativos de conexão.

Selecione **Salvar rascunho** antes de continuar.

## <a name="publish"></a>Publicar

### <a name="submit-offer-to-preview"></a>Enviar oferta para versão prévia

Depois de concluir todas as seções necessárias da oferta, selecione **revisar e publicar** no canto superior direito do Portal.

Se for a primeira vez que você publicar essa oferta, você poderá:

- Ver o status de conclusão de cada seção da oferta.
    - **Não iniciado** -a seção não foi tocada e precisa ser concluída.
    - **Incompleto** -a seção tem erros que precisam ser corrigidos ou que exigem mais informações. Volte para as seções e atualize-as.
    - **Concluída** -a conclusão da seção, todos os dados necessários foram fornecidos e não há erros. Todas as seções da oferta precisam estar no estado concluída para que você possa enviar a oferta.
- Na seção **Notas para certificação**, forneça instruções de teste à equipe de certificação para garantir que seu aplicativo seja testado corretamente, além de eventuais notas suplementares úteis para compreensão do seu aplicativo.
- Envie a oferta para publicação selecionando **Enviar**. Você receberá uma mensagem de email quando uma versão de visualização da oferta estiver disponível para revisão e aprovação. Retorne ao Partner Center e selecione **Go-Live** para publicar sua oferta no público.

## <a name="next-steps"></a>Próximas etapas

- [Atualizar uma oferta existente no Marketplace comercial](./update-existing-offer.md)
---
title: 'Tutorial: Integração do SSO (logon único) do Azure Active Directory ao NEOGOV | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o NEOGOV.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/14/2019
ms.author: jeedes
ms.openlocfilehash: 03efcd91553ab2235782d68cc70f4acdeedce4c6
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/29/2021
ms.locfileid: "92506884"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-neogov"></a>Tutorial: Integração do SSO (logon único) do Azure Active Directory ao NEOGOV

Neste tutorial, você aprenderá a integrar o NEOGOV ao Azure AD (Azure Active Directory). Ao integrar o NEOGOV ao Azure AD, você poderá:

* Controlar no Azure AD quem tem acesso ao NEOGOV.
* Permitir que os usuários sejam conectados automaticamente ao NEOGOV com suas contas do Azure AD.
* Gerenciar suas contas em um local central: o portal do Azure.

Para saber mais sobre a integração de aplicativos SaaS ao Azure AD, confira [O que é o acesso de aplicativos e o logon único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para começar, você precisará dos seguintes itens:

* Uma assinatura do Azure AD. Caso você não tenha uma assinatura, obtenha uma [conta gratuita](https://azure.microsoft.com/free/).
* Assinatura habilitada para SSO (logon único) do NEOGOV.

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configurará e testará o SSO do Azure AD em um ambiente de teste.

* O NEOGOV dá suporte ao SSO iniciado por **IDP**

## <a name="adding-neogov-from-the-gallery"></a>Como adicionar o NEOGOV por meio da galeria

Para configurar a integração do NEOGOV ao Azure AD, você precisará adicionar o NEOGOV por meio da galeria à lista de aplicativos SaaS gerenciados.

1. Entre no [portal do Azure](https://portal.azure.com) usando uma conta corporativa ou de estudante ou uma conta pessoal da Microsoft.
1. No painel de navegação esquerdo, escolha o serviço **Azure Active Directory**.
1. Navegue até **Aplicativos Empresariais** e, em seguida, escolha **Todos os Aplicativos**.
1. Para adicionar um novo aplicativo, escolha **Novo aplicativo**.
1. Na seção **Adicionar por meio da galeria**, digite **NEOGOV** na caixa de pesquisa.
1. Selecione **NEOGOV** no painel de resultados e, em seguida, adicione o aplicativo. Aguarde alguns segundos enquanto o aplicativo é adicionado ao seu locatário.


## <a name="configure-and-test-azure-ad-single-sign-on-for-neogov"></a>Configurar e testar o logon único do Azure AD para o NEOGOV

Configure e teste o SSO do Azure AD com o NEOGOV usando um usuário de teste com o nome **B.Fernandes**. Para que o SSO funcione, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do NEOGOV.

Para configurar e testar o SSO do Azure AD com o NEOGOV, conclua os seguintes blocos de construção:

1. **[Configurar o SSO do Azure AD](#configure-azure-ad-sso)** – para permitir que os usuários usem esse recurso.
    * **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** para testar o logon único do Azure AD com B.Fernandes.
    * **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que B.Fernandes use o logon único do Azure AD.
1. **[Configurar o SSO do NEOGOV](#configure-neogov-sso)** – para configurar o logon único no lado do aplicativo.
    * **[Criar um usuário de teste do NEOGOV](#create-neogov-test-user)** – para ter um equivalente de B.Fernandes no NEOGOV que esteja vinculado à representação de usuário do Azure AD.
1. **[Testar o SSO](#test-sso)** – para verificar se a configuração funciona.

## <a name="configure-azure-ad-sso"></a>Configurar o SSO do Azure AD

Siga estas etapas para habilitar o SSO do Azure AD no portal do Azure.

1. No [portal do Azure](https://portal.azure.com/), na página de integração de aplicativos do **NEOGOV**, localize a seção **Gerenciar** e selecione **logon único**.
1. Na página **Selecionar um método de logon único**, escolha **SAML**.
1. Na página **Configurar o logon único com o SAML**, clique no ícone de edição/caneta da **Configuração Básica do SAML** para editar as configurações.

   ![Editar a Configuração Básica de SAML](common/edit-urls.png)

1. Na página **Configurar logon único com SAML**, insira os valores para os seguintes campos:

    a. Na caixa de texto **Identificador**, digite uma URL usando o seguinte padrão:

    | Ambiente | Padrão de URL |
    | -- | -- |
    | Produção | `https://www.neogov.com/` |
    | Área restrita | `https://www.uat.neogov.net/` |
    | | |

    b. Na caixa de texto **URL de Resposta**, digite uma URL usando o seguinte padrão:

    | Ambiente | Padrão de URL |
    | -- | -- |
    | Produção | `https://login.neogov.com/authentication/saml/consumer` |
    | Área restrita | `https://login.uat.neogov.net/authentication/saml/consumer` |
    | | |

1. O aplicativo NEOGOV espera as declarações SAML em um formato específico, o que exige a adição de mapeamentos de atributo personalizado à configuração de atributos de token SAML. A captura de tela a seguir mostra a lista de atributos padrão, em que **nameidentifier** é mapeado com **user.userprincipalname**. O aplicativo NEOGOV espera que **nameidentifier** seja mapeado com **user.objectid**. Portanto, você precisará editar o mapeamento de atributo clicando no ícone **Editar** e alterar o mapeamento de atributo.

    ![image](common/edit-attribute.png)

1. Além do indicado acima, o aplicativo NEOGOV espera que mais alguns atributos sejam passados novamente na resposta SAML, que são mostrados abaixo. Esses atributos também são pré-populados, mas você pode examiná-los de acordo com seus requisitos.

    | Nome |  Atributo de Origem|
    | -------|--------- |
    | mail | user.mail |

1. Na página **Configurar o logon único com o SAML**, na seção **Certificado de Autenticação SAML**, clique no botão Copiar para copiar a **URL de Metadados de Federação do Aplicativo** e salve-a no computador.

    ![O link de download do Certificado](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD

Nesta seção, você criará um usuário de teste no portal do Azure chamado B.Fernandes.

1. No painel esquerdo do portal do Azure, escolha **Azure Active Directory**, **Usuários** e, em seguida, **Todos os usuários**.
1. Selecione **Novo usuário** na parte superior da tela.
1. Nas propriedades do **Usuário**, siga estas etapas:
   1. No campo **Nome**, insira `B.Simon`.  
   1. No campo **Nome de usuário**, insira username@companydomain.extension. Por exemplo, `B.Simon@contoso.com`.
   1. Marque a caixa de seleção **Mostrar senha** e, em seguida, anote o valor exibido na caixa **Senha**.
   1. Clique em **Criar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permitirá que B.Fernandes use o logon único do Azure concedendo a ela acesso ao NEOGOV.

1. No portal do Azure, selecione **Aplicativos empresariais** e, em seguida, selecione **Todos os aplicativos**.
1. Na lista de aplicativos, selecione **NEOGOV**.
1. Na página de visão geral do aplicativo, localize a seção **Gerenciar** e escolha **Usuários e grupos**.

   ![O link “Usuários e grupos”](common/users-groups-blade.png)

1. Escolha **Adicionar usuário** e, em seguida, **Usuários e grupos** na caixa de diálogo **Adicionar Atribuição**.

    ![O link Adicionar Usuário](common/add-assign-user.png)

1. Na caixa de diálogo **Usuários e grupos**, selecione **B.Fernandes** na lista Usuários e clique no botão **Selecionar** na parte inferior da tela.
1. Se você estiver esperando um valor de função na declaração SAML, na caixa de diálogo **Selecionar Função**, escolha a função apropriada para o usuário da lista e, em seguida, clique no botão **Escolher** na parte inferior da tela.
1. Na caixa de diálogo **Adicionar atribuição**, clique no botão **Atribuir**.

## <a name="configure-neogov-sso"></a>Configurar o SSO do NEOGOV

Para configurar o logon único no lado do **NEOGOV**, é necessário enviar a **URL de Metadados de Federação do Aplicativo** para a [equipe de suporte do NEOGOV](mailto:itops@neogov.net). Eles definem essa configuração para ter a conexão de SSO de SAML definida corretamente em ambos os lados.

### <a name="create-neogov-test-user"></a>Criar um usuário de teste do NEOGOV

Nesta seção, você criará um usuário com o nome B.Fernandes no NEOGOV. Trabalhe com a [equipe de suporte do NEOGOV](mailto:itops@neogov.net) para adicionar os usuários na plataforma do NEOGOV. Os usuários devem ser criados e ativados antes de usar o logon único.

## <a name="test-sso"></a>Testar o SSO 

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco do NEOGOV no Painel de Acesso, você deverá ser conectado automaticamente ao NEOGOV, para o qual você configurou o SSO. Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Recursos adicionais

- [ Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure ](./tutorial-list.md)

- [O que é o acesso a aplicativos e logon único com o Azure Active Directory? ](../manage-apps/what-is-single-sign-on.md)

- [O que é o acesso condicional no Azure Active Directory?](../conditional-access/overview.md)

- [Experimente o NEOGOV com o Azure AD](https://aad.portal.azure.com/)
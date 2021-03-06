---
title: 'Tutorial: Criar um balanceador de carga público com um back-end baseado em IP – portal do Azure'
titleSuffix: Azure Load Balancer
description: Neste tutorial, saiba como criar um balanceador de carga público com um pool de back-end baseado em IP.
author: asudbring
ms.author: allensu
ms.service: load-balancer
ms.topic: tutorial
ms.date: 3/31/2021
ms.custom: template-tutorial
ms.openlocfilehash: f8174d46d1674e0cf81e36e89f6fb6180091a9b9
ms.sourcegitcommit: 9f4510cb67e566d8dad9a7908fd8b58ade9da3b7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/01/2021
ms.locfileid: "106123009"
---
# <a name="tutorial-create-a-public-load-balancer-with-an-ip-based-backend-using-the-azure-portal"></a>Tutorial: Criar um balanceador de carga público com um back-end baseado em IP usando o portal do Azure

Neste tutorial, você aprenderá a criar um balanceador de carga público com um pool de back-end baseado em IP. 

Uma implantação tradicional do Azure Load Balancer usa o adaptador de rede das máquinas virtuais. Com um back-end baseado em IP, as máquinas virtuais são adicionadas ao back-end pelo endereço IP.

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> * Criar uma rede virtual
> * Criar um gateway da NAT para conectividade de saída
> * Criar um Azure Load Balancer
> * Criar um pool de back-end baseado em IP
> * Criar duas máquinas virtuais
> * Testar o balanceador de carga
## <a name="prerequisites"></a>Pré-requisitos

- Uma conta do Azure com uma assinatura ativa. [Crie uma conta gratuitamente](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-a-virtual-network"></a>Criar uma rede virtual

Nesta seção, você criará uma rede virtual para o balanceador de carga, o gateway da NAT e as máquinas virtuais.

1. Entre no [portal do Azure](https://portal.azure.com).

1. No canto superior esquerdo da tela, selecione **Criar um recurso > Rede > Rede virtual** ou pesquise por **Rede virtual** na caixa de pesquisa.

2. Selecione **Criar**. 

3. Em **Criar rede virtual**, insira ou selecione estas informações na guia **Básico**:

    | **Configuração**          | **Valor**                                                           |
    |------------------|-----------------------------------------------------------------|
    | **Detalhes do projeto**  |                                                                 |
    | Subscription     | Selecionar sua assinatura do Azure                                  |
    | Grupo de recursos   | Selecione **TutorPubLBIP-rg** |
    | **Detalhes da instância** |                                                                 |
    | Nome             | Insira **myVNet**                                    |
    | Região           | Selecione **(EUA) Leste dos EUA** |

4. Selecione a guia **Endereços IP** ou selecione o botão **Avançar: Endereços IP** na parte inferior da página.

5. Na guia **Endereços IP**, insira estas informações:

    | Configuração            | Valor                      |
    |--------------------|----------------------------|
    | Espaço de endereço IPv4 | Insira **10.1.0.0/16** |

6. Em **Nome da sub-rede**, selecione a palavra **padrão**.

7. Em **Editar sub-rede**, insira estas informações:

    | Configuração            | Valor                      |
    |--------------------|----------------------------|
    | Nome da sub-rede | Insira **myBackendSubnet** |
    | Intervalo de endereços da sub-rede | Insira **10.1.0.0/24** |

8. Clique em **Salvar**.

9. Selecione a guia **Segurança**.

10. Em **BastionHost**, selecione **Habilitar**. Insira estas informações:

    | Configuração            | Valor                      |
    |--------------------|----------------------------|
    | Nome do bastion | Insira **myBastionHost** |
    | Espaço de endereço da AzureBastionSubnet | Insira **10.1.1.0/27** |
    | Endereço IP público | Selecione **Criar novo**. </br> Em **Nome**, insira **myBastionIP**. </br> Selecione **OK**. |


11. Selecione a guia **Revisar + criar** ou o botão **Revisar + criar**.

12. Selecione **Criar**.
## <a name="create-nat-gateway"></a>Criar gateway NAT

Nesta seção, você criará um gateway da NAT e o atribuirá à sub-rede na rede virtual criada anteriormente.

1. No lado superior esquerdo da tela, clique em **Criar um recurso > Rede > gateway da NAT** ou pesquise a opção **gateway da NAT** na caixa de pesquisa.

2. Selecione **Criar**. 

3. Em **Criar gateway da NAT (conversão de endereços de rede)** , insira ou selecione as seguintes informações na guia **Básico**:

    | **Configuração**          | **Valor**                                                           |
    |------------------|-----------------------------------------------------------------|
    | **Detalhes do projeto**  |                                                                 |
    | Subscription     | Selecione sua assinatura do Azure.                                  |
    | Grupo de recursos   | Selecione **Criar** e insira **TutorPubLBIP-rg** na caixa de texto. </br> Selecione **OK**. |
    | **Detalhes da instância** |                                                                 |
    | Nome             | Insira a opção **myNATgateway**                                    |
    | Região           | Selecione **(EUA) Leste dos EUA**  |
    | Zona de disponibilidade | Selecione **Nenhum**. |
    | Tempo limite de ociosidade (minutos) | Insira **10**. |

4. Selecione a guia **IP de saída** ou clique no botão **Avançar: IP de saída** na parte inferior da página.

5. Na guia **IP de saída**, insira ou selecione as seguintes informações:

    | **Configuração** | **Valor** |
    | ----------- | --------- |
    | Endereços IP públicos | Clique em **Criar um endereço IP público**. </br> Em **Nome**, insira **myPublicIP-NAT**. </br> Selecione **OK**. |

6. Selecione a guia **Sub-rede** ou clique no botão **Avançar: sub-rede** na parte inferior da página.

7. Na guia **Sub-rede**, selecione a opção **myVNet** no menu suspenso **Rede virtual**.

8. Marque a caixa ao lado de **myBackendSubnet**.

9. Selecione a guia **Examinar + criar** ou clique no botão azul **Examinar + criar** na parte inferior da página.

10. Selecione **Criar**.
## <a name="create-load-balancer"></a>Criar um balanceador de carga

Nesta seção, você criará um Standard Azure Load Balancer. 

1. Entre no [portal do Azure](https://portal.azure.com).
2. Selecione **Criar um recurso**. 
3. Na caixa de pesquisa, digite **Balanceador de carga**. Selecione **Balanceador de carga** nos resultados da pesquisa.
4. Na página **Balanceador de carga**, clique em **Criar**.
5. Na página **Criar balanceador de carga**, insira ou selecione as seguintes informações: 

    | Configuração                 | Valor                                              |
    | ---                     | ---                                                |
    | **Detalhes do projeto** |   |
    | Subscription               | Selecione sua assinatura.    |    
    | Resource group         | Selecione **TutorPubLBIP-rg**.|
    | **Detalhes da instância** |   |
    | Nome                   | Insira **myLoadBalancer**                                   |
    | Região         | Selecione **(EUA) Leste dos EUA**.                                        |
    | Type          | Selecione **Público**.                                        |
    | SKU           | Deixe o padrão **Standard**. |
    | Camada          | Deixe o padrão **Regional**. |
    | **Endereço IP público** |   |
    | Endereço IP público | Selecione **Criar novo**. </br> Se você tiver um IP público existente que deseja usar, selecione **Usar existente**. |
    | Nome do endereço IP público | Insira **myPublicIP-LB** na caixa de texto.|
    | Zona de disponibilidade | Selecione **Com redundância de zona** para criar uma balanceador de carga resiliente. Para criar um balanceador de carga zonal, selecione uma zona específica de 1, 2 ou 3 |
    | Adicionar um endereço IPv6 público | Selecione **Não**. </br> Para obter mais informações sobre endereços IPv6 e o balanceador de carga, confira [O que é o IPv6 da Rede Virtual do Azure?](../virtual-network/ipv6-overview.md)  |
    | Preferência de roteamento | Deixe o padrão da **rede da Microsoft**. </br> Para obter mais informações sobre preferências de roteamento, confira [O que é uma preferência de roteamento (versão prévia)?](../virtual-network/routing-preference-overview.md). |

6. Aceite os padrões para as demais configurações e selecione **Examinar + criar**.

7. Na guia **Examinar + criar**, selecione **Criar**.

## <a name="create-load-balancer-resources"></a>Criar recursos do balanceador de carga

Nesta seção, você definirá:

* Configurações do balanceador de carga para um pool de endereços de back-end.
* Uma investigação de integridade.
* Uma regra do balanceador de carga.

### <a name="create-a-backend-pool"></a>Crie um pool de back-end

Um pool de endereços de back-end contém os endereços IP das (NICs) virtuais conectadas ao balanceador de carga. 

Crie o pool de endereços de back-end **myBackendPool** para incluir máquinas virtuais para balanceamento de carga de tráfego da Internet.

1. Clique em **Todos os serviços** no menu à esquerda, selecione **Todos os recursos** e depois selecione **myLoadBalancer** na lista de recursos.

2. Em **Configurações**, selecione **Pools de back-end** e **+ Adicionar**.

3. Na página **Adicionar um pool de back-end**, insira ou selecione as seguintes informações:

    | Configuração | Valor |
    | ------- | ----- |
    | Nome | Insira **myBackendPool**. |
    | Rede virtual | Selecione **myVNet**. |
    | Configuração do pool de back-end | Selecione **Endereço IP**. |
    | Versão IP | Selecione **IPv4**. |

4. Selecione **Adicionar**.

### <a name="create-a-health-probe"></a>Criar uma investigação de integridade

O balanceador de carga monitora o status do seu aplicativo com uma investigação de integridade. 

A investigação de integridade adiciona ou remove VMs do balanceador de carga com base na resposta às verificações de integridade. 

Crie uma investigação de integridade chamada **myHealthProbe** para monitorar a integridade das VMs.

1. Clique em **Todos os serviços** no menu à esquerda, selecione **Todos os recursos** e depois selecione **myLoadBalancer** na lista de recursos.

2. Em **Configurações**, selecione **Investigações de integridade** e, em seguida, **+ Adicionar**.
    
    | Configuração | Valor |
    | ------- | ----- |
    | Nome | Insira **myHealthProbe**. |
    | Protocolo | selecione **TCP**. |
    | Porta | Insira **80**.|
    | Intervalo | Insira **15** para o número de **Intervalo** em segundos entre tentativas de investigação. |
    | Limite não íntegro | Selecione **2**. |
   
3. Mantenha o restante com os padrões e selecione **Adicionar**.

### <a name="create-a-load-balancer-rule"></a>Criar uma regra de balanceador de carga

Uma regra de balanceador de carga é usada para definir como o tráfego é distribuído para as VMs. Você define a configuração de IP de front-end para o tráfego de entrada e o pool de IPs de back-end para receber o tráfego. A porta de origem e de destino são definidas na regra. 

Nesta seção, você criará uma regra de balanceador de carga:

* Chamada **myHTTPRule**.
* No front-end chamado **LoadBalancerFrontEnd**.
* Escutando a **Porta 80**.
* Direciona o tráfego com balanceamento de carga para o back-end chamado **myBackendPool** na **Porta 80**.

1. Clique em **Todos os serviços** no menu à esquerda, selecione **Todos os recursos** e depois selecione **myLoadBalancer** na lista de recursos.

2. Em **Configurações**, selecione **Regras de balanceamento de carga** e **+ Adicionar**.

3. Insira ou selecione as seguintes informações para a regra do balanceador de carga:
    
    | Configuração | Valor |
    | ------- | ----- |
    | Nome | Insira **myHTTPRule**. |
    | Versão IP | Selecione **IPv4** |
    | Endereço IP de front-end | Selecione **LoadBalancerFrontEnd** |
    | Protocolo | selecione **TCP**. |
    | Porta | Insira **80**.|
    | Porta de back-end | Insira **80**. |
    | Pool de back-end | Selecione **myBackendPool**.|
    | Investigação de integridade | Selecione **myHealthProbe**. |
    | Persistência de sessão | Mantenha o padrão **Nenhum**. |
    | Tempo limite de ociosidade (minutos) | Insira **15** minutos. |
    | Redefinição de TCP | Selecione **Habilitado**. |
    | IP flutuante | Selecione **Desabilitado**. |
    | SNAT (conversão de endereços de rede de origem) de saída | Selecione **(Recomendado) Usar regras de saída para fornecer acesso à Internet aos membros do pool de back-ends.** |

4. Mantenha o restante dos padrões e selecione **Adicionar**.

## <a name="create-virtual-machines"></a>Criar máquinas virtuais

Nesta seção, você criará duas VMs (**myVM1** e **myVM2**) em duas zonas diferentes (**Zona 1** e **Zona 2**).

Essas VMs são adicionadas ao pool de back-end do balanceador de carga criado anteriormente.

1. No canto superior esquerdo do portal, selecione **Criar um recurso** > **Computação** > **Máquina virtual**. 
   
2. Em **Criar uma máquina virtual**, insira ou selecione os valores na guia **Informações básicas**:

    | Configuração | Valor                                          |
    |-----------------------|----------------------------------|
    | **Detalhes do projeto** |  |
    | Subscription | Selecionar sua assinatura do Azure |
    | Grupo de recursos | Selecione **TutorPubLBIP-rg** |
    | **Detalhes da instância** |  |
    | Nome da máquina virtual | Insira **myVM1** |
    | Região | Selecione **(EUA) Leste dos EUA** |
    | Opções de disponibilidade | Selecione **Zonas de disponibilidade** |
    | Zona de disponibilidade | Selecione **1** |
    | Imagem | Selecione **Windows Server 2019 Datacenter** |
    | Instância do Azure Spot | Deixar o padrão |
    | Tamanho | Escolha o tamanho da VM ou use a configuração padrão |
    | **Conta de administrador** |  |
    | Nome de Usuário | Insira um nome de usuário |
    | Senha | Insira uma senha |
    | Confirmar senha | Insira novamente a senha |
    | **Regras de porta de entrada** |  |
    | Porta de entrada públicas | Selecione **Nenhum** |

3. Selecione a guia **Rede** ou selecione **Avançar: Discos**, em seguida, **Avançar: Rede**.
  
4. Na guia Rede, selecione ou insira:

    | Configuração | Valor |
    |-|-|
    | **Interface de rede** |  |
    | Rede virtual | **myVNet** |
    | Sub-rede | **myBackendSubnet** |
    | IP público | Selecione **Nenhum**. |
    | Grupo de segurança de rede da NIC | Selecione **Avançado**|
    | Configurar um grupo de segurança de rede | Selecione **Criar novo**. </br> Em **Criar grupo de segurança de rede**, insira **myNSG** no **Nome**. </br> Em **Regras de entrada**, selecione **+Adicionar uma regra de entrada**. </br> Em **Serviço**, selecione **HTTP**. </br> Em **Prioridade**, insira **100**. </br> Em **Nome**, insira **myHTTPRule** </br> Selecione **Adicionar** </br> Selecione **OK** |
    | **Balanceamento de carga**  |
    | Proteger esta máquina virtual com uma solução de balanceamento de carga existente? | Marque a caixa de seleção.|
    | **Configurações de balanceamento de carga** |
    | Opções de balanceamento de carga | Selecione **Azure Load Balancer** |
    | Selecionar um balanceador de carga | Selecione **myLoadBalancer**  |
    | Selecionar um pool de back-end | Selecione **myBackendPool** |
   
5. Selecione **Examinar + criar**. 
  
6. Examine as configurações e selecione **Criar**.

7. Siga as etapas 1 a 6 para criar uma VM com os seguintes valores e todas as outras configurações iguais à da **myVM1**:

    | Configuração | VM 2 |
    | ------- | ----- |
    | Nome |  **myVM2** |
    | Zona de disponibilidade | **2** |
    | Grupo de segurança de rede | Selecione o **myNSG** existente| 

## <a name="install-iis"></a>Instalar o IIS

1. Selecione **Todos os serviços** no menu à esquerda, **Todos os recursos** e, na lista de recursos, selecione **myVM1**, localizada no grupo de recursos **TutorPubLBIP-rg**.

2. Na página **Visão Geral**, selecione **Conectar** e **Bastion**.

3. Selecione o botão **Usar Bastion**.

4. Insira o nome de usuário e a senha fornecidos durante a criação da VM.

5. Selecione **Conectar**.

6. Na área de trabalho do servidor, navegue até **Ferramentas Administrativas do Windows** > **Windows PowerShell**.

7. Na janela do PowerShell, execute os seguintes comandos para:

    * Instalar o servidor IIS
    * Remover o arquivo padrão iisstart.htm
    * Adicionar um novo arquivo iisstart.htm que exiba o nome da VM:

   ```powershell
    # install IIS server role
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # remove default htm file
    Remove-Item C:\inetpub\wwwroot\iisstart.htm
    
    # Add a new htm file that displays server name
    Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
   ```
8. Feche a sessão do Bastion com **myVM1**.

9. Repita as etapas de 1 a 7 para instalar o IIS e o arquivo iisstart.htm atualizado em **myVM2**.

## <a name="test-the-load-balancer"></a>Testar o balanceador de carga

1. Encontre o endereço IP público para o balanceador de carga na tela **Visão Geral**. Escolha **Todos os serviços** no menu à esquerda, **Todos os recursos** e **myPublicIP-LB**.

2. Copie o endereço IP público e cole-o na barra de endereços do seu navegador. A página padrão do servidor Web do IIS é exibida no navegador.

   ![Servidor Web do IIS](./media/tutorial-load-balancer-standard-zonal-portal/load-balancer-test.png)

Para ver o balanceador de carga distribuir o tráfego para a myVM2, force a atualização do seu navegador da Web no computador cliente.
## <a name="clean-up-resources"></a>Limpar os recursos

Caso não pretenda continuar usando esse aplicativo, exclua a rede virtual, a máquina virtual e o gateway da NAT usando as seguintes etapas:

1. No menu esquerdo, selecione **Grupos de recursos**.

2. Selecione o grupo de recursos **TutorPubLBIP-rg**.

3. Selecione **Excluir grupo de recursos**.

4. Insira **TutorPubLBIP-rg** e selecione **Excluir**.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você:

* Criou uma rede virtual
* Criou um gateway da NAT
* Criou um balanceador de carga com um pool de back-end baseado em IP
* Testou o balanceador de carga

Passe para o próximo artigo para saber como criar um balanceador de carga entre regiões:
> [!div class="nextstepaction"]
> [Criar um Azure Load Balancer entre regiões usando o portal do Azure](tutorial-cross-region-portal.md)

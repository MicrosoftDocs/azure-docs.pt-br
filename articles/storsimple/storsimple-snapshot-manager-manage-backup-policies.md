---
title: Políticas de backup do StorSimple Snapshot Manager | Microsoft Docs
description: Descreve como usar o snap-in StorSimple Snapshot Manager MMC para criar e gerenciar as políticas de backup que controlam os backups agendados.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: 04415d0b-42f0-4737-8afa-257fb2dbe5d0
ms.service: storsimple
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: alkohli
ms.openlocfilehash: 745aeb9844f1abad075d34b34735563200ea618b
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/19/2021
ms.locfileid: "90054935"
---
# <a name="use-storsimple-snapshot-manager-to-create-and-manage-backup-policies"></a>Usar o StorSimple Snapshot Manager para criar e gerenciar políticas de backup
## <a name="overview"></a>Visão geral
Uma política de backup cria um cronograma para o backup de dados de volumes localmente ou na nuvem. Quando cria uma política de backup, você também pode especificar uma política de retenção. (Você pode reter um máximo de 64 instantâneos). Para obter mais informações sobre políticas de backup, consulte [tipos de backup](storsimple-what-is-snapshot-manager.md#backup-types-and-backup-policies) no [StorSimple 8000 Series: uma solução de nuvem híbrida](storsimple-overview.md).

Este tutorial explica como:

* Criar uma política de backup
* Editar uma política de backup
* Excluir uma política de backup

## <a name="create-a-backup-policy"></a>Criar uma política de backup
Use o procedimento a seguir para criar uma nova política de backup.

#### <a name="to-create-a-backup-policy"></a>Para criar uma política de backup
1. Clique no ícone da área de trabalho para iniciar o StorSimple Snapshot Manager.
2. No painel **Escopo**, clique com o botão direito do mouse em **Políticas de backup** e clique em **Criar política de backup**.

    ![Criar uma política de backup](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_BU_policy.png)

    A caixa de diálogo **Criar uma política** aparece.

    ![Criar uma Política - guia Geral](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_general.png)
3. Na guia **Geral**, preencha as informações a seguir:

   1. Na caixa de texto **Nome**, digite um nome para a política.
   2. Na caixa de texto **Grupo de volume**, digite o nome do grupo de volume associado à política.
   3. Selecione **Instantâneo local** ou **Instantâneo de nuvem**.
   4. Selecione o número de instantâneos a serem retidos. Se você selecionar **Tudo**, 64 instantâneos serão retidos (o máximo).
4. Clique na guia **Agenda**.

    ![Criar uma política - guia Agenda](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_schedule.png)
5. Na guia **Agenda**, preencha as informações a seguir:

   1. Clique na caixa de seleção **Habilitar** para agendar o próximo backup.
   2. Em **Configurações**, selecione **Uma vez**, **Diário**, **Semanal** ou **Mensal**.
   3. Na caixa de texto **Iniciar**, clique no ícone de calendário e selecione a data de início.
   4. Em **Configurações avançadas**, é possível definir agendas opcionais de repetição e uma data de término.
   5. Clique em **OK**.

Depois de criar uma política de backup, as seguintes informações aparecem no painel **Resultados**:

* **Nome** – o nome da política de backup.
* **Tipo** – instantâneo local ou instantâneo de nuvem.
* **Grupo de volume** – o grupo de volume associado à política.
* **Retenção** – o número de instantâneos retidos; o máximo é 64.
* **Criado** – a data em que esta política foi criada.
* **Habilitado** – se a política está em vigor: **Verdadeiro** indica que está em vigor; **Falso** indica que não está.

## <a name="edit-a-backup-policy"></a>Editar uma política de backup
Use o procedimento a seguir para editar uma política de backup existente.

#### <a name="to-edit-a-backup-policy"></a>Para editar uma política de backup
1. Clique no ícone da área de trabalho para iniciar o StorSimple Snapshot Manager.
2. No painel **Escopo**, clique no nó **Políticas de Backup**. Todas as políticas de backup aparecem no painel **Resultados**.
3. Clique com o botão direito na política que deseja editar e clique em **Editar**.

    ![Editar uma política de backup](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Edit_BU_policy.png)
4. Quando a janela **Criar uma Política** aparece, insira suas alterações e, em seguida, clique em **OK**.

## <a name="delete-a-backup-policy"></a>Excluir uma política de backup
Use o procedimento a seguir para excluir uma política de backup.

#### <a name="to-delete-a-backup-policy"></a>Para excluir uma política de backup
1. Clique no ícone da área de trabalho para iniciar o StorSimple Snapshot Manager.
2. No painel **Escopo**, clique no nó **Políticas de Backup**. Todas as políticas de backup aparecem no painel **Resultados**.
3. Clique com o botão direito do mouse na política de backup que deseja excluir e, em seguida, clique em **Excluir**.
4. Quando a mensagem de confirmação aparecer, clique em **Sim**.

    ![Excluir confirmação da política de backup](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Delete_BU_policy.png)

## <a name="next-steps"></a>Próximas etapas
* Saiba como [usar o StorSimple Snapshot Manager para administrar sua solução do StorSimple](storsimple-snapshot-manager-admin.md).
* Saiba como [Usar o StorSimple Snapshot Manager para exibir e gerenciar trabalhos de backup](storsimple-snapshot-manager-manage-backup-jobs.md).

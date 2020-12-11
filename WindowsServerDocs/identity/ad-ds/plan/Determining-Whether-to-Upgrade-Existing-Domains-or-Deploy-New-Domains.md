---
description: Más información acerca de cómo determinar si actualizar dominios existentes o implementar dominios nuevos
ms.assetid: c20231dd-2b83-4494-9385-1172272e00d6
title: Determinar si se deben actualizar los dominios existentes o implementar nuevos dominios
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: b89ea1c898fc1c7a6859b88d2b7ba21d47516d06
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97044073"
---
# <a name="determining-whether-to-upgrade-existing-domains-or-deploy-new-domains"></a>Determinar si se deben actualizar los dominios existentes o implementar nuevos dominios

> Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cada dominio del diseño será un nuevo dominio o un dominio actualizado existente. Los usuarios de dominios existentes que no se actualizan deben moverse a nuevos dominios.

Mover cuentas entre dominios puede afectar a los usuarios finales. Antes de decidir si mover usuarios a un nuevo dominio o actualizar dominios existentes, evalúe las ventajas administrativas a largo plazo de un nuevo dominio de AD DS con el costo de mover usuarios al dominio.

Para obtener más información acerca de la actualización de dominios de Active Directory a Windows Server 2008, consulte [actualización de dominios de Active Directory a dominios de AD DS de Windows server 2008 y Windows server 2008 R2](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731188(v=ws.10)).

Para obtener más información acerca de la reestructuración de dominios de AD DS entre bosques, consulte la [Guía de ADMT: migración y reestructuración de dominios de Active Directory](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc974332(v=ws.10)).

Para obtener una hoja de cálculo que le ayude a documentar los planes de dominios nuevos y actualizados, descargue Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip de la [ayuda del trabajo para el kit de implementación de Windows Server 2003](https://microsoft.com/download/details.aspx?id=9608) y abra "planeamiento de dominios" (DSSLOGI_5.doc).

---
ms.assetid: 2a2f493a-9796-454a-9721-e223b799dfa7
title: Planear la ubicación del controlador de dominio de raíz del bosque
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 57eafcc884a827d98c249e2da0c0af6888abc5b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823646"
---
# <a name="planning-forest-root-domain-controller-placement"></a>Planear la ubicación del controlador de dominio de raíz del bosque

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Controladores de dominio raíz del bosque se necesitan para crear rutas de acceso de confianza para los clientes que necesitan tener acceso a recursos en dominios que no sean sus propios. Coloque los controladores de dominio raíz del bosque en ubicaciones de centro y en ubicaciones que hospedan los centros de datos. Si los usuarios de una ubicación determinada necesitan tener acceso a recursos de otros dominios en la misma ubicación y la disponibilidad de red entre el centro de datos y la ubicación del usuario no es confiable, puede agregar un controlador de dominio raíz de bosque en la ubicación o cree una confianza directa entre los dos dominios. Es más rentable para crear una confianza directa entre los dominios a menos que tenga otras razones para colocar un controlador de dominio raíz de bosque en esa ubicación.  
  
Ayuda a optimizar las solicitudes de autenticación realizadas desde usuarios ubicados en cualquiera de los dominios de confianzas de acceso directo. Para obtener más información acerca de confianzas entre dominios, vea el artículo [cuándo crear una confianza directa](https://go.microsoft.com/fwlink/?LinkId=107061).  
  
Para que una hoja de cálculo que le ayudarán a documentar la ubicación del controlador de dominio de bosque raíz, consulte [trabajo ayudas para Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), descargue Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip y abra "Ubicación del controlador de dominio" (DSSTOPO_4.doc).  
  
Deberá consultar esta información cuando se crea el dominio raíz del bosque. Para obtener más información sobre cómo implementar el dominio raíz del bosque, consulte [implementar un dominio raíz del bosque de Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  

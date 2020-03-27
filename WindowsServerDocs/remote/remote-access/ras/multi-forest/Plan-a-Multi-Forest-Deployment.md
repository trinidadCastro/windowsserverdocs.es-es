---
title: Planear una implementación con varios bosques
description: Este tema forma parte de la guía implementar el acceso remoto en un entorno de varios bosques en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8acc260f-d6d1-4d32-9e3a-1fd0b2a71586
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 8e483f5986a5a23123495e3a13440ddc57a6c521
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314041"
---
# <a name="plan-a-multi-forest-deployment"></a>Planear una implementación con varios bosques

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describen los pasos de planeación necesarios a la hora de configurar Acceso remoto en una implementación con varios bosques.  
  
## <a name="prerequisites"></a>Requisitos previos  
Antes de empezar a implementar este escenario, revise esta lista de requisitos importantes:  
  
-   Se requiere confianza bidireccional.  
  
## <a name="plan-trust-between-forests"></a>Planear la confianza entre bosques  
Cuando tome la decisión de permitir el acceso a los recursos desde un bosque nuevo, dejar que los clientes del nuevo bosque usen DirectAccess o agregar servidores de Acceso remoto desde el nuevo bosque a modo de puntos de entrada a la implementación de Acceso remoto, deberá asegurarse de que entre ambos bosques se establece una plena confianza (esto es, una confianza transitiva bidireccional). Consulte el tema sobre los [tipos de confianza](https://technet.microsoft.com/library/cc775736.aspx). La plena confianza entre bosques es un requisito previo indispensable para que un administrador lleve a cabo diversas operaciones en una implementación con varios bosques, como modificar GPO en el nuevo bosque, usar grupos de seguridad del nuevo bosque como el grupo de seguridad de cliente, efectuar llamadas remotas (WinRM, RPC) a equipos del nuevo bosque o autenticar a clientes remotos desde el nuevo bosque.  
  
## <a name="plan-remote-access-administrator-permissions"></a>Planear permisos de administrador de Acceso remoto  
Al configurar Acceso remoto, se actualizan (y, a veces, crean) GPO en cada uno de los dominios que contengan servidores o clientes de Acceso remoto. Al igual que en los entornos con un solo bosque, el administrador de Acceso Remoto en un entorno con varios bosques debe poseer permisos para escribir y modificar los GPO de DirectAccess y sus filtros de seguridad y, opcionalmente, permisos para crear vínculos relativos a los GPO de DirectAccess en todos los bosques implicados en el proceso. Estos permisos son necesarios con independencia del bosque al que pertenezca el administrador de Acceso Remoto.  
  
Además, este administrador debe ser un administrador local en todos los servidores de Acceso remoto, incluidos los servidores de Acceso remoto del nuevo bosque que se han agregado a la implementación de Acceso remoto original como puntos de entrada.  
  
## <a name="plan-client-security-groups"></a><a name="ClientSG"></a>Planeación de grupos de seguridad de cliente  
En el nuevo bosque se debe configurar al menos un grupo de seguridad para los equipos cliente de DirectAccess que haya en dicho bosque. La razón es que un solo grupo de seguridad no puede contener cuentas de diversos bosques.  
  
> [!NOTE]  
> -   DirectAccess requiere al menos un grupo de seguridad de cliente de Windows 10&reg; o Windows&reg; 8 para cada bosque. Sin embargo, se recomienda tener un grupo de seguridad de cliente de Windows 10 o Windows 8 para cada dominio que contenga clientes de Windows 10 o Windows 8.  
> -   Cuando se habilita multisitio, DirectAccess requiere al menos un grupo de seguridad de cliente de Windows 7&reg; por bosque para cada punto de entrada de DirectAccess en el que se admitan los equipos cliente de Windows 7. Sin embargo, se recomienda tener un grupo de seguridad de cliente de Windows 7 independiente para cada punto de entrada de cada dominio que contenga clientes de Windows 7.  
>   
> Para que DirectAccess se aplique a equipos cliente en más dominios, se deben crear GPO de cliente en tales dominios. La adición de grupos de seguridad hace que se escriban nuevos GPO de cliente para los dominios nuevos; por lo tanto, si se agrega un nuevo grupo de seguridad de un nuevo dominio a la lista de grupos de seguridad de cliente de DirectAccess, se creará automáticamente un GPO de cliente en el nuevo dominio, y a través de dicho GPO se aplicará la configuración de DirectAccess a los equipos cliente del nuevo dominio.  
>   
> A este respecto, conviene indicar que si se agrega un cliente de un nuevo dominio a un grupo de seguridad existente que ya está configurado como grupo de seguridad de cliente de DirectAccess, el GPO de cliente no se creará automáticamente en el nuevo dominio. En consecuencia, el cliente en el nuevo dominio no recibirá la configuración de DirectAccess y no podrá conectarse a través de DirectAccess.  
  
## <a name="plan-certification-authorities"></a>Planear entidades de certificación  
Si la implementación de DirectAccess está configurada para usar la autenticación mediante contraseña de un solo uso (OTP), todos los bosques contendrán las mismas plantillas de certificado de firma, pero valores de Oid distintos. Esto tiene como resultado que los bosques no se pueden configurar como una sola unidad de configuración. Para resolver este problema y configurar OTP en un entorno de varios bosques, consulte la sección "configurar OTP en una implementación de varios bosques" en el tema [configurar una implementación de varios bosques](Configure-a-Multi-Forest-Deployment.md).  
  
Al usar la autenticación de certificado de equipo IPsec, el certificado de equipo de todos los equipos cliente y servidor debe proceder de la misma entidad de certificación raíz o intermedia, independientemente del bosque al que pertenezcan.  
  
## <a name="plan-otp-exemptions"></a>Planear exenciones de OTP  
Si usa la autenticación OTP de DirectAccess, tenga en cuenta que el grupo de seguridad de exención de OTP se limita a los usuarios de un solo bosque. Esto se debe a que un grupo de seguridad puede contener exclusivamente usuarios de un solo bosque y solo se puede configurar un único grupo de seguridad.  
  



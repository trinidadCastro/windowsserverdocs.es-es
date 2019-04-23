---
title: Kerberos Constrained Delegation Overview
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-kerberos
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 51923b0a-0c1a-47b2-93a0-d36f8e295589
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: d77dd6f6f310ae71a4d9e2d52b44cc9b1bef6401
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821936"
---
# <a name="kerberos-constrained-delegation-overview"></a>Kerberos Constrained Delegation Overview

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema para profesionales de TI describe nuevas funcionalidades para la delegación restringida de Kerberos en Windows Server 2012 R2 y Windows Server 2012.

**Descripción de la característica**

La delegación limitada de Kerberos se presentó en Windows Server 2003 y proporcionaba una forma de delegación más segura que podían usar los servicios. Cuando se configura, restringe los servicios en los que puede actuar un servidor determinado en nombre de un usuario. Para esto se requieren privilegios de administrador de dominio para configurar una cuenta de dominio para un servicio y se restringe la cuenta a un único dominio. En las empresas de hoy en día, los servicios front-end no están diseñados para limitarse a la integración con servicios exclusivamente en su dominio.

En los sistemas operativos anteriores donde el administrador de dominio configuraba el servicio, el administrador de servicios no disponía de ninguna manera que fuera útil para saber qué servicios front-end delegaban a los servicios de recurso que poseían. Y cualquier servicio front-end que podía delegar a un servicio de recursos representaba un punto de ataque potencial. Si estaba comprometido un servidor que hospedaba un servicio front-end, y estaba configurado para delegar a servicios de recursos, los servicios de recursos también podían verse comprometidos.

En Windows Server 2012 R2 y Windows Server 2012, capacidad de configurar la delegación restringida para el servicio se transfirió desde el administrador del dominio al administrador de servicios. De esta manera, el administrador de servicios back-end puede permitir o denegar servicios front-end.

Para obtener información detallada sobre la delegación limitada según se presentó en Windows Server 2003, consulte el tema sobre la [delegación limitada y la transición de protocolo de Kerberos](https://technet.microsoft.com/library/cc739587(v=ws.10)).

La implementación de Windows Server 2012 R2 y Windows Server 2012 del protocolo Kerberos incluye extensiones específicamente para la delegación restringida.  S4U2Proxy (del inglés Service for User to Proxy) permite que un servicio use su vale de servicio de Kerberos para que un usuario obtenga un vale de servicio del Centro de distribución de claves (KDC) para el servicio back-end. Estas extensiones permiten que la delegación restringida en la cuenta del servicio back-end, que puede estar en otro dominio. Para obtener más información sobre estas extensiones, consulte [ \[MS-SFU\]: Extensiones del protocolo Kerberos: Servicio para el usuario y la especificación del protocolo de delegación restringida](https://msdn.microsoft.com/library/cc246071(PROT.13).aspx) en MSDN Library.

**Aplicaciones prácticas**

La delegación restringida proporciona a los administradores de servicios la capacidad de especificar y exigir límites de confianza de aplicaciones limitando el ámbito en que los servicios de aplicación pueden actuar en nombre de un usuario. Los administradores de servicio pueden configurar qué cuentas de servicio front-end pueden delegar a sus servicios back-end.

Mediante la delegación restringida auxiliar en dominios de Windows Server 2012 R2 y Windows Server 2012, servicios de front-end como Microsoft Internet Security and Acceleration (ISA) Server, Microsoft Forefront Threat Management Gateway, Microsoft Exchange Outlook Web Access (OWA) y Microsoft SharePoint Server pueden configurarse para usar la delegación restringida para autenticarse en servidores de otros dominios. Esto proporciona compatibilidad con soluciones de servicios entre dominios mediante una infraestructura de Kerberos existente. La delegación limitada de Kerberos puede ser administrada por administradores de dominio o administradores de servicios.

## <a name="resource-based-constrained-delegation-across-domains"></a>Delegación limitada basada en recursos entre dominios

La delegación limitada de Kerberos se puede usar para proporcionar delegación limitada cuando los servicios de recurso y el servicio front-end no se encuentran en el mismo dominio. Los administradores de servicio pueden configurar la nueva delegación especificando las cuentas de dominio de los servicios front-end que pueden suplantar a los usuarios en los objetos de cuenta de los servicios de recurso.

**¿Qué valor aporta este cambio?**

Admitiendo la delegación limitada entre dominios, los servicios pueden configurarse para usar la delegación limitada para autenticar servidores de otros dominios en lugar de usar la delegación sin restricciones. Esto proporciona compatibilidad para la autenticación para soluciones de servicio entre dominios mediante el uso de una infraestructura de Kerberos existente sin necesidad de confiar en servicios front-end para delegar a cualquier servicio.

Esto también desplaza la decisión sobre si debe confiar en un servidor de origen de una identidad delegada desde el Administrador de dominio de delegación para el propietario del recurso.

**¿Qué funciona de manera diferente?**

Un cambio en el protocolo subyacente permite la delegación limitada entre dominios. La implementación de Windows Server 2012 R2 y Windows Server 2012 del protocolo Kerberos incluye extensiones al servicio para el usuario en el protocolo de Proxy (S4U2Proxy). Este es un conjunto de extensiones al protocolo de Kerberos que permite que un servicio use su vale de servicio de Kerberos para que un usuario obtenga un vale de servicio del Centro de distribución de claves (KDC) para un servicio back-end.

Para obtener más información sobre estas extensiones, consulte [ \[MS-SFU\]: Extensiones del protocolo Kerberos: Servicio para el usuario y la especificación del protocolo de delegación restringida](https://msdn.microsoft.com/library/cc246071(PROT.10).aspx) en MSDN.

Para obtener más información acerca de la secuencia de mensajes básica para la delegación de Kerberos con un vale-concesión de vales (TGT) en comparación con el servicio para las extensiones de usuario (S4U), consulte la sección [1.3.3 Introducción al protocolo](https://msdn.microsoft.com/library/cc246080(v=prot.10).aspx) en [MS-SFU]: Extensiones del protocolo Kerberos: especificación del protocolo de delegación limitada y de servicio para el usuario.

**Implicaciones de seguridad de la delegación restringida basada en recursos**

Delegación de control de PUT de delegación en manos del administrador del propietario del recurso que se obtiene acceso limitada basada en recursos. Depende de los atributos del servicio de recursos en lugar de con el servicio de confianza que se va a delegar. Como resultado, la delegación restringida basada en recursos no puede usar el bit de confianza-a-autenticar-de-delegación que anteriormente se controla la transición del protocolo. El KDC siempre permite la transición del protocolo al realizar basada en recursos de la delegación restringida como si se han establecido el bit.

Dado que el KDC no limita la transición de protocolos, se introdujeron dos nuevos SID conocido para dar a este control para el Administrador de recursos.  Estos SID identificar si se ha producido la transición del protocolo y puede utilizarse con listas de control de acceso estándar para conceder o limitar el acceso según sea necesario.

|SID|Descripción|
|-------|--------|
|AUTHENTICATION_AUTHORITY_ASSERTED_IDENTITY<br />S-1-18-1|Un SID que significa que la identidad del cliente se impone por una autoridad de autenticación en función de prueba de posesión de las credenciales del cliente.|
|SERVICE_ASSERTED_IDENTITY<br />S-1-18-2|Un SID que significa que la identidad del cliente se impone por un servicio.|

Un servicio back-end puede usar las expresiones estándar de ACL para determinar cómo se autenticó al usuario.

**¿Cómo configurar la delegación limitada basada en recursos?**

Para configurar un servicio de recurso para que permita que un servicio front-end obtenga acceso en nombre de los usuarios, use cmdlets de Windows PowerShell.

-   Para recuperar una lista de entidades de seguridad, use la **Get-ADComputer**, **Get-ADServiceAccount**, y **Get-ADUser** cmdlets con el **propiedades PrincipalsAllowedToDelegateToAccount** parámetro.

-   Para configurar el servicio de recursos, use el **New-ADComputer**, **New-ADServiceAccount**, **New-ADUser**, **Set-ADComputer**,  **Set-ADServiceAccount**, y **Set-ADUser** cmdlets con el **PrincipalsAllowedToDelegateToAccount** parámetro.

## <a name="BKMK_SOFT"></a>Requisitos de software
La delegación restringida basada en recursos solamente puede configurarse en un controlador de dominio que ejecutan Windows Server 2012 R2 y Windows Server 2012, pero puede aplicarse dentro de un bosque de modo mixto.

Debe aplicar la siguiente revisión a todos los controladores de dominio que ejecuta Windows Server 2012 en usuario dominios de cuentas en la ruta de acceso de referencia entre los dominios de front-end y back-end que ejecutan sistemas operativos anteriores a Windows Server:   un error en KDC_ERR_POLICY de delegación limitada basada en recursos en entornos que tienen controladores de dominio basados Windows Server 2008 R2.
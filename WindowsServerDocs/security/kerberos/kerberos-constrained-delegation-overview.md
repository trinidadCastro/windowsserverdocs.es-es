---
title: Kerberos Constrained Delegation Overview
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: e6e62effcb875c0e3a1cdd6c886f3d74923e1b94
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403420"
---
# <a name="kerberos-constrained-delegation-overview"></a>Kerberos Constrained Delegation Overview

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema de introducción para profesionales de TI se describen nuevas funcionalidades para la delegación limitada de Kerberos en Windows Server 2012 R2 y Windows Server 2012.

**Descripción de la característica**

La delegación limitada de Kerberos se presentó en Windows Server 2003 y proporcionaba una forma de delegación más segura que podían usar los servicios. Cuando se configura, restringe los servicios en los que puede actuar un servidor determinado en nombre de un usuario. Para esto se requieren privilegios de administrador de dominio para configurar una cuenta de dominio para un servicio y se restringe la cuenta a un único dominio. En las empresas actuales, los servicios front-end no están diseñados para limitarse a la integración con solo los servicios de su dominio.

En los sistemas operativos anteriores donde el administrador de dominio configuraba el servicio, el administrador de servicios no disponía de ninguna manera que fuera útil para saber qué servicios front-end delegaban a los servicios de recurso que poseían. Y cualquier servicio front-end que podía delegar a un servicio de recursos representaba un punto de ataque potencial. Si estaba comprometido un servidor que hospedaba un servicio front-end, y estaba configurado para delegar a servicios de recursos, los servicios de recursos también podían verse comprometidos.

En Windows Server 2012 R2 y Windows Server 2012, la capacidad de configurar la delegación restringida para el servicio se transfirió desde el administrador de dominio al administrador de servicios. De esta manera, el administrador de servicios back-end puede permitir o denegar servicios front-end.

Para obtener información detallada sobre la delegación limitada según se presentó en Windows Server 2003, consulte el tema sobre la [delegación limitada y la transición de protocolo de Kerberos](https://technet.microsoft.com/library/cc739587(v=ws.10)).

La implementación de Windows Server 2012 R2 y Windows Server 2012 del protocolo Kerberos incluye extensiones específicamente para la delegación restringida.  S4U2Proxy (del inglés Service for User to Proxy) permite que un servicio use su vale de servicio de Kerberos para que un usuario obtenga un vale de servicio del Centro de distribución de claves (KDC) para el servicio back-end. Estas extensiones permiten configurar la delegación restringida en la cuenta del servicio back-end, que puede estar en otro dominio. Para obtener más información acerca de estas extensiones, consulte [\[MS-SFU\]: extensiones de protocolo Kerberos: especificación del Protocolo de delegación limitada y servicio para el usuario](https://msdn.microsoft.com/library/cc246071(PROT.13).aspx) en MSDN Library.

**Aplicaciones prácticas**

La delegación restringida proporciona a los administradores de servicios la capacidad de especificar y exigir límites de confianza de aplicaciones limitando el ámbito en el que los servicios de aplicación pueden actuar en nombre de un usuario. Los administradores de servicio pueden configurar qué cuentas de servicio front-end pueden delegar a sus servicios back-end.

Al admitir la delegación restringida entre dominios en Windows Server 2012 R2 y Windows Server 2012, servicios front-end como Microsoft Internet Security and Acceleration (ISA) Server, Microsoft Forefront Threat Management Gateway, Microsoft Exchange Outlook Web Access (OWA) y Microsoft SharePoint Server pueden configurarse para usar la delegación restringida para autenticarse en los servidores de otros dominios. Esto proporciona compatibilidad con soluciones de servicios entre dominios mediante una infraestructura de Kerberos existente. La delegación limitada de Kerberos puede ser administrada por administradores de dominio o administradores de servicios.

## <a name="resource-based-constrained-delegation-across-domains"></a>Delegación limitada basada en recursos entre dominios

La delegación limitada de Kerberos se puede usar para proporcionar delegación limitada cuando los servicios de recurso y el servicio front-end no se encuentran en el mismo dominio. Los administradores de servicio pueden configurar la nueva delegación especificando las cuentas de dominio de los servicios front-end que pueden suplantar a los usuarios en los objetos de cuenta de los servicios de recurso.

**¿Qué valor aporta este cambio?**

Admitiendo la delegación limitada entre dominios, los servicios pueden configurarse para usar la delegación limitada para autenticar servidores de otros dominios en lugar de usar la delegación sin restricciones. Esto proporciona compatibilidad para la autenticación para soluciones de servicio entre dominios mediante el uso de una infraestructura de Kerberos existente sin necesidad de confiar en servicios front-end para delegar a cualquier servicio.

Esto también desplazará la decisión de si un servidor debe confiar en el origen de una identidad delegada del administrador de dominio que delega desde al propietario del recurso.

**¿Qué funciona de manera diferente?**

Un cambio en el protocolo subyacente permite la delegación limitada entre dominios. La implementación de Windows Server 2012 R2 y Windows Server 2012 del protocolo Kerberos incluye extensiones al protocolo Service for User to proxy (S4U2Proxy). Este es un conjunto de extensiones al protocolo de Kerberos que permite que un servicio use su vale de servicio de Kerberos para que un usuario obtenga un vale de servicio del Centro de distribución de claves (KDC) para un servicio back-end.

Para obtener información de implementación acerca de estas extensiones, consulte [\[MS-SFU\]: extensiones de protocolo Kerberos: especificación del Protocolo de delegación limitada y servicio para el usuario](https://msdn.microsoft.com/library/cc246071(PROT.10).aspx) en MSDN.

Para obtener más información acerca de la secuencia de mensajes básica para la delegación de Kerberos con un vale de concesión de vales (TGT) remitido según se compara con las extensiones de servicio para el usuario (S4U) consulte la sección [1.3.3 de introducción al protocolo](https://msdn.microsoft.com/library/cc246080(v=prot.10).aspx) en el tema [MS-SFU]: Extensiones del protocolo Kerberos: especificación del protocolo de delegación limitada y de servicio para el usuario.

**Implicaciones de seguridad de la delegación restringida basada en recursos**

La delegación restringida basada en recursos coloca el control de la delegación en manos del administrador que posee el recurso al que se accede. Depende de los atributos del servicio de recursos, en lugar de que el servicio sea de confianza para delegar. Como resultado, la delegación restringida basada en recursos no puede usar el bit de confianza para autenticarse en la delegación que anteriormente controlaba la transición del protocolo. El KDC siempre permite la transición de protocolos al realizar la delegación restringida basada en recursos como si se hubiera establecido el bit.

Dado que el KDC no limita la transición de protocolo, se introdujeron dos nuevos SID conocidos para proporcionar este control al administrador de recursos.  Estos SID identifican si se ha producido una transición de protocolo y se pueden usar con las listas de control de acceso estándar para conceder o limitar el acceso según sea necesario.

|SID|Descripción|
|-------|--------|
|AUTHENTICATION_AUTHORITY_ASSERTED_IDENTITY<br />S-1-18-1|Un SID que significa que la identidad del cliente está impuesta por una autoridad de autenticación en función de la prueba de posesión de las credenciales del cliente.|
|SERVICE_ASSERTED_IDENTITY<br />S-1-18-2|Un SID que significa que la identidad del cliente está impuesta por un servicio.|

Un servicio back-end puede usar expresiones ACL estándar para determinar cómo se autenticó el usuario.

**¿Cómo se configura la delegación restringida basada en recursos?**

Para configurar un servicio de recurso para que permita que un servicio front-end obtenga acceso en nombre de los usuarios, use cmdlets de Windows PowerShell.

-   Para recuperar una lista de entidades de seguridad, use los cmdlets **Get-ADComputer**, **Get-ADServiceAccount**y **Get-ADUser** con el parámetro **Properties PrincipalsAllowedToDelegateToAccount** .

-   Para configurar el servicio de recursos, use los cmdlets **New-ADComputer**, **New-ADServiceAccount**, **New-ADUser**, **set-ADComputer**, **set-ADServiceAccount**y **set-ADUser** con el parámetro **PrincipalsAllowedToDelegateToAccount** .

## <a name="BKMK_SOFT"></a>Requisitos de software
La delegación restringida basada en recursos solo se puede configurar en un controlador de dominio que ejecute Windows Server 2012 R2 y Windows Server 2012, pero se puede aplicar en un bosque de modo mixto.

Debe aplicar la siguiente revisión a todos los controladores de dominio que ejecutan Windows Server 2012 en dominios de cuenta de usuario en la ruta de acceso de referencia entre los dominios front-end y back-end que ejecutan sistemas operativos anteriores a Windows Server: la delegación restringida basada en recursos KDC_ERR_POLICY error en entornos que tienen controladores de dominio basados en Windows Server 2008 R2 (https://support.microsoft.com/en-gb/help/2665790/resource-based-constrained-delegation-kdc-err-policy-failure-in-enviro).

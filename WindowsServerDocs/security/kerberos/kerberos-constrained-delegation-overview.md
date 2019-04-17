---
title: "Información general de la delegación restringida de Kerberos"
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
ms.openlocfilehash: e90aa5e395fa43a3f94cccb13444fd6991ac1074
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="kerberos-constrained-delegation-overview"></a>Información general de la delegación restringida de Kerberos

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Este tema de introducción para profesionales de TI describe las nuevas funcionalidades para la delegación restringida de Kerberos en Windows Server 2012 R2 y Windows Server 2012.

## <a name="feature-description"></a>Descripción de la característica
Delegación restringida de Kerberos se presentó en Windows Server 2003 para proporcionar una forma más segura de delegación que podría ser usada por los servicios. Cuando se configura, delegación restringida restringe los servicios en la que el servidor especificado puede actuar en nombre de un usuario. Esto requiere privilegios de administrador de dominio para configurar una cuenta de dominio para un servicio y es restringe la cuenta en un solo dominio. En las empresas de hoy, servicios front-end no están diseñados para estar limitado a la integración con únicamente los servicios en su dominio.

En versiones anteriores del sistema operativo donde el Administrador de dominio configurado el servicio, el Administrador de servicio no podía útil saber qué servicios front-end delegadas a los servicios de recursos que pertenecen. Y cualquier servicio front-end que puede delegar a un servicio de recursos representa un punto de ataques potenciales. Si se ha puesto en peligro un servidor que hospeda un servicio front-end y se ha configurado para delegar a servicios de recursos, los servicios de recurso podrían también estar en riesgo.

En Windows Server 2012 R2 y Windows Server 2012, capacidad de configurar la delegación restringida para el servicio se ha transferido desde el Administrador de dominio para el Administrador de servicio. De este modo, el Administrador de servicios back-end puede permitir o denegar servicios front-end.

Para obtener información detallada acerca de la delegación restringida introducidas en Windows Server 2003, vea [transición del protocolo Kerberos y delegación restringida](https://technet.microsoft.com/library/cc739587(v=ws.10)).

La implementación de Windows Server 2012 R2 y Windows Server 2012 del protocolo Kerberos incluye extensiones específicamente para la delegación restringida.  Servicio de usuario mediante proxy (S4U2Proxy) permite a un servicio usar su vale de servicio Kerberos para un usuario para obtener un vale de servicio desde el centro de distribución de claves (KDC) a un servicio back-end. Estas extensiones permiten que la delegación restringida para configurarse en la cuenta del servicio back-end, que puede estar en otro dominio. Para obtener más información acerca de estas extensiones, consulta [\[MS-SFU\]: extensiones del protocolo Kerberos: servicio de usuario y la especificación del protocolo de la delegación restringida](https://msdn.microsoft.com/library/cc246071(PROT.13).aspx) en la biblioteca de MSDN.

## <a name="practical-applications"></a>Aplicaciones prácticas
La delegación restringida ofrece a los administradores de servicio la capacidad para especificar y aplicar los límites de confianza de la aplicación mediante la limitación del ámbito donde los servicios de la aplicación pueden actuar en nombre de un usuario. Los administradores de servicio pueden configurar que las cuentas de servicio front-end pueden delegar en sus servicios back-end.

Al admitir la delegación restringida en dominios de Windows Server 2012 R2 y Windows Server 2012, front-end servicios como Microsoft Internet Security y Acceleration (ISA) Server, Microsoft Forefront Threat Management Gateway, Outlook Web Access (OWA) de Microsoft Exchange y Microsoft SharePoint Server pueden configurarse para utilizar la delegación restringida para autenticar a los servidores de otros dominios. Esto proporciona compatibilidad para dominios de soluciones de servicio mediante el uso de una infraestructura existente de Kerberos. Delegación restringida de Kerberos puede administrarse mediante los administradores de dominio o los administradores de servicio.

## <a name="new-and-changed-functionality"></a>Funcionalidades nuevas y modificadas
**Basado en recursos de la delegación restringida entre dominios**

Delegación restringida de Kerberos puede usarse para proporcionar la delegación restringida cuando el servicio de aplicaciones y los servicios de recurso no están en el mismo dominio. Los administradores de servicio son capaces de configurar la delegación nuevo mediante la especificación de las cuentas de dominio de los servicios front-end que pueden suplantar a los usuarios en los objetos de la cuenta de los servicios de recursos.

**¿Qué valor agregar este cambio?**

Al admitir la delegación restringida entre dominios, servicios pueden configurarse para utilizar la delegación restringida para autenticar a los servidores de otros dominios en lugar de usar la delegación sin restricciones. Esto proporciona compatibilidad con autenticación a través de soluciones de servicios de dominio mediante el uso de una infraestructura de Kerberos existente sin necesidad de confiar en los servicios front-end delegar a cualquier servicio.

**¿Qué funciona de manera diferente?**

Un cambio en el protocolo subyacente permite la delegación restringida entre dominios. La implementación de Windows Server 2012 R2 y Windows Server 2012 del protocolo Kerberos incluye extensiones al servicio de usuario de protocolo de Proxy (S4U2Proxy). Este es un conjunto de extensiones en el protocolo Kerberos que permite a un servicio para usar su vale de servicio Kerberos para un usuario para obtener un vale de servicio desde el centro de distribución de claves (KDC) a un servicio back-end.

Para obtener información de implementación acerca de estas extensiones, consulta [\[MS-SFU\]: extensiones del protocolo Kerberos: servicio de usuario y la especificación del protocolo de la delegación restringida](https://msdn.microsoft.com/library/cc246071(PROT.10).aspx) en MSDN.

Para obtener más información acerca de la secuencia de mensaje básico para la delegación Kerberos con un reenviado vale (TGT) en comparación con el servicio para las extensiones de usuario (S4U), consulta la sección [1.3.3 información general de protocolo](https://msdn.microsoft.com/library/cc246080(v=prot.10).aspx) en [MS-SFU]: extensiones del protocolo Kerberos: servicio de usuario y la especificación del protocolo de la delegación restringida.

Para configurar un servicio de recursos para permitir el acceso a un front-end del servicio en nombre de los usuarios, usar cmdlets de Windows PowerShell.

-   Para recuperar una lista de principales, usa el **Get ADComputer**, **Get ADServiceAccount**, y **Get-ADUser** cmdlets con la **propiedades PrincipalsAllowedToDelegateToAccount** parámetro.

-   Para configurar el servicio de recursos, usa el **nuevo ADComputer**, **nuevo ADServiceAccount**, **nuevo-ADUser**, **conjunto ADComputer**, **conjunto ADServiceAccount**, y **Set-ADUser** cmdlets con la **PrincipalsAllowedToDelegateToAccount** parámetro.

## <a name="BKMK_SOFT"></a>Requisitos de software
La delegación restringida basado en recursos solo se puede configurar en un controlador de dominio que ejecutan Windows Server 2012 R2 y Windows Server 2012, pero puede aplicarse dentro de un bosque de modo mixto.

Debes aplicar la siguiente revisión en todos los controladores de dominio que ejecuta Windows Server 2012 en usuario dominios de cuentas en la ruta de acceso de referencias entre los dominios front-end y back-end que ejecutan sistemas operativos anteriores a Windows Server: basado en recursos de la delegación error KDC_ERR_POLICY en entornos que tienen controladores de dominio basados en Windows Server 2008 R2 restringida.

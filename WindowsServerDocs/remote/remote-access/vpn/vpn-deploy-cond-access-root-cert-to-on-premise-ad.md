---
title: Implementar certificados raíz de acceso condicional en AD local
description: ''
services: active-directory
ms.prod: windows-server-threshold
ms.technology: networking-ras
documentationcenter: ''
ms.assetid: ''
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 210540846f5d62dfc74a2e629a6b7675ccf9894d
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067479"
---
# Paso 7.4. Implementar certificados raíz de acceso condicional en local de AD

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

En este paso, se implementa el certificado raíz de acceso condicional como certificado raíz de confianza para la autenticación de VPN en tus locales AD.

& #171;  [ **Anterior:** paso 7.3. Configurar la directiva de acceso condicional](vpn-config-conditional-access-policy.md)<br>
& #187; [ **Siguiente:** paso 7.5. Crear OMA DM en función de los perfiles de VPNv2 a dispositivos Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md)

1. En la página de **conectividad VPN** , haz clic en **Descargar certificado**. 
   
    ![Descarga el certificado para el acceso condicional](../../media/Always-On-Vpn/06.png)

    >[!NOTE]
    >La opción de **Descargar base64 certificado** está disponible para algunas configuraciones que requieren base64 certificados para la implementación. 

2. Inicia sesión en un equipo unido al dominio con derechos de administrador y ejecuta estos comandos desde un símbolo del sistema de administrador para agregar los certificados raíz de la nube en el almacén *NTauth de empresa* :

    >[!NOTE]
    >Para entornos donde el servidor VPN no está unido al dominio de Active Directory, los certificados raíz de la nube deben agregarse manualmente en el almacén de _Entidades de certificación raíz de confianza_ .

    |Comando  |Descripción  |  
    |---------|-------------| 
    |`certutil -dspublish -f VpnCert.cer RootCA`     |Crea dos contenedores de **raíz generación 1 de entidad emisora de certificados de VPN de Microsoft** en la **CN = AIA** y **CN = entidades de certificación** contenedores y publica cada certificado raíz como un valor en el atributo _cACertificate_ de ambos raíz **VPN de Microsoft Generación de CA 1** contenedores.|  
    |`certutil -dspublish -f VpnCert.cer NTAuthCA`   |Crea un **CN = NTAuthCertificates** contenedor bajo el **CN = AIA** y **CN = entidades de certificación** contenedores y publica cada certificado raíz como un valor en el atributo _cACertificate_ el **CN = NTAuthCertificates** contenedor. |  
    |`gpupdate /force`     |Acelera la adición de los certificados raíz para los equipos cliente y servidor de Windows.  |

3.  Comprueba que los certificados raíz están presentes en el almacén Enterprise NTAuth y mostrar como de confianza:

    a.  Inicia sesión en un servidor con derechos de administrador que tenga instaladas las **Herramientas de administración de entidad de certificado** .

    >[!NOTE]
    >De manera predeterminada, las **Herramientas de administración de entidad de certificado** son los servidores de entidad emisora de certificados instalados. Puede instalarse en otros servidores miembros como parte de las **Herramientas de administración de roles** en el administrador del servidor.

    b.  En el servidor VPN, en el menú Inicio, escribe **pkiview.msc** para abrir el cuadro de diálogo de PKI de empresa.

    c.  En el menú Inicio, escribe **pkiview.msc** para abrir el cuadro de diálogo de PKI de empresa.

    d.  Haz clic en **PKI de empresa** y selecciona **Administrar contenedores de AD**.

    d.  Comprueba que cada certificado de generación 1 de entidad de certificación raíz de VPN de Microsoft está presente en:<ul><li>NTAuthCertificates</li><li>Contenedor AIA</li><li>Contenedor de entidades de certificación</li></ul>

    
## Paso siguiente
Paso [7.5. Crear OMA DM en función de los perfiles de VPNv2 a dispositivos Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md): en este paso, puedes crear OMA DM en función de los perfiles de VPNv2 usar Intune para implementar una directiva de configuración del dispositivo VPN. Si quieres SCCM o Script de PowerShell para crear perfiles de VPNv2, consulta la [configuración de VPNv2 CSP](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) para obtener más detalles.

---

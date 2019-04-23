---
title: Configurar plantillas de certificado para los requisitos de PEAP y EAP
description: En este tema se proporciona información sobre el uso de certificados con el servidor de directivas de red y acceso remoto en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2af0a1df-5c44-496b-ab11-5bc340dc96f0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 41f6f88705fa3d58be695fd825d960e7df21cd24
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885886"
---
# <a name="configure-certificate-templates-for-peap-and-eap-requirements"></a>Configurar plantillas de certificado para los requisitos de PEAP y EAP

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Todos los certificados que se usan para la autenticación de acceso de red con protocolo de autenticación Extensible\-Transport Layer Security \(EAP\-TLS\), protegido Protocolo de autenticación Extensible\- Seguridad de la capa de transporte \(PEAP\-TLS\)y PEAP\-protocolo de autenticación de Microsoft desafío mutuo versión 2 \(MS\-CHAP v2\) debe cumplir los requisitos de certificados X.509 y funcionar para conexiones que usan Secure Socket Layer/Transport nivel de seguridad (SSL/TLS). Certificados de cliente y servidor tienen requisitos adicionales.

>[!IMPORTANT]
>En este tema proporciona instrucciones para configurar las plantillas de certificado. Para usar estas instrucciones, es necesario que ha implementado su propia infraestructura de clave pública \(PKI\) con Active Directory Certificate Services \(AD CS\).

## <a name="minimum-server-certificate-requirements"></a>Requisitos de certificado de servidor mínima

Con PEAP\-MS\-CHAP v2, PEAP\-TLS o EAP\-TLS como método de autenticación, NPS debe usar un certificado de servidor que cumpla los requisitos de certificado de servidor mínima. 

Los equipos cliente pueden configurarse para validar los certificados de servidor mediante el **Validar certificado de servidor** opción en el equipo cliente o en la directiva de grupo. 

El equipo cliente acepta el intento de autenticación del servidor cuando el certificado de servidor cumple los requisitos siguientes:

- El nombre de sujeto contiene un valor. Si emite un certificado para el servidor que ejecuta el servidor de directivas de redes (NPS) que tiene un nombre de sujeto en blanco, el certificado no está disponible para autenticar el NPS. Para configurar la plantilla de certificado con un nombre de sujeto:

    1. Abra las plantillas de certificado.
    2. En el panel de detalles, haga clic en la plantilla de certificado que desea cambiar y, a continuación, haga clic en **propiedades** .
    3. Haga clic en el **nombre de sujeto** pestaña y, a continuación, haga clic en **construido a partir de esta información de Active Directory**.
    4. En **formato de nombre de sujeto**, seleccione un valor distinto de **ninguno**.

- El certificado de equipo en el servidor se encadena a una entidad de certificación raíz de confianza (CA) y no no fail cualquiera de las comprobaciones que realizan CryptoAPI y que se especifican en la directiva de acceso remoto o la directiva de red.

- El certificado de equipo para el servidor NPS o VPN está configurado con el propósito de autenticación de servidor en extensiones de uso mejorado de clave (EKU). (El identificador de objeto para la autenticación de servidor es 1.3.6.1.5.5.7.3.1).

- Configure el certificado de servidor con la configuración de criptografía requerida:

    1. Abra las plantillas de certificado.
    2. En el panel de detalles, haga clic en la plantilla de certificado que desea cambiar y, a continuación, haga clic en **propiedades**.
    3. Haga clic en el **criptografía** pestaña y asegúrese de que configurar lo siguiente:
       - **Categoría del proveedor:** Proveedor de almacenamiento de claves
       - **Nombre del algoritmo:** RSA
       - **Proveedores:** Proveedor de cifrado de la plataforma de Microsoft
       - **Tamaño mínimo de clave:** 2048
       - **Algoritmo hash:** SHA2
    4. Haz clic en **Siguiente**.

- La extensión de nombre alternativo del sujeto (SubjectAltName), si usa, debe contener el nombre DNS del servidor. Para configurar la plantilla de certificado con el nombre del sistema de nombres de dominio (DNS) del servidor: 

    1. Abra las plantillas de certificado.
    2. En el panel de detalles, haga clic en la plantilla de certificado que desea cambiar y, a continuación, haga clic en **propiedades** .
    3. Haga clic en el **nombre de sujeto** pestaña y, a continuación, haga clic en **construido a partir de esta información de Active Directory**.
    4. En **incluir esta información en nombre de sujeto alternativo**, seleccione **nombre DNS**.

Al usar PEAP y EAP-TLS, NPSs mostrar una lista de todos los certificados instalados en el almacén de certificados de equipo, con las siguientes excepciones:

- No se muestran los certificados que no contienen el propósito de autenticación de servidor en extensiones EKU.

- No se muestran los certificados que no contienen un nombre de sujeto.

- Basado en registro y no se muestran los certificados de inicio de sesión de tarjeta inteligente.

Para obtener más información, consulte [implementar certificados de servidor para las implementaciones inalámbricas y cableadas 802.1X](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="minimum-client-certificate-requirements"></a>Requisitos de certificados de cliente mínima

Con EAP-TLS o PEAP-TLS, el servidor acepta el intento de autenticación de cliente cuando el certificado cumple los requisitos siguientes:

- El certificado de cliente es emitido por una CA de empresa o asignado a una cuenta de usuario o equipo en Active Directory Domain Services \(AD DS\).

- El certificado de equipo o usuario en las cadenas de cliente a una entidad emisora de certificados raíz de confianza incluye el propósito de autenticación del cliente en extensiones EKU \(el identificador de objeto para la autenticación de cliente es 1.3.6.1.5.5.7.3.2\)y se produce un error ni el comprobaciones que realiza CryptoAPI y que se especifican en la directiva de acceso remoto o directiva de red ni las comprobaciones de identificador de objeto de certificado que se especifican en la directiva de red NPS.

- El cliente 802.1X no usa certificados basados en el registro ya sea de sesión con tarjetas inteligentes o certificados protegidos con contraseña.

- Para los certificados de usuario, nombre alternativo del firmante \(SubjectAltName\) extensión del certificado contiene el nombre principal de usuario \(UPN\). Para configurar el UPN en una plantilla de certificado:

    1. Abra las plantillas de certificado.
    2. En el panel de detalles, haga clic en la plantilla de certificado que desea cambiar y, a continuación, haga clic en **propiedades**.
    3. Haga clic en el **nombre de sujeto** pestaña y, a continuación, haga clic en **construido a partir de esta información de Active Directory**.
    4. En **incluir esta información en nombre de sujeto alternativo**, seleccione **nombre principal de usuario \(UPN\)**.

- Los certificados de equipo, nombre alternativo del firmante \(SubjectAltName\) extensión del certificado debe contener el nombre de dominio completo \(FQDN\) del cliente, que también se denomina el  *Nombre DNS*. Para configurar este nombre en la plantilla de certificado:

    1. Abra las plantillas de certificado.
    2. En el panel de detalles, haga clic en la plantilla de certificado que desea cambiar y, a continuación, haga clic en **propiedades**.
    3. Haga clic en el **nombre de sujeto** pestaña y, a continuación, haga clic en **construido a partir de esta información de Active Directory**.
    4. En **incluir esta información en nombre de sujeto alternativo**, seleccione **nombre DNS**.

Con PEAP\-TLS y EAP\-TLS, el cliente muestra una lista de todos los certificados instalados en el complemento certificados, con las siguientes excepciones:

- Los clientes inalámbricos no muestran basada en registros y los certificados de inicio de sesión de tarjeta inteligente. 

- Los clientes inalámbricos y los clientes VPN no muestran certificados protegidos con contraseña. 

- No se muestran los certificados que no contienen el propósito de autenticación del cliente en extensiones EKU.


Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).

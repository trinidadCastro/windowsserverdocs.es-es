---
title: Configurar plantillas de certificado para los requisitos de PEAP y EAP
description: En este tema se proporciona información acerca del uso de certificados con el servidor de directivas de redes y el acceso remoto en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 2af0a1df-5c44-496b-ab11-5bc340dc96f0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f60e5a1da1388a6dd2432a3603f83d6ca2990a29
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405405"
---
# <a name="configure-certificate-templates-for-peap-and-eap-requirements"></a>Configurar plantillas de certificado para los requisitos de PEAP y EAP

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Todos los certificados que se usan para la autenticación de acceso a la red con el protocolo de autenticación extensible @ no__t-0Transport seguridad de capa \(EAP @ no__t-2TLS @ no__t-3, protocolo de autenticación extensible protegido @ no__t-4Transport seguridad de capa \(PEAP @ no__t-6TLS @ no__t-7 y PEAP @ no__t-8Microsoft protocolo de autenticación por desafío mutuo versión 2 \(MS @ no__t-10CHAP V2 @ no__t-11 debe cumplir los requisitos de los certificados X. 509 y el trabajo de las conexiones que usan socket seguro Seguridad de nivel de transporte (SSL/TLS). Los certificados de cliente y de servidor tienen requisitos adicionales.

>[!IMPORTANT]
>En este tema se proporcionan instrucciones para configurar plantillas de certificado. Para usar estas instrucciones, es necesario haber implementado su propia infraestructura de clave pública \(PKI @ no__t-1 con Active Directory servicios de Certificate Server \(AD CS @ no__t-3.

## <a name="minimum-server-certificate-requirements"></a>Requisitos mínimos de certificados de servidor

Con PEAP @ no__t-0 ms @ no__t-1CHAP V2, PEAP @ no__t-2TLS o EAP @ no__t-3TLS como método de autenticación, el NPS debe usar un certificado de servidor que cumpla los requisitos mínimos de certificados de servidor. 

Los equipos cliente se pueden configurar para validar certificados de servidor mediante la opción **validar certificado de servidor** en el equipo cliente o en Directiva de grupo. 

El equipo cliente acepta el intento de autenticación del servidor cuando el certificado de servidor cumple los requisitos siguientes:

- El nombre de sujeto contiene un valor. Si emite un certificado para el servidor que ejecuta el servidor de directivas de redes (NPS) que tiene un nombre de sujeto en blanco, el certificado no está disponible para autenticar el NPS. Para configurar la plantilla de certificado con un nombre de sujeto:

    1. Abra las plantillas de certificado.
    2. En el panel de detalles, haga clic con el botón secundario en la plantilla de certificado que desea cambiar y, a continuación, haga clic en **propiedades** .
    3. Haga clic en la pestaña **nombre de sujeto** y, a continuación, haga clic en **compilar a partir de esta información Active Directory**.
    4. En **formato de nombre de sujeto**, seleccione un valor distinto de **ninguno**.

- El certificado de equipo en el servidor se encadena a una entidad de certificación (CA) raíz de confianza y no genera ningún error en ninguna de las comprobaciones realizadas por CryptoAPI y que se especifican en la Directiva de acceso remoto o en la Directiva de red.

- El certificado de equipo para el servidor NPS o VPN se configura con el propósito de autenticación de servidor en extensiones de uso mejorado de clave (EKU). (El identificador de objeto para la autenticación del servidor es 1.3.6.1.5.5.7.3.1).

- Configure el certificado de servidor con la configuración de criptografía necesaria:

    1. Abra las plantillas de certificado.
    2. En el panel de detalles, haga clic con el botón secundario en la plantilla de certificado que desea cambiar y, a continuación, haga clic en **propiedades**.
    3. Haga clic en la pestaña **Criptografía** y asegúrese de configurar lo siguiente:
       - **Categoría de proveedor:** Proveedor de almacenamiento de claves
       - **Nombre del algoritmo:** RSA
       - **Presta** Proveedor de cifrado de plataforma de Microsoft
       - **Tamaño mínimo de clave:** 2048
       - **Algoritmo hash:** SHA2
    4. Haz clic en **Siguiente**.

- La extensión de nombre alternativo del firmante (SubjectAltName), si se utiliza, debe contener el nombre DNS del servidor. Para configurar la plantilla de certificado con el nombre del sistema de nombres de dominio (DNS) del servidor de inscripción: 

    1. Abra las plantillas de certificado.
    2. En el panel de detalles, haga clic con el botón secundario en la plantilla de certificado que desea cambiar y, a continuación, haga clic en **propiedades** .
    3. Haga clic en la pestaña **nombre de sujeto** y, a continuación, haga clic en **compilar a partir de esta información Active Directory**.
    4. En **incluir esta información en un nombre de sujeto alternativo**, seleccione **nombre DNS**.

Al usar PEAP y EAP-TLS, NPSs muestra una lista de todos los certificados instalados en el almacén de certificados del equipo, con las siguientes excepciones:

- No se muestran los certificados que no contienen el propósito de autenticación de servidor en extensiones EKU.

- No se muestran los certificados que no contienen un nombre de sujeto.

- No se muestran los certificados de inicio de sesión de tarjeta inteligente y basados en el registro.

Para obtener más información, consulte [implementación de certificados de servidor para implementaciones cableadas e inalámbricas de 802.1 x](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="minimum-client-certificate-requirements"></a>Requisitos mínimos de certificados de cliente

Con EAP-TLS o PEAP-TLS, el servidor acepta el intento de autenticación del cliente cuando el certificado cumple los siguientes requisitos:

- El certificado de cliente lo emite una CA de empresa o se asigna a una cuenta de usuario o de equipo en Active Directory Domain Services \(AD DS @ no__t-1.

- El certificado de usuario o de equipo en el cliente se encadena a una CA raíz de confianza, incluye el propósito de autenticación del cliente en extensiones EKU @no__t el identificador de objeto para la autenticación del cliente es 1.3.6.1.5.5.7.3.2 @ no__t-1 y no supera las comprobaciones realizada por CryptoAPI y que se especifican en la Directiva de acceso remoto o en la Directiva de red, y las comprobaciones de identificador de objeto de certificado que se especifican en la Directiva de red NPS.

- El cliente de 802.1 X no usa certificados basados en el registro que sean certificados de inicio de sesión de tarjeta inteligente o protegidos por contraseña.

- En el caso de los certificados de usuario, la extensión de nombre alternativo del sujeto \(SubjectAltName @ no__t-1 del certificado contiene el nombre principal de usuario \(UPN @ no__t-3. Para configurar el UPN en una plantilla de certificado:

    1. Abra las plantillas de certificado.
    2. En el panel de detalles, haga clic con el botón secundario en la plantilla de certificado que desea cambiar y, a continuación, haga clic en **propiedades**.
    3. Haga clic en la pestaña **nombre de sujeto** y, a continuación, haga clic en **compilar a partir de esta información Active Directory**.
    4. En **incluir esta información en un nombre de sujeto alternativo**, seleccione **nombre principal de usuario \(UPN @ no__t-3**.

- En el caso de los certificados de equipo, la extensión de nombre alternativo del sujeto \(SubjectAltName @ no__t-1 del certificado debe contener el nombre de dominio completo \(FQDN @ no__t-3 del cliente, que también se denomina *nombre DNS*. Para configurar este nombre en la plantilla de certificado:

    1. Abra las plantillas de certificado.
    2. En el panel de detalles, haga clic con el botón secundario en la plantilla de certificado que desea cambiar y, a continuación, haga clic en **propiedades**.
    3. Haga clic en la pestaña **nombre de sujeto** y, a continuación, haga clic en **compilar a partir de esta información Active Directory**.
    4. En **incluir esta información en un nombre de sujeto alternativo**, seleccione **nombre DNS**.

Con PEAP @ no__t-0TLS y EAP @ no__t-1TLS, los clientes muestran una lista de todos los certificados instalados en el complemento certificados, con las siguientes excepciones:

- Los clientes inalámbricos no muestran certificados de inicio de sesión de tarjeta inteligente y basados en el registro. 

- Los clientes inalámbricos y los clientes VPN no muestran los certificados protegidos mediante contraseña. 

- No se muestran los certificados que no contienen el propósito de autenticación del cliente en las extensiones EKU.


Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).

---
title: Configurar plantillas de certificado para los requisitos de PEAP y EAP
description: En este tema se proporciona información acerca del uso de certificados con el servidor de directivas de redes y el acceso remoto en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 2af0a1df-5c44-496b-ab11-5bc340dc96f0
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 4f1680c37510a0a45dfc4ce1ce34ca62859a8f09
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948991"
---
# <a name="configure-certificate-templates-for-peap-and-eap-requirements"></a>Configurar plantillas de certificado para los requisitos de PEAP y EAP

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Todos los certificados que se usan para la autenticación de acceso a la red con el protocolo de autenticación extensible \- seguridad \( EAP \- TLS \) , protocolo de autenticación extensible protegido seguridad de la \- capa de transporte \( PEAP \- TLS y \) PEAP protocolo de autenticación por \- desafío mutuo de Microsoft versión 2 \( MS \- CHAP V2 \) deben cumplir los requisitos de los certificados X. 509 y el trabajo de las conexiones que utilizan la seguridad de nivel de sockets/TLS ( Los certificados de cliente y de servidor tienen requisitos adicionales.

>[!IMPORTANT]
>En este tema se proporcionan instrucciones para configurar plantillas de certificado. Para usar estas instrucciones, es necesario haber implementado su propia infraestructura de clave pública \( PKI \) con Active Directory servicios de Certificate Server \( ad CS \) .

## <a name="minimum-server-certificate-requirements"></a>Requisitos mínimos para certificados de servidor

Con PEAP \- MS \- CHAP V2, PEAP \- TLS o EAP \- TLS como método de autenticación, el NPS debe usar un certificado de servidor que cumpla los requisitos mínimos de certificados de servidor.

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
       - **Proveedores:** Proveedor de cifrado de plataforma de Microsoft
       - **Tamaño mínimo de clave:** 2048
       - **Algoritmo hash:** SHA2
    4. Haga clic en **Next**.

- Si se usa la extensión de nombre alternativo de sujeto (SubjectAltName), debe contener el nombre DNS del servidor. Para configurar la plantilla de certificado con el nombre del sistema de nombres de dominio (DNS) del servidor de inscripción:

    1. Abra las plantillas de certificado.
    2. En el panel de detalles, haga clic con el botón secundario en la plantilla de certificado que desea cambiar y, a continuación, haga clic en **propiedades** .
    3. Haga clic en la pestaña **nombre de sujeto** y, a continuación, haga clic en **compilar a partir de esta información Active Directory**.
    4. En **incluir esta información en un nombre de sujeto alternativo**, seleccione **nombre DNS**.

Al usar PEAP y EAP-TLS, NPSs muestra una lista de todos los certificados instalados en el almacén de certificados del equipo, con las siguientes excepciones:

- No se muestran los certificados que no contienen el propósito de autenticación de servidor en extensiones EKU.

- No se muestran los certificados que no contienen un nombre de sujeto.

- No se muestran los certificados de inicio de sesión de tarjeta inteligente y basados en el Registro.

Para obtener más información, consulte [implementación de certificados de servidor para implementaciones cableadas e inalámbricas de 802.1 x](../../core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments.md).

## <a name="minimum-client-certificate-requirements"></a>Requisitos mínimos para certificados de cliente

Con EAP-TLS o PEAP-TLS, el servidor acepta el intento de autenticación del cliente cuando el certificado cumple los siguientes requisitos:

- El certificado de cliente lo emite una CA de empresa o se asigna a una cuenta de usuario o de equipo en Active Directory Domain Services \( AD DS \) .

- El certificado de usuario o de equipo en el cliente se encadena a una CA raíz de confianza, incluye el propósito de autenticación del cliente en extensiones EKU. \( el identificador de objeto para la autenticación del cliente es 1.3.6.1.5.5.7.3.2 \) y no supera las comprobaciones realizadas por CryptoAPI y que se especifican en la Directiva de acceso remoto o en la Directiva de red, ni las comprobaciones del identificador de objeto de certificado que se

- El cliente 802.1X no usa certificados basados en el Registro que sean certificados de inicio de sesión de tarjeta inteligente o protegidos con contraseña.

- En el caso de los certificados de usuario, la extensión de nombre alternativo del firmante \( SubjectAltName del \) certificado contiene el UPN del nombre principal del usuario \( \) . Para configurar el UPN en una plantilla de certificado:

    1. Abra las plantillas de certificado.
    2. En el panel de detalles, haga clic con el botón secundario en la plantilla de certificado que desea cambiar y, a continuación, haga clic en **propiedades**.
    3. Haga clic en la pestaña **nombre de sujeto** y, a continuación, haga clic en **compilar a partir de esta información Active Directory**.
    4. En **incluir esta información en un nombre de sujeto alternativo**, seleccione **nombre principal de usuario \( UPN \)**.

- En el caso de los certificados de equipo, la extensión SubjectAltName del nombre alternativo del sujeto del \( \) certificado debe contener el FQDN del nombre de dominio completo \( \) del cliente, que también se denomina *nombre DNS*. Para configurar este nombre en la plantilla de certificado:

    1. Abra las plantillas de certificado.
    2. En el panel de detalles, haga clic con el botón secundario en la plantilla de certificado que desea cambiar y, a continuación, haga clic en **propiedades**.
    3. Haga clic en la pestaña **nombre de sujeto** y, a continuación, haga clic en **compilar a partir de esta información Active Directory**.
    4. En **incluir esta información en un nombre de sujeto alternativo**, seleccione **nombre DNS**.

Con PEAP \- TLS y EAP \- TLS, los clientes muestran una lista de todos los certificados instalados en el complemento certificados, con las siguientes excepciones:

- Los clientes inalámbricos no muestran certificados de inicio de sesión de tarjeta inteligente y basados en el Registro.

- Los clientes inalámbricos y los clientes VPN no muestran certificados protegidos con contraseña.

- No se muestran los certificados que no contienen el propósito de autenticación de cliente en extensiones EKU.


Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).
---
title: Configurar las plantillas de certificado para PEAP y los requisitos de EAP
description: Este tema proporciona información sobre el uso de certificados con el servidor de directivas de red y acceso remoto en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2af0a1df-5c44-496b-ab11-5bc340dc96f0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e597a65982aeead907c9a41f17f1785a0bf81016
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="configure-certificate-templates-for-peap-and-eap-requirements"></a>Configurar las plantillas de certificado para PEAP y los requisitos de EAP

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Todos los certificados que se usan para la autenticación de acceso de red con seguridad de la capa de transporte de Protocol\ de autenticación Extensible \(EAP\-TLS\), protegido Extensible Authentication Protocol\ transporte Layer Security \(PEAP\-TLS\) y versión del protocolo de autenticación por desafío mutuo de PEAP\ Microsoft 2 \ (v2\ MS\ CHAP) deben cumplir los requisitos para certificados X.509 y el trabajo para las conexiones que utilizan seguro de seguridad de nivel del transporte/capa de Sockets (SSL/TLS). Los certificados de cliente y servidor tienen requisitos adicionales.

>[!IMPORTANT]
>Este tema proporciona instrucciones para configurar las plantillas de certificado. Para usar estas instrucciones, es necesario que ha implementado tu propio \(PKI\) infraestructura de clave pública con los servicios de certificados de Active Directory \(AD CS\).

## <a name="minimum-server-certificate-requirements"></a>Requisitos de certificado de servidor mínimo

Con PEAP\-MS\-CHAP v2, PEAP\ TLS o EAP\-TLS como método de autenticación, el servidor NPS debe usar un certificado de servidor que cumpla los requisitos de certificado de servidor mínimo. 

Los equipos cliente pueden configurarse para validar certificados de servidor mediante la **Validar certificado de servidor** opción en el equipo cliente o en la directiva de grupo. 

El equipo cliente acepta el intento de autenticación del servidor si el certificado de servidor cumple los requisitos siguientes:

- El nombre del sujeto contiene un valor. Si se emite un certificado al servidor que ejecuta el servidor de directivas de redes (NPS) que tiene un nombre de asunto en blanco, el certificado no está disponible para autenticar el servidor NPS. Para configurar la plantilla de certificado con un nombre de asunto:

    1. Abre las plantillas de certificado.
    2. En el panel de detalles, haz clic en la plantilla de certificado que quieras cambiar y, a continuación, haz clic en **propiedades**.
    3. Haz clic en el **nombre de sujeto** pestaña y, a continuación, haz clic en **construido a partir de esta información de Active Directory**.
    4. En **formato de nombre de asunto**, seleccione un valor distinto de **ninguno**.

- El certificado de equipo en las cadenas de servidor a una entidad de certificación (CA) de raíz de confianza y no no errores cualquiera de los controles que se realizan por CryptoAPI y que se especifican en la directiva de acceso remoto o de la directiva de red.

- El certificado de equipo para el servidor NPS o el servidor VPN se configura con el propósito de autenticación de servidores en extensiones de uso extendido de claves (EKU). (El identificador de objeto para la autenticación de servidor es 1.3.6.1.5.5.7.3.1).

- El certificado de servidor está configurado con un valor de algoritmo obligatorio **RSA**. Para configurar la configuración de cifrado necesarios:

    1. Abre las plantillas de certificado.
    2. En el panel de detalles, haz clic en la plantilla de certificado que quieras cambiar y, a continuación, haz clic en **propiedades**.
    3. Haz clic en el **criptografía** pestaña. En **nombre del algoritmo**, haz clic en **RSA**. Asegúrate de que **tamaño mínimo de clave** se establece en **2048**.

- La extensión de nombre alternativo del sujeto (SubjectAltName), si usa, debe contener el nombre DNS del servidor. Para configurar la plantilla de certificado con el nombre del sistema de nombres de dominio (DNS) del servidor de inscripción: 

    1. Abre las plantillas de certificado.
    2. En el panel de detalles, haz clic en la plantilla de certificado que quieras cambiar y, a continuación, haz clic en **propiedades**.
    3. Haz clic en el **nombre de sujeto** pestaña y, a continuación, haz clic en **construido a partir de esta información de Active Directory**.
    4. En **incluir esta información en el nombre de sujeto alternativo**, selecciona **nombre DNS**.

Al usar PEAP y EAP-TLS, servidores NPS mostrar una lista de todos los certificados instalados en el almacén de certificados de equipo, con las siguientes excepciones:

- No se muestran los certificados que no contienen el propósito de autenticación de servidores en las extensiones EKU.

- No se muestran los certificados que no contienen un nombre de asunto.

- En función del registro y no se muestran los certificados de inicio de sesión de tarjeta inteligente.

Para obtener más información, consulta [implementar certificados de servidor para implementaciones de conexión inalámbrica y cableadas 802.1X](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="minimum-client-certificate-requirements"></a>Requisitos de certificados de cliente mínimo

Con EAP-TLS o PEAP-TLS, el servidor acepta el intento de autenticación de cliente cuando el certificado cumple los requisitos siguientes:

- El certificado de cliente es emitido por una CA de empresa o asignado a una cuenta de usuario o equipo en Active Directory Domain Services \(AD DS\).

- El certificado de usuario o equipo en las cadenas de cliente a una CA de raíz de confianza incluye el propósito de autenticación de cliente en las extensiones EKU \ (el identificador de objeto para la autenticación de cliente es 1.3.6.1.5.5.7.3.2\) y se produce un error de los controles que se realizan por CryptoAPI y que se especifican en la directiva de acceso remoto o de la directiva de red ni las comprobaciones de identificador de objeto de certificado que se especifican en la directiva de red NPS.

- El cliente 802.1X no usa certificados basados en el registro que están bien tarjeta inteligente inicio de sesión o protegida por contraseña.

- Para los certificados de usuario, la extensión de nombre alternativo del sujeto \(SubjectAltName\) en el certificado contiene la entidad de seguridad de usuario \(UPN\) el nombre. Para configurar el UPN en una plantilla de certificado:

    1. Abre las plantillas de certificado.
    2. En el panel de detalles, haz clic en la plantilla de certificado que quieras cambiar y, a continuación, haz clic en **propiedades**.
    3. Haz clic en el **nombre de sujeto** pestaña y, a continuación, haz clic en **construido a partir de esta información de Active Directory**.
    4. En **incluir esta información en el nombre de sujeto alternativo**, selecciona **principal del usuario nombre \(UPN\)**.

- Para los certificados de equipo, la extensión de nombre alternativo del sujeto \(SubjectAltName\) en el certificado debe contener el nombre de dominio completo nombre \(FQDN\) del cliente, que también se denomina la *nombre DNS*. Para configurar este nombre en la plantilla de certificado:

    1. Abre las plantillas de certificado.
    2. En el panel de detalles, haz clic en la plantilla de certificado que quieras cambiar y, a continuación, haz clic en **propiedades**.
    3. Haz clic en el **nombre de sujeto** pestaña y, a continuación, haz clic en **construido a partir de esta información de Active Directory**.
    4. En **incluir esta información en el nombre de sujeto alternativo**, selecciona **nombre DNS**.

Con PEAP\-TLS y EAP\ TLS, los clientes de mostrar una lista de todos los certificados instalados en el complemento certificados, con las siguientes excepciones:

- Los clientes inalámbricos no se muestran en función del registro y los certificados de inicio de sesión de tarjeta inteligente. 

- Los clientes inalámbricos y los clientes VPN no se muestran certificados protegidos por contraseña. 

- No se muestran los certificados que no contienen el propósito de autenticación de cliente en las extensiones EKU.


Para obtener más información acerca de NPS, consulta [servidor de directivas de redes (NPS)](nps-top.md).

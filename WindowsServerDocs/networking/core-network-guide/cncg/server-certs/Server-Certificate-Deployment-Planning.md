---
title: Planeación de la implementación del certificado de servidor
description: Este tema forma parte de la guía de implementación de certificados de servidor para las implementaciones cableadas e inalámbricas de 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: 7eb746e0-1046-4123-b532-77d5683ded44
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d73cee05c36176755e70796c1190620223327306
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87949359"
---
# <a name="server-certificate-deployment-planning"></a>Planeación de la implementación del certificado de servidor

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Antes de implementar los certificados de servidor, debe planear los siguientes elementos:

- [Planear la configuración básica del servidor](#bkmk_basic)

- [Planear el acceso a dominios](#bkmk_domain)

- [Planear la ubicación y el nombre del directorio virtual en el servidor Web](#bkmk_virtual)

- [Planear un registro de alias DNS (CNAME) para el servidor Web](#bkmk_cname)

- [Planear la configuración de CAPolicy. inf](#bkmk_capolicy)

- [Planeación de la configuración de las extensiones de CDP y AIA en CA1](#bkmk_cdp)

- [Planeación de la operación de copia entre la CA y el servidor Web](#bkmk_copy)

- [Planear la configuración de la plantilla de certificado de servidor en la CA](#bkmk_template)

## <a name="plan-basic-server-configuration"></a><a name="bkmk_basic"></a>Planear la configuración básica del servidor
Después de instalar Windows Server 2016 en los equipos que piensa usar como entidad de certificación y servidor Web, debe cambiar el nombre del equipo y asignar y configurar una dirección IP estática para el equipo local.

Para obtener más información, consulte la guía de [red principal](../../../core-network-guide/Core-Network-Guide.md)de Windows Server 2016.

## <a name="plan-domain-access"></a><a name="bkmk_domain"></a>Planear el acceso a dominios
Para iniciar sesión en el dominio, el equipo debe ser miembro del dominio y la cuenta de usuario se debe crear en AD DS antes del intento de inicio de sesión. Además, la mayoría de los procedimientos de esta guía requieren que la cuenta de usuario sea miembro de los grupos administradores de organización o Admins. del dominio en Active Directory usuarios y equipos, por lo que debe iniciar sesión en la entidad de certificación con una cuenta que tenga la pertenencia al grupo adecuada.

Para obtener más información, consulte la guía de [red principal](../../../core-network-guide/Core-Network-Guide.md)de Windows Server 2016.

## <a name="plan-the-location-and-name-of-the-virtual-directory-on-your-web-server"></a><a name="bkmk_virtual"></a>Planear la ubicación y el nombre del directorio virtual en el servidor Web
Para proporcionar acceso a la CRL y al certificado de la entidad de certificación en otros equipos, debe almacenar estos elementos en un directorio virtual del servidor Web. En esta guía, el directorio virtual se encuentra en el servidor Web WEB1. Esta carpeta se encuentra en la unidad "C:" y se denomina "PKI". Puede buscar el directorio virtual en el servidor Web en cualquier ubicación de carpeta que sea adecuada para su implementación.

## <a name="plan-a-dns-alias-cname-record-for-your-web-server"></a><a name="bkmk_cname"></a>Planear un registro de alias DNS (CNAME) para el servidor Web
Los registros de recursos de alias (CNAME) a veces se denominan registros de recursos de nombre canónico. Con estos registros, puede usar más de un nombre para señalar a un solo host, lo que facilita tareas como el hospedaje de un servidor FTP (Protocolo de transferencia de archivos) y un servidor web en el mismo equipo. Por ejemplo, los nombres de servidor conocidos (FTP, www) se registran mediante registros de recursos de alias (CNAME) que se asignan al nombre de host del sistema de nombres de dominio (DNS), como WEB1, para el equipo servidor que hospeda estos servicios.

En esta guía se proporcionan instrucciones para configurar el servidor web para hospedar la lista de revocación de certificados (CRL) de la entidad de certificación (CA). Dado que es posible que también desee usar el servidor web para otros propósitos, como hospedar un sitio web o FTP, es una buena idea crear un registro de recursos de alias en DNS para el servidor Web. En esta guía, el registro CNAME se denomina "PKI", pero puede elegir un nombre que sea adecuado para su implementación.

## <a name="plan-configuration-of-capolicyinf"></a><a name="bkmk_capolicy"></a>Planear la configuración de CAPolicy. inf
Antes de instalar AD CS, debe configurar CAPolicy. inf en la CA con información que sea correcta para su implementación. Un archivo CAPolicy. inf contiene la siguiente información:

```
[Version]
Signature="$Windows NT$"
[PolicyStatementExtension]
Policies=InternalPolicy
[InternalPolicy]
OID=1.2.3.4.1455.67.89.5
Notice="Legal Policy Statement"
URL=https://pki.corp.contoso.com/pki/cps.txt
[Certsrv_Server]
RenewalKeyLength=2048
RenewalValidityPeriod=Years
RenewalValidityPeriodUnits=5
CRLPeriod=weeks
CRLPeriodUnits=1
LoadDefaultTemplates=0
AlternateSignatureAlgorithm=1
```

Debe planear los siguientes elementos para este archivo:

- **Dirección URL**. El archivo CAPolicy. inf de ejemplo tiene un valor de dirección URL de **https://pki.corp.contoso.com/pki/cps.txt** . Esto se debe a que el servidor Web de esta guía se denomina WEB1 y tiene un registro de recursos CNAME de DNS de PKI. El servidor web también se une al dominio corp.contoso.com. Además, hay un directorio virtual en el servidor Web denominado "PKI" donde se almacena la lista de revocación de certificados. Asegúrese de que el valor que proporciona para la dirección URL en el archivo CAPolicy. inf apunta a un directorio virtual del servidor Web de su dominio.

- **RenewalKeyLength**. La longitud de clave de renovación predeterminada para AD CS en Windows Server 2012 es 2048. La longitud de la clave que seleccione debe ser lo más larga posible y, al mismo tiempo, proporcionar compatibilidad con las aplicaciones que piensa usar.

- **RenewalValidityPeriodUnits**. El archivo CAPolicy. inf de ejemplo tiene un valor RenewalValidityPeriodUnits de 5 años. Esto se debe a que la duración esperada de la CA es de unos diez años. El valor de RenewalValidityPeriodUnits debe reflejar el período de validez total de la CA o el número más alto de años para los que desea proporcionar inscripción.

- **CRLPeriodUnits**. El archivo CAPolicy. inf de ejemplo tiene un valor de CRLPeriodUnits de 1. Esto se debe a que el intervalo de actualización de ejemplo para la lista de revocación de certificados en esta guía es 1 semana. En el valor de intervalo que especifique con esta configuración, debe publicar la CRL en la CA en el directorio virtual del servidor Web en el que almacena la CRL y proporciona acceso a ella para los equipos que están en el proceso de autenticación.

- **AlternateSignatureAlgorithm**. Este archivo CAPolicy. inf implementa un mecanismo de seguridad mejorado implementando formatos de firma alternativos. No debe implementar esta configuración si todavía tiene clientes de Windows XP que requieren certificados de esta CA.

Si no tiene previsto agregar CA subordinadas a la infraestructura de clave pública en otro momento, y si desea evitar la adición de las CA subordinadas, puede Agregar la clave PathLength al archivo CAPolicy. inf con un valor de 0. Para agregar esta clave, copie y pegue el siguiente código en el archivo:

```
[BasicConstraintsExtension]
PathLength=0
Critical=Yes
```

> [!IMPORTANT]
> No se recomienda cambiar otras opciones del archivo CAPolicy. inf a menos que tenga una razón concreta para hacerlo.

## <a name="plan-configuration-of-the-cdp-and-aia-extensions-on-ca1"></a><a name="bkmk_cdp"></a>Planeación de la configuración de las extensiones de CDP y AIA en CA1
Al configurar el punto de distribución de la lista de revocación de certificados (CRL) y la configuración de acceso a la información de entidad (AIA) en CA1, necesitará el nombre del servidor Web y el nombre de dominio. También necesita el nombre del directorio virtual que crea en el servidor web donde se almacenan la lista de revocación de certificados (CRL) y el certificado de la entidad de certificación.

La ubicación de CDP que debe especificar durante este paso de implementación tiene el formato:

`http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl.`

Por ejemplo, si el servidor Web se denomina WEB1 y el registro CNAME de alias DNS para el servidor web es "PKI", el dominio es corp.contoso.com y el directorio virtual se denomina PKI, la ubicación de CDP es:

`http:\/\/pki.corp.contoso.com\/pki\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`

La ubicación de AIA que debe escribir tiene el formato:

`http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<ServerDNSName>\_<CaName><CertificateName>.crt.`

Por ejemplo, si el servidor Web se denomina WEB1 y el registro CNAME de alias DNS para el servidor web es "PKI", el dominio es corp.contoso.com y el directorio virtual se denomina PKI, la ubicación de AIA es:

`http:\/\/pki.corp.contoso.com\/pki\/<ServerDNSName>\_<CaName><CertificateName>.crt`

## <a name="plan-the-copy-operation-between-the-ca-and-the-web-server"></a><a name="bkmk_copy"></a>Planeación de la operación de copia entre la CA y el servidor Web
Para publicar la CRL y el certificado de CA de la CA en el directorio virtual del servidor Web, puede ejecutar el comando certutil-CRL después de configurar las ubicaciones de CDP y AIA en la CA. Asegúrese de configurar las rutas de acceso correctas en la pestaña **extensiones** de las propiedades de la entidad de certificación antes de ejecutar este comando siguiendo las instrucciones de esta guía. Además, para copiar el certificado de entidad de certificación empresarial en el servidor Web, debe haber creado el directorio virtual en el servidor Web y configurado la carpeta como una carpeta compartida.

## <a name="plan-the-configuration-of-the-server-certificate-template-on-the-ca"></a><a name="bkmk_template"></a>Planear la configuración de la plantilla de certificado de servidor en la CA
Para implementar certificados de servidor de inscripción automática, debe copiar la plantilla de certificado denominada **RAS e IAS Server**. De forma predeterminada, esta copia se denomina **copia del servidor RAS e IAS**. Si desea cambiar el nombre de esta copia de plantilla, planee el nombre que desea usar durante este paso de implementación.

> [!NOTE]
> Las tres últimas secciones de la implementación de esta guía, que le permiten configurar la inscripción automática de certificados de servidor, actualizar directiva de grupo en servidores y comprobar que los servidores hayan recibido un certificado de servidor válido de la entidad de certificación; no requieren pasos de planeación adicionales.

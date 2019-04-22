---
ms.assetid: 2a5d5271-6ac6-4c1b-b4ef-9b568932a55a
title: Actualizaciones del esquema de todo el dominio de Active Directory
description: Actualizaciones del esquema de todo el dominio realizadas adprep /domainprep al promocionar un controlador de dominio
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 10/29/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 20598431b9c2376051247fd7ba14e33af00968cc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812326"
---
# <a name="domain-wide-schema-updates"></a>Actualizaciones del esquema de todo el dominio

>Se aplica a: Windows Server

Puede revisar el siguiente conjunto de cambios para ayudar a entender y prepararse para las actualizaciones del esquema que realizan adprep /domainprep en Windows Server.

A partir de Windows Server 2012, comandos Adprep se ejecutan automáticamente según sea necesario durante la instalación de AD DS. También se puede ejecutar por separado antes de la instalación de AD DS. Para obtener más información, consulta [Ejecutar Adprep.exe](https://technet.microsoft.com/library/dd464018(v=ws.10).aspx).

Para obtener más información sobre cómo interpretar las cadenas de entrada (ACE) del control de acceso, consulte [cadenas ACE](https://msdn.microsoft.com/library/aa374928(VS.85).aspx). Para obtener más información sobre cómo interpretar las cadenas de identificador (SID) de seguridad, consulte [cadenas SID](https://msdn.microsoft.com/library/aa379602(VS.85).aspx).

## <a name="windows-server-semi-annual-channel-domain-wide-updates"></a>Windows Server (canal semianual): Actualizaciones para todo el dominio

Después de las operaciones realizadas por **domainprep** en Windows server2016 (operación 89) haya finalizado, el **revisión** atributo CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = Objeto dominioRaízDelBosque se establece en **16**.

|Número de operaciones y el GUID|Descripción|Permisos|
|------------------------------|---------------|--------------|---------------|
|**Operación 89**: {A0C238BA-9E30-4EE6-80A6-43F731E9A5CD}|Elimine la ACE concede Control total a los administradores de clave de la empresa y agregar una ACE conceder clave Enterprise Admins Control total sobre simplemente el atributo msdsKeyCredentialLink.|Eliminar (A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; Administradores de la clave empresarial) <br /> <br />Agregar (OA; CI; RPWP; 5b47d60f-6090-40b2-9f37-2a4de88f3063; Administradores de la clave empresarial)|

## <a name="windows-server-2016-domain-wide-updates"></a>Windows Server 2016: Actualizaciones para todo el dominio

Después de las operaciones realizadas por **domainprep** en Windows server2016 (operaciones 82-88) haya finalizado, el **revisión** atributo CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = dominioRaízDelBosque objeto se establece en **15**.

|Número de operaciones y el GUID|Descripción|Atributos|Permisos|
|------------------------------|---------------|--------------|---------------|
|**Operación 82**: {83C53DA7-427E-47A4-A07A-A324598B88F7}|Crear CN = contenedor de claves en la raíz del dominio|-objectClass: contenedor<br />: Descripción: Contenedor predeterminado para objetos de credencial de clave<br />- ShowInAdvancedViewOnly: TRUE|(A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; EA)<br />(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;;DA)<br />(A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)<br />(A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; D. D.)<br />(A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; ED)|
|**Operación 83**: {C81FC9CC-0130-4FD1-B272-634D74818133}|Agregue Control total "Permitir ACE" a CN = contenedor de claves para "domain\Key Admins" y "rootdomain\Enterprise administradores clave".|N/D|(A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; Administradores de clave)<br />(A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; Administradores de la clave empresarial)|
|**Operación 84**: {E5F9E791-D96D-4FC9-93C9-D53E1DC439BA}|Modifique el atributo otherWellKnownObjects para que apunte a CN = contenedor de claves.|-otherWellKnownObjects: B:32:683A24E2E8164BD3AF86AC3C2CF3F981:CN=Keys,%ws|N/D|
|**Operación 85**: {e6d5fd00-385d-4e65-b02d-9da3493ed850}|Modificar el dominio de la controladora de red para permitir "domain\Key Admins" y "rootdomain\Enterprise administradores clave" para modificar el atributo msds-KeyCredentialLink. |N/D|(OA; CI; RPWP; 5b47d60f-6090-40b2-9f37-2a4de88f3063; Administradores de clave)<br />(OA; CI; RPWP; 5b47d60f-6090-40b2-9f37-2a4de88f3063; Administradores de empresas clave en el dominio raíz, pero en los dominios que no son raíz dio como resultado una ACE de dominio relativa falsa con un SID-527 no se puede resolver)|
|**Operación 86**: {3a6b3fbf-3168-4312-a10d-dd5b3393952d}|Conceder el AUTOMÓVIL DS-validado-Write-Computer a Creador propietario y self|N/D|(OA;CIIO;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11d0-a285-00aa003049e2;PS)<br />(OA;CIIO;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11d0-a285-00aa003049e2;CO)|
|**Operación 87**: {7F950403-0AB3-47F9-9730-5D7B0269F9BD}|Elimine la ACE concede Control total al grupo de administradores de empresas clave relacionados con el dominio incorrecto y agregar una ACE conceder Control total al grupo de administradores de la clave. |N/D|Eliminar (A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; Administradores de la clave empresarial)<br /> <br />Agregar (A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; Administradores de la clave empresarial)|
|**Operación 88**: {434bb40d-dbc9-4fe7-81d4-d57229f7b080}|Agregue "msDS-ExpirePasswordsOnSmartCardOnlyAccounts" en el objeto de la controladora de red de dominio y establezca el valor predeterminado en FALSE|N/D|N/D|

Los grupos Administradores de la clave de empresas y administradores clave solo se crean después de un controlador de dominio de Windows Server 2016 se promueve y asume el rol FSMO del emulador de PDC.

## <a name="windows-server-2012-r2-domain-wide-updates"></a>Windows Server 2012 R2: Actualizaciones para todo el dominio

Aunque no se realiza por **domainprep** en Windows Server 2012 R2, una vez completado el comando, el **revisión** atributo CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = dominioRaízDelBosque objeto se establece en **10**.

## <a name="windows-server-2012-domain-wide-updates"></a>Windows Server 2012: Actualizaciones para todo el dominio

Después de las operaciones realizadas por **domainprep** en 2012 de servidor de Windows (operaciones de 78, 79, 80 y 81) haya finalizado, el **revisión** atributo CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = dominioRaízDelBosque objeto se establece en **9**.

|Número de operaciones y el GUID|Descripción|Atributos|Permisos|
|------------------------------|---------------|--------------|---------------|
|**Operation 78**: {c3c927a6-cc1d-47c0-966b-be8f9b63d991}|Cree un nuevo objeto CN = dispositivos TPM en la partición de dominio.|Clase de objeto: msTPM InformationObjectsContainer|N/D|
|**Operación 79**: {54afcfb9-637a-4251-9f47-4d50e7021211}|Crea una entrada de control de acceso para el servicio TPM.|N/D|(OA;CIIO;WP;ea1b7b93-5e48-46d5-bc6c-4df4fda78a35;bf967a86-0de6-11d0-a285-00aa003049e2;PS)|
|**Operación 80**: {f4728883-84dd-483c-9897-274f2ebcf11e}|Conceder "Clone DC" derecho extendido para **controladores de dominio clonables** grupo|N/D|(OA; CR; 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e; *SID del dominio*-522)|
|**Operation 81**: {ff4f9d27-7157-4cb0-80a9-5d6f2b14c8ff}|Conceder ms-DS-Allowed-To-Act-On-Behalf-Of-Other-Identity a automático Principal en todos los objetos.|N/D|(OA; CIOI; RPWP; 3f78c3e5-f79a-46bd-a0b8-9d18116ddc79; PS)|

---
ms.assetid: 2a5d5271-6ac6-4c1b-b4ef-9b568932a55a
title: Active Directory actualizaciones de esquema en todo el dominio
description: Actualizaciones de esquema en todo el dominio realizadas por Adprep/DomainPrep al promover un controlador de dominio
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 10/29/2018
ms.topic: article
ms.openlocfilehash: d40a4d82875dd02e3085ba0332cb75db77ce891a
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93068188"
---
# <a name="domain-wide-schema-updates"></a>Actualizaciones de esquema en todo el dominio

>Se aplica a: Windows Server

Puede revisar el siguiente conjunto de cambios para facilitar la comprensión y preparación de las actualizaciones de esquema realizadas por Adprep/DomainPrep en Windows Server.

A partir de Windows Server 2012, los comandos adprep se ejecutan automáticamente según sea necesario durante la instalación de AD DS. También se pueden ejecutar por separado antes de AD DS instalación. Para obtener más información, consulta [Ejecutar Adprep.exe](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10)).

Para obtener más información sobre cómo interpretar las cadenas de entrada de control de acceso (ACE), consulte [cadenas ACE](/windows/win32/secauthz/ace-strings). Para obtener más información sobre cómo interpretar las cadenas de identificador de seguridad (SID), consulte [cadenas de SID](/windows/win32/secauthz/sid-strings).

## <a name="windows-server-semi-annual-channel-domain-wide-updates"></a>Windows Server (canal semianual): actualizaciones en todo el dominio

Después de completar las operaciones realizadas por **DomainPrep** en Windows Server 2016 (operación 89), el atributo **revision** del objeto CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = dominioraízbosque se establece en **16** .

|Número de operaciones y GUID|Descripción|Permisos|
|------------------------------|---------------|--------------|---------------|
|**Operación 89** : {A0C238BA-9E30-4EE6-80A6-43F731E9A5CD}|Elimine la ACE que concede el control total a los administradores de claves empresariales y agregue una ACE que conceda a los administradores de claves empresariales control total sobre solo el atributo msdsKeyCredentialLink.|Eliminar (A; IA RPWPCRLCLOCCDCRCWDWOSDDTSW;;; Administradores de claves de empresa) <br /> <br />Agregar (OA; IA RPWP;5b47d60f-6090-40b2-9f37-2a4de88f3063;; Administradores de claves de empresa)|

## <a name="windows-server-2016-domain-wide-updates"></a>Windows Server 2016: actualizaciones en todo el dominio

Después de que se completen las operaciones realizadas por **DomainPrep** en Windows Server 2016 (operations 82-88), el atributo **revision** del objeto CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = dominioraízbosque se establece en **15** .

|Número de operaciones y GUID|Description|Atributos|Permisos|
|------------------------------|---------------|--------------|---------------|
|**Operación 82** : {83C53DA7-427E-47A4-A07A-A324598B88F7}|Crear el contenedor CN = Keys en la raíz del dominio|-objectClass: contenedor<br />-Description: contenedor predeterminado para los objetos de credencial de clave<br />-Del showinadvancedviewonly: TRUE|Un IA RPWPCRLCLOCCDCRCWDWOSDDTSW;;; EA<br />Un IA RPWPCRLCLOCCDCRCWDWOSDDTSW;;;D Un<br />Un IA RPWPCRLCLOCCDCRCWDWOSDDTSW;;; SY<br />Un IA RPWPCRLCLOCCDCRCWDWOSDDTSW;;;D D<br />Un IA RPWPCRLCLOCCDCRCWDWOSDDTSW;;; Ed|
|**Operación 83** : {C81FC9CC-0130-4FD1-B272-634D74818133}|Agregue control total permitir ACE al contenedor CN = keys para "domain\Key Admins" y "rootdomain\Enterprise Key Admins".|N/D|Un IA RPWPCRLCLOCCDCRCWDWOSDDTSW;;; Administradores de claves)<br />Un IA RPWPCRLCLOCCDCRCWDWOSDDTSW;;; Administradores de claves de empresa)|
|**Operación 84** : {E5F9E791-D96D-4FC9-93C9-D53E1DC439BA}|Modifique el atributo otherWellKnownObjects para que apunte al contenedor CN = Keys.|-otherWellKnownObjects: B:32:683A24E2E8164BD3AF86AC3C2CF3F981: CN = Keys,% WS|N/D|
|**Operación 85** : {e6d5fd00-385d-4e65-b02d-9da3493ed850}|Modifique el NC del dominio para permitir que "domain\Key Admins" y "rootdomain\Enterprise Key Admins" modifiquen el atributo MSDS-KeyCredentialLink. |N/D|OA IA RPWP;5b47d60f-6090-40b2-9f37-2a4de88f3063;; Administradores de claves)<br />OA IA RPWP;5b47d60f-6090-40b2-9f37-2a4de88f3063;; Los administradores de clave empresarial del dominio raíz, pero en los dominios que no son raíz dieron lugar a una ACE relativa a un dominio fantasma con un SID que no se pueda resolver: 527|
|**Operación 86** : {3a6b3fbf-3168-4312-a10d-dd5b3393952d}|Conceder el automóvil DS-Validate-Write-Computer al propietario del creador y al propio|N/D|OA CIIO; SW; 9b026da6-0d3c-465c-8bee-5199d7165cba; bf967a86-0de6-11d0-A285-00aa003049e2; PS)<br />OA CIIO; SW; 9b026da6-0d3c-465c-8bee-5199d7165cba; bf967a86-0de6-11d0-A285-00aa003049e2; CO)|
|**Operación 87** : {7F950403-0AB3-47F9-9730-5D7B0269F9BD}|Elimine la ACE que concede el control total al grupo de administradores de claves empresariales relativos al dominio incorrecto y agregue un ACE que conceda control total al grupo administradores de claves empresariales. |N/D|Eliminar (A; IA RPWPCRLCLOCCDCRCWDWOSDDTSW;;; Administradores de claves de empresa)<br /> <br />Agregar (A; IA RPWPCRLCLOCCDCRCWDWOSDDTSW;;; Administradores de claves de empresa)|
|**Operación 88** : {434bb40d-dbc9-4fe7-81d4-d57229f7b080}|Agregue "msDS-ExpirePasswordsOnSmartCardOnlyAccounts" en el objeto NC del dominio y establezca el valor predeterminado en FALSE.|N/D|N/D|

Los grupos administradores de claves de empresa y administradores de claves solo se crean después de promocionar un controlador de dominio de Windows Server 2016 y asume el rol FSMO del emulador de PDC.

## <a name="windows-server-2012-r2-domain-wide-updates"></a>Windows Server 2012 R2: actualizaciones en todo el dominio

Aunque **DomainPrep** no realiza ninguna operación en Windows Server 2012 R2, una vez completado el comando, el atributo **revision** del objeto CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = dominioraízbosque se establece en **10** .

## <a name="windows-server-2012-domain-wide-updates"></a>Windows Server 2012: actualizaciones en todo el dominio

Después de que se completen las operaciones realizadas por **DomainPrep** en Windows Server 2012 (operaciones 78, 79, 80 y 81), el atributo **revision** del objeto CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = dominioraízbosque se establece en **9** .

|Número de operaciones y GUID|Description|Atributos|Permisos|
|------------------------------|---------------|--------------|---------------|
|**Operación 78** : {c3c927a6-cc1d-47c0-966b-be8f9b63d991}|Cree un nuevo objeto CN = TPM Devices en la partición de dominio.|Clase de objeto: msTPM-InformationObjectsContainer|N/D|
|**Operación 79** : {54afcfb9-637a-4251-9f47-4d50e7021211}|Creó una entrada de control de acceso para el servicio TPM.|N/D|OA CIIO; WP; ea1b7b93-5e48-46d5-bc6c-4df4fda78a35; bf967a86-0de6-11d0-A285-00aa003049e2; PS)|
|**Operación 80** : {f4728883-84dd-483c-9897-274f2ebcf11e}|Conceda el derecho extendido "clonar DC" al grupo **controladores de dominio clonables**|N/D|(OA;; CR; 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e;; *SID de dominio* -522)|
|**Operación 81** : {ff4f9d27-7157-4cb0-80a9-5d6f2b14c8ff}|Conceda MS-DS-allowed-to-Act-on-nombre-of-Other-Identity a la entidad de seguridad propia en todos los objetos.|N/D|OA CIOI; RPWP;3f78c3e5-f79a-46bd-a0b8-9d18116ddc79;; PS|

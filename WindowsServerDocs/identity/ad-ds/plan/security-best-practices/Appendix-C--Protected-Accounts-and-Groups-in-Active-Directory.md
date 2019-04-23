---
ms.assetid: 5b2876ac-fe7d-4054-bfba-b692e57bc0d2
title: Apéndice C - protegidas las cuentas y grupos en Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 70e29ad42b57cf315c7179d6eea8220ad2bfab48
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834706"
---
# <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>Apéndice C: Cuentas protegidas y grupos en Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

## <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>Apéndice C: Cuentas protegidas y grupos en Active Directory

Dentro de Active Directory, un conjunto predeterminado de grupos y cuentas con privilegios elevados se consideran cuentas protegidas y grupos. Con la mayoría de los objetos en Active Directory, pueden cambiar los permisos en los objetos, incluido el cambio de permisos para permitir que ellos mismos cambiar la pertenencia a los administradores delegados (los usuarios que han sido delegados permisos para administrar objetos de Active Directory) los grupos, por ejemplo.  

Sin embargo, con grupos y cuentas protegidas, los permisos de los objetos establece y aplica a través de un proceso automático que garantiza que los permisos en lo objetos se mantenga la consistencia incluso si los objetos son mueven el directorio. Aun cuando alguien cambie manualmente los permisos de un objeto protegido, este proceso garantiza que los permisos se devuelven rápidamente a sus valores predeterminados.  

### <a name="protected-groups"></a>Grupos protegidos

En la tabla siguiente contiene los grupos protegidos en Active Directory por el sistema operativo de controlador de dominio.  

#### <a name="protected-accounts-and-groups-in-active-directory-by-operating-system"></a>Cuentas protegidas y grupos en Active Directory por sistema operativo

| Windows Server 2003 RTM | Windows Server 2003 SP1 y versiones posteriores | Windows Server 2012, <br> Windows Server 2008 R2, <br> Windows Server 2008 | Windows Server 2016 |
| --- | --- | --- | --- |
|Operadores de cuentas|Operadores de cuentas|Operadores de cuentas|Operadores de cuentas|
|Administrador|Administrador|Administrador|Administrador|
|Administradores|Administradores|Administradores|Administradores|
|Operadores de copia de seguridad|Operadores de copia de seguridad|Operadores de copia de seguridad|Operadores de copia de seguridad|
|Publicadores de certificados|||
|Admins. del dominio|Admins. del dominio|Admins. del dominio|Admins. del dominio|
|Controladores de dominio|Controladores de dominio|Controladores de dominio|Controladores de dominio|
|Administradores de empresas|Administradores de empresas|Administradores de empresas|Administradores de empresas|
||||Administradores de organización clave|
||||Administradores de claves|
|Krbtgt|Krbtgt|Krbtgt|Krbtgt|
|Opers. de impresión|Opers. de impresión|Opers. de impresión|Opers. de impresión|
|||Controladores de dominio de solo lectura|Controladores de dominio de solo lectura|
|Replicador|Replicador|Replicador|Replicador|
|Administradores de esquema|Administradores de esquema|Administradores de esquema|Administradores de esquema|
|Operadores de servidores|Operadores de servidores|Operadores de servidores|Operadores de servidores|

#### <a name="adminsdholder"></a>AdminSDHolder

El propósito del objeto AdminSDHolder es proporcionar permisos de "plantilla" para las cuentas protegidas y grupos del dominio. AdminSDHolder se crea automáticamente como un objeto en el contenedor del sistema de cada dominio de Active Directory. Su ruta de acceso es: **CN = AdminSDHolder, CN = System, DC = < componenteDeDominio >, DC = < componenteDeDominio >?.**  

A diferencia de la mayoría de los objetos en el dominio de Active Directory, que es propiedad del grupo Administradores, AdminSDHolder es propiedad del grupo Admins. del dominio. De forma predeterminada, EAs pueden realizar cambios al objeto del cualquier dominio AdminSDHolder, igual que los grupos Admins. del dominio y administradores del dominio. Además, aunque el propietario predeterminado de AdminSDHolder es el grupo de administradores de dominio del dominio, los miembros de los administradores o administradores de empresa pueden tomar posesión del objeto.  

#### <a name="sdprop"></a>SDProp

SDProp es un proceso que se ejecuta cada 60 minutos (de forma predeterminada) en el controlador de dominio que contiene el emulador de PDC (PDCE del dominio). SDProp compara los permisos en el objeto AdminSDHolder del dominio con los permisos en las cuentas protegidas y grupos del dominio. Si los permisos en cualquiera de los grupos y cuentas protegidas no coinciden con los permisos en el objeto AdminSDHolder, se restablecen los permisos en los grupos y cuentas protegidas para que coincidan con los del objeto AdminSDHolder del dominio.  

Además, la herencia de permisos está deshabilitada en grupos protegidos y cuentas, lo que significa que incluso si las cuentas y grupos se mueven a ubicaciones diferentes en el directorio, no heredan permisos de sus objetos primarios nuevo. Se deshabilita la herencia en el objeto AdminSDHolder para que los cambios de permisos a los objetos primarios no cambian los permisos de AdminSDHolder.  

##### <a name="changing-sdprop-interval"></a>Cambiar el intervalo de SDProp

Normalmente, no deberían necesitar cambiar el intervalo en el que se ejecuta el elemento SDProp, excepto con fines de prueba. Si necesita cambiar el intervalo de SDProp en el PDCE para el dominio, utilice regedit para agregar o modificar el valor de DWORD AdminSDProtectFrequency en HKLM\SYSTEM\CurrentControlSet\Services\NTDS\Parameters.  

El intervalo de valores es en segundos entre 60 a 7200 (un minuto a dos horas). Para revertir los cambios, elimine la clave de AdminSDProtectFrequency, lo que hará que SDProp revertir al intervalo de 60 minutos. Por lo general no se debe reducir este intervalo en dominios de producción tal como puede aumentar la sobrecarga de procesamiento en el controlador de dominio LSASS. El impacto de este aumento es depende del número de objetos protegidos en el dominio.  

##### <a name="running-sdprop-manually"></a>Ejecución manual de SDProp

Un mejor enfoque para probar los cambios de AdminSDHolder es ejecutar SDProp manualmente, lo que hace que la tarea se ejecute inmediatamente, pero no afecta a la ejecución programada. Ejecutar SDProp manualmente es realizar una forma ligeramente diferente en los controladores de dominio que ejecuta Windows Server 2008 y versiones anteriores de lo que resulta en controladores de dominio que ejecutan Windows Server 2012 o Windows Server 2008 R2.  

Se proporcionan procedimientos para ejecutar SDProp manualmente en sistemas operativos anteriores en [artículo de Microsoft Support 251343](https://support.microsoft.com/kb/251343), y los siguientes son instrucciones paso a paso para los sistemas operativos anteriores y recientes. En cualquier caso, debe conectar con el objeto rootDSE en Active Directory y realizar una operación de modificación con DN es null para el objeto rootDSE, especificando el nombre de la operación como el atributo para modificar. Para obtener más información acerca de las operaciones modificables del objeto rootDSE, consulte [rootDSE modificar operaciones](https://msdn.microsoft.com/library/cc223297.aspx) en el sitio Web MSDN.  

###### <a name="running-sdprop-manually-in-windows-server-2008-or-earlier"></a>Ejecución manual SDProp en Windows Server 2008 o versiones anteriores

Puede forzar SDProp para ejecutar mediante el uso de Ldp.exe o mediante la ejecución de una secuencia de comandos de modificación de LDAP. Para ejecutar SDProp mediante Ldp.exe, realice los pasos siguientes después de realizar cambios en el objeto AdminSDHolder en un dominio:  

1. Iniciar **Ldp.exe**.  
2. Haga clic en **conexión** en el cuadro de diálogo Ldp y haga clic en **Connect**.  

   ![cuentas protegidas y grupos](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_9.gif)  

3. En el **Connect** diálogo cuadro, escriba el nombre del controlador de dominio para el dominio que contiene el rol de emulador de PDC (PDCE) y haga clic en **Aceptar**.  

   ![cuentas protegidas y grupos](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_10.png)  

4. Compruebe que se ha conectado correctamente, tal y como indica **Dn: (RootDSE)**  en la siguiente captura de pantalla, haga clic en **conexión** y haga clic en **enlazar**.  

   ![cuentas protegidas y grupos](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_11.png)  

5. En el **enlazar** diálogo cuadro, escriba las credenciales de una cuenta de usuario que tenga permiso para modificar el objeto rootDSE. (Si ha iniciado sesión como ese usuario, puede seleccionar **enlazar como** usuario actualmente conectado.) Haga clic en **Aceptar**.  

   ![cuentas protegidas y grupos](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_12.png)  

6. Después de haber completado la operación de enlace, haga clic en **examinar**y haga clic en **modificar**.  

   ![cuentas protegidas y grupos](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_13.png)  

7. En el **modificar** cuadro de diálogo, deje el **DN** campo en blanco. En el **Editar atributo de entrada** , escriba **FixUpInheritance**y en el **valores** , escriba **Sí**. Haga clic en **ENTRAR** para rellenar el **lista de entradas** tal como se muestra en la captura de pantalla siguiente.  

   ![cuentas protegidas y grupos](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_14.gif)  

8. En el cuadro de diálogo Modificar rellenado, haga clic en ejecutar y compruebe que los cambios realizados en el objeto AdminSDHolder han aparecido en ese objeto.  

> [!NOTE]  
> Para obtener información acerca de cómo modificar AdminSDHolder para permitir que las cuentas sin privilegios designadas modificar la pertenencia a grupos protegidos, consulte [apéndice I: Creación de cuentas de administración para proteger cuentas y grupos en Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  

Si prefiere ejecutar SDProp manualmente a través de LDIFDE o una secuencia de comandos, puede crear una entrada de modificar, como se muestra aquí:  

![cuentas protegidas y grupos](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_15.gif)  

###### <a name="running-sdprop-manually-in-windows-server-2012-or-windows-server-2008-r2"></a>Ejecutar SDProp manualmente en Windows Server 2012 o Windows Server 2008 R2

También puede forzar SDProp para ejecutar mediante el uso de Ldp.exe o mediante la ejecución de una secuencia de comandos de modificación de LDAP. Para ejecutar SDProp mediante Ldp.exe, realice los pasos siguientes después de realizar cambios en el objeto AdminSDHolder en un dominio:  

1. Iniciar **Ldp.exe**.  

2. En el **Ldp** cuadro de diálogo, haga clic en **conexión**y haga clic en **Connect**.  

   ![cuentas protegidas y grupos](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_16.gif)  

3. En el **Connect** diálogo cuadro, escriba el nombre del controlador de dominio para el dominio que contiene el rol de emulador de PDC (PDCE) y haga clic en **Aceptar**.  

   ![cuentas protegidas y grupos](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_17.gif)  

4. Compruebe que se ha conectado correctamente, tal y como indica **Dn: (RootDSE)**  en la siguiente captura de pantalla, haga clic en **conexión** y haga clic en **enlazar**.  

   ![cuentas protegidas y grupos](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_18.gif)  

5. En el **enlazar** diálogo cuadro, escriba las credenciales de una cuenta de usuario que tenga permiso para modificar el objeto rootDSE. (Si ha iniciado sesión como ese usuario, puede seleccionar **enlace actualmente como usuario que inició sesión**.) Haga clic en **Aceptar**.  

   ![cuentas protegidas y grupos](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_19.gif)  

6. Después de haber completado la operación de enlace, haga clic en **examinar**y haga clic en **modificar**.  

   ![cuentas protegidas y grupos](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_20.gif)  

7. En el **modificar** cuadro de diálogo, deje el **DN** campo en blanco. En el **Editar atributo de entrada** , escriba **RunProtectAdminGroupsTask**y en el **valores** , escriba **1**. Haga clic en **ENTRAR** para rellenar la lista de entrada, como se muestra aquí.  

   ![cuentas protegidas y grupos](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_21.gif)  

8. En el rellenado **modificar** cuadro de diálogo, haga clic en **ejecutar**y compruebe que los cambios realizados en el objeto AdminSDHolder han aparecido en ese objeto.  

Si prefiere ejecutar SDProp manualmente a través de LDIFDE o una secuencia de comandos, puede crear una entrada de modificar, como se muestra aquí:  

![cuentas protegidas y grupos](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_22.gif)  

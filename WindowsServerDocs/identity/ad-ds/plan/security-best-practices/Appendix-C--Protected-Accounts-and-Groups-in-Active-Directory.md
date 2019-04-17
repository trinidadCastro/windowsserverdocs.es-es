---
ms.assetid: 5b2876ac-fe7d-4054-bfba-b692e57bc0d2
title: "Apéndice C: - protegido cuentas y grupos de Active Directory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c1d29b61844428115f8b2d1f5d935f225a249659
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>Apéndice C: cuentas protegidas y grupos de Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>Apéndice C: cuentas protegidas y grupos de Active Directory  
Dentro de Active Directory, se consideran un conjunto predeterminado de grupos y cuentas con privilegios elevados cuentas protegidas y grupos. Con la mayoría de los objetos de Active Directory, los administradores delegados (usuarios que han sido delegados los permisos para administrar objetos de Active Directory) pueden cambiar los permisos en los objetos, incluido cómo cambiar los permisos para permitir que por sí solas para cambiar la pertenencia a los grupos, por ejemplo.  

Sin embargo, con los grupos y cuentas protegidas, los permisos de los objetos se establece y exige a través de un proceso automático que garantiza que los permisos en la permanece objetos coherente incluso si los objetos son movido al directorio. Incluso si alguien manualmente cambia los permisos de un objeto protegido, este proceso garantiza que los permisos se devuelven los valores predeterminados rápidamente.  

### <a name="protected-groups"></a>Grupos protegidos  
La tabla siguiente contiene los grupos protegidos en enumerados por el sistema operativo de controlador de dominio de Active Directory.  

**Cuentas protegidas y los grupos de Active Directory por sistema operativo**  

|||||  
|-|-|-|-|  
|**Windows 2000 < SP4**|**Windows 2000 SP4 - Windows Server 2003 RTM**|**Windows Server 2003 SP1 +**|**Windows Server 2012, Windows Server 2008 R2, Windows Server 2008**|  
|Administradores|Operadores de cuentas|Operadores de cuentas|Operadores de cuentas|  
||Administrador|Administrador|Administrador|  
||Administradores|Administradores|Administradores|  
||Operadores de copia de seguridad|Operadores de copia de seguridad|Operadores de copia de seguridad|  
||Publicadores de certificados|||  
|Administradores de dominio|Administradores de dominio|Administradores de dominio|Administradores de dominio|  
||Controladores de dominio|Controladores de dominio|Controladores de dominio|  
|Administradores de empresa|Administradores de empresa|Administradores de empresa|Administradores de empresa|  
||Krbtgt|Krbtgt|Krbtgt|  
||Operadores de impresión|Operadores de impresión|Operadores de impresión|  
||||Controladores de dominio de solo lectura|  
||Replicator|Replicator|Replicator|  
|Administradores de esquema|Administradores de esquema|Administradores de esquema|Administradores de esquema|  
||Operadores de servidor|Operadores de servidor|Operadores de servidor|  

#### <a name="adminsdholder"></a>AdminSDHolder  
El propósito del objeto AdminSDHolder es proporcionar permisos de "plantillas" para las cuentas protegidas y los grupos en el dominio. AdminSDHolder automáticamente se crea como un objeto en el contenedor del sistema de cada dominio de Active Directory. ¿Su ruta de acceso es: **CN = AdminSDHolder, CN = sistema, DC = < componenteDeDominio >, DC = < componenteDeDominio >?**  

A diferencia de la mayoría de los objetos en el dominio de Active Directory que pertenece al grupo de administradores, AdminSDHolder es propiedad del grupo Administradores de dominio. De manera predeterminada, EAs realizar cambios al objeto de AdminSDHolder de cualquier dominio, como grupos de administradores de dominio y administradores del dominio. Además, aunque el propietario predeterminado de AdminSDHolder es el grupo de administradores de dominio del dominio, los miembros de los administradores o administradores de empresa pueden tomar posesión del objeto.  

#### <a name="sdprop"></a>SDProp  
SDProp es un proceso que se ejecuta cada 60 minutos (de forma predeterminada) en el controlador de dominio que contiene el emulador de PDC (PDCE del dominio). SDProp compara los permisos de objeto de AdminSDHolder del dominio con los permisos en las cuentas protegidas y los grupos en el dominio. Si los permisos en ninguno de los grupos y cuentas protegidas no coinciden con los permisos en el objeto AdminSDHolder, se restablecen los permisos de las cuentas protegidas y los grupos para que coincida con las del objeto de AdminSDHolder del dominio.  

Además, la herencia de permisos está deshabilitada en grupos protegidos y cuentas, lo que significa que incluso si las cuentas y grupos se mueven a diferentes ubicaciones en el directorio, no heredan permisos de sus objetos primarios nuevo. Herencia está deshabilitada en el objeto AdminSDHolder para que los cambios en los permisos a los objetos primarios no cambian los permisos de AdminSDHolder.  

##### <a name="changing-sdprop-interval"></a>Cambiar el intervalo de SDProp  
Normalmente, no es necesario cambiar el intervalo en el que se ejecuta SDProp, excepto para fines de prueba. Si es necesario cambiar el intervalo SDProp, en la PDCE para el dominio, usar regedit para agregar o modificar el valor de DWORD AdminSDProtectFrequency en HKLM\SYSTEM\CurrentControlSet\Services\NTDS\Parameters.  

El intervalo de valores es en segundos de 60 a 7200 (un minuto a dos horas). Para revertir los cambios, elimina la clave de AdminSDProtectFrequency, lo que hará que SDProp volver al intervalo de 60 minutos. Por lo general, no se debe reducir este intervalo en dominios de producción como puede aumentar LSASS carga de procesamiento en el controlador de dominio. El impacto de este aumento depende de la cantidad de objetos protegidos en el dominio.  

##### <a name="running-sdprop-manually"></a>Ejecuta SDProp manualmente  
Un mejor enfoque para probar los cambios de AdminSDHolder es ejecutar SDProp manualmente, que hace que la tarea Ejecutar inmediatamente, pero no afecta a la ejecución programada. Ejecuta SDProp manualmente es realizadas distinta en controladores de dominio ejecutan Windows Server 2008 y versiones anteriores de lo que resulta en controladores de dominio que ejecutan Windows Server 2012 o Windows Server 2008 R2.  

Se proporcionan procedimientos para ejecutar SDProp manualmente en sistemas operativos anteriores en [artículo de Microsoft Support 251343](https://support.microsoft.com/kb/251343), y siguiente es instrucciones paso a paso para sistemas operativos más antiguos y más reciente. En cualquier caso, debe conectarse al objeto rootDSE en Active Directory y realizar una operación de modificar DN es null para el objeto rootDSE, especificando el nombre de la operación como el atributo en modificar. Para obtener más información acerca de las operaciones pueden modificables en el objeto rootDSE, consulta [rootDSE modificar operaciones](https://msdn.microsoft.com/library/cc223297.aspx) en el sitio Web MSDN.  

###### <a name="running-sdprop-manually-in-windows-server-2008-or-earlier"></a>Ejecuta SDProp manualmente en Windows Server 2008 o versiones anteriores  
Puedes forzar SDProp ejecutar utilizando Ldp.exe o si se ejecuta un script de modificación de LDAP. Para ejecutar SDProp mediante Ldp.exe, realiza los siguientes pasos después de que has realizado cambios en el objeto AdminSDHolder en un dominio:  

1.  Iniciar **Ldp.exe**.  

2.  Haz clic en **conexión** en el cuadro de diálogo Ldp y haz clic en **conectar**.  

    ![grupos y cuentas protegidas](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_9.gif)  

3.  En la **conectar** cuadro de diálogo, escriba el nombre del controlador de dominio del dominio que contiene el rol de emulador PDC (PDCE) y haz clic en **Aceptar**.  

    ![grupos y cuentas protegidas](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_10.png)  

4.  Comprueba que se ha conectado correctamente, como se indica en **Dn: (RootDSE)** en la siguiente captura de pantalla, haz clic en **conexión** y haz clic en **enlazar**.  

    ![grupos y cuentas protegidas](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_11.png)  

5.  En la **enlazar** cuadro de diálogo, escriba las credenciales de cuenta de usuario que tiene permiso para modificar el objeto rootDSE. (Si has iniciado sesión como ese usuario, puedes seleccionar **enlazar como** usuario actualmente conectado.) Haz clic en **Aceptar**.  

    ![grupos y cuentas protegidas](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_12.png)  

6.  Después de completar la operación de enlace, haz clic en **examinar**y haz clic en **modificar**.  

    ![grupos y cuentas protegidas](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_13.png)  

7.  En la **modificar** cuadro de diálogo, deja la **DN** campo en blanco. En la **editar el atributo de entrada** , escriba **FixUpInheritance**y en la **valores** , escriba **Sí**. Haz clic en **ENTRAR** para rellenar el **lista de entradas** como se muestra en la siguiente captura de pantalla.  

    ![grupos y cuentas protegidas](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_14.gif)  

8.  En el cuadro de diálogo Modificar rellenado, haz clic en ejecutar y comprueba que los cambios realizados en el objeto AdminSDHolder no han aparecido en ese objeto.  

> [!NOTE]  
> Para obtener información sobre cómo modificar AdminSDHolder para permitir que las cuentas sin privilegios designadas modificar la pertenencia a grupos protegidos, vea [apéndice I: creación de administración de cuentas para las cuentas protegidas y los grupos de Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  

Si prefieres ejecutar SDProp manualmente a través de un script o LDIFDE, puedes crear una entrada de modificar como se muestra aquí:  

![grupos y cuentas protegidas](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_15.gif)  

###### <a name="running-sdprop-manually-in-windows-server-2012-or-windows-server-2008-r2"></a>Ejecuta SDProp manualmente en Windows Server 2012 o Windows Server 2008 R2  
También puede forzar SDProp ejecutar utilizando Ldp.exe o si se ejecuta un script de modificación de LDAP. Para ejecutar SDProp mediante Ldp.exe, realiza los siguientes pasos después de que has realizado cambios en el objeto AdminSDHolder en un dominio:  

1.  Iniciar **Ldp.exe**.  

2.  En la **Ldp** cuadro de diálogo, haz clic en **conexión**y haz clic en **conectar**.  

    ![grupos y cuentas protegidas](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_16.gif)  

3.  En la **conectar** cuadro de diálogo, escriba el nombre del controlador de dominio del dominio que contiene el rol de emulador PDC (PDCE) y haz clic en **Aceptar**.  

    ![grupos y cuentas protegidas](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_17.gif)  

4.  Comprueba que se ha conectado correctamente, como se indica en **Dn: (RootDSE)** en la siguiente captura de pantalla, haz clic en **conexión** y haz clic en **enlazar**.  

    ![grupos y cuentas protegidas](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_18.gif)  

5.  En la **enlazar** cuadro de diálogo, escriba las credenciales de cuenta de usuario que tiene permiso para modificar el objeto rootDSE. (Si has iniciado sesión como ese usuario, puedes seleccionar **Bind como usuario que inició sesión**.) Haz clic en **Aceptar**.  

    ![grupos y cuentas protegidas](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_19.gif)  

6.  Después de completar la operación de enlace, haz clic en **examinar**y haz clic en **modificar**.  

    ![grupos y cuentas protegidas](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_20.gif)  

7.  En la **modificar** cuadro de diálogo, deja la **DN** campo en blanco. En la **editar el atributo de entrada** , escriba **RunProtectAdminGroupsTask**y en la **valores** , escriba **1**. Haz clic en **ENTRAR** para rellenar la lista de entrada, como se muestra aquí.  

    ![grupos y cuentas protegidas](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_21.gif)  

8.  En el que se rellena **modificar** cuadro de diálogo, haz clic en **ejecutar**y comprueba que los cambios realizados en el objeto AdminSDHolder no han aparecido en ese objeto.  

Si prefieres ejecutar SDProp manualmente a través de un script o LDIFDE, puedes crear una entrada de modificar como se muestra aquí:  

![grupos y cuentas protegidas](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_22.gif)  

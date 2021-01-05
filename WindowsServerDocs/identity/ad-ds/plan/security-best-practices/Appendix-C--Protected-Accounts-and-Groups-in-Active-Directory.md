---
description: 'Más información acerca de: Apéndice C: cuentas y grupos protegidos en Active Directory'
ms.assetid: 5b2876ac-fe7d-4054-bfba-b692e57bc0d2
title: 'Apéndice C: cuentas y grupos protegidos en Active Directory'
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: a968ddd70aa74d571e939c3d2c21cbc24e572f5e
ms.sourcegitcommit: d2224cf55c5d4a653c18908da4becf94fb01819e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2020
ms.locfileid: "97711770"
---
# <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>Anexo C: cuentas protegidas y grupos de Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

## <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>Anexo C: cuentas protegidas y grupos de Active Directory

Dentro de Active Directory, un conjunto predeterminado de cuentas y grupos con privilegios elevados se considera cuentas y grupos protegidos. Con la mayoría de los objetos de Active Directory, los administradores delegados (usuarios a los que se han delegado permisos para administrar objetos de Active Directory) pueden cambiar los permisos de los objetos, incluido el cambio de permisos para permitirse cambiar la pertenencia de los grupos, por ejemplo.

Sin embargo, con cuentas y grupos protegidos, los permisos de los objetos se establecen y se aplican a través de un proceso automático que garantiza que los permisos en los objetos siguen siendo coherentes incluso si los objetos se mueven al directorio. Incluso si alguien cambia manualmente los permisos de un objeto protegido, este proceso garantiza que los permisos se devuelven a sus valores predeterminados rápidamente.

### <a name="protected-groups"></a>Grupos protegidos

La tabla siguiente contiene los grupos protegidos en Active Directory enumerados por el sistema operativo del controlador de dominio.

#### <a name="protected-accounts-and-groups-in-active-directory-by-operating-system"></a>Cuentas y grupos protegidos en Active Directory por sistema operativo

| Windows Server 2003 RTM | Windows Server 2003 SP1 + | Windows Server 2012, <br> Windows Server 2008 R2, <br> Windows Server 2008 | Windows Server 2016 |
| --- | --- | --- | --- |
|Operadores de cuentas|Operadores de cuentas|Operadores de cuentas|Operadores de cuentas|
|Administrador|Administrador|Administrador|Administrador|
|Administradores|Administradores|Administradores|Administradores|
|Operadores de copias de seguridad|Operadores de copia de seguridad|Operadores de copia de seguridad|Operadores de copia de seguridad|
|Publicadores de certificados|||
|Administradores de dominio|Administradores de dominio|Administradores de dominio|Admins. del dominio|
|Controladores de dominio|Controladores de dominio|Controladores de dominio|Controladores de dominio|
|Administradores de empresas|Administradores de empresas|Administradores de empresas|Administradores de empresas|
|Krbtgt|Krbtgt|Krbtgt|Krbtgt|
|Operadores de impresión|Operadores de impresión|Operadores de impresión|Operadores de impresión|
|||Controladores de dominio de solo lectura|Controladores de dominio de solo lectura|
|Duplicadores|Duplicadores|Duplicadores|Duplicadores|
|Administradores de esquema|Administradores de esquema|Administradores de esquema|Administradores de esquema|
|Operadores de servidores|Operadores de servidores|Operadores de servidores|Operadores de servidores|

#### <a name="adminsdholder"></a>AdminSDHolder

El propósito del objeto AdminSDHolder es proporcionar permisos de "plantilla" para las cuentas protegidas y los grupos del dominio. AdminSDHolder se crea automáticamente como un objeto en el contenedor del sistema de cada dominio de Active Directory. Su ruta de acceso es: **CN = AdminSDHolder, CN = System, DC =<domain_component>, DC =<domain_component>?.**

A diferencia de la mayoría de los objetos del dominio Active Directory, que son propiedad del grupo administradores, AdminSDHolder es propiedad del grupo Admins. del dominio. De forma predeterminada, EAs puede realizar cambios en el objeto AdminSDHolder de cualquier dominio, como los grupos Admins. del dominio y administradores del dominio. Además, aunque el propietario predeterminado de AdminSDHolder es el grupo Admins. del dominio del dominio, los miembros de administradores o administradores de empresa pueden tomar posesión del objeto.

#### <a name="sdprop"></a>SDProp

SDProp es un proceso que se ejecuta cada 60 minutos (de forma predeterminada) en el controlador de dominio que contiene el emulador de PDC del dominio (PDCE). SDProp compara los permisos del objeto AdminSDHolder del dominio con los permisos en las cuentas protegidas y los grupos del dominio. Si los permisos de cualquiera de las cuentas y grupos protegidos no coinciden con los permisos en el objeto AdminSDHolder, se restablecen los permisos de los grupos y las cuentas protegidas para que coincidan con los del objeto AdminSDHolder del dominio.

Además, la herencia de permisos está deshabilitada en grupos y cuentas protegidos, lo que significa que incluso si las cuentas y los grupos se mueven a ubicaciones diferentes del directorio, no heredan los permisos de los nuevos objetos primarios. La herencia se deshabilita en el objeto AdminSDHolder para que los cambios de permisos en los objetos primarios no cambien los permisos de AdminSDHolder.

##### <a name="changing-sdprop-interval"></a>Cambiar el intervalo de SDProp

Normalmente, no es necesario cambiar el intervalo en el que se ejecuta SDProp, excepto con fines de prueba. Si tiene que cambiar el intervalo de SDProp, en el PDCE del dominio, use regedit para agregar o modificar el valor DWORD AdminSDProtectFrequency en HKLM\SYSTEM\CurrentControlSet\Services\NTDS\Parameters.

El intervalo de valores está en segundos de 60 a 7200 (de un minuto a dos horas). Para invertir los cambios, elimine la clave AdminSDProtectFrequency, lo que hará que SDProp vuelva al intervalo de 60 minutos. Por lo general, no debe reducir este intervalo en los dominios de producción, ya que puede aumentar la sobrecarga de procesamiento de LSASS en el controlador de dominio. El impacto de este aumento depende del número de objetos protegidos en el dominio.

##### <a name="running-sdprop-manually"></a>Ejecutar SDProp manualmente

Un enfoque mejor para probar los cambios de AdminSDHolder es ejecutar SDProp manualmente, lo que hace que la tarea se ejecute inmediatamente, pero no afecta a la ejecución programada. La ejecución manual de SDProp se realiza de forma ligeramente diferente en los controladores de dominio que ejecutan Windows Server 2008 y versiones anteriores a los controladores de dominio que ejecutan Windows Server 2012 o Windows Server 2008 R2.

Los procedimientos para ejecutar SDProp manualmente en sistemas operativos anteriores se proporcionan en [soporte técnico de Microsoft artículo 251343](https://support.microsoft.com/kb/251343)y a continuación se indican instrucciones paso a paso para los sistemas operativos antiguos y más recientes. En cualquier caso, debe conectarse al objeto rootDSE en Active Directory y realizar una operación de modificación con un DN nulo para el objeto rootDSE, especificando el nombre de la operación como el atributo que se va a modificar. Para obtener más información acerca de las operaciones modificables en el objeto rootDSE, vea las [operaciones de modificación de RootDSE](/openspecs/windows_protocols/ms-adts/fc74972f-b267-4c1a-8716-0f5b48cf52b9) en el sitio web de MSDN.

###### <a name="running-sdprop-manually-in-windows-server-2008-or-earlier"></a>Ejecutar SDProp manualmente en Windows Server 2008 o versiones anteriores

Puede forzar que SDProp se ejecute mediante Ldp.exe o mediante la ejecución de un script de modificación de LDAP. Para ejecutar SDProp con Ldp.exe, realice los pasos siguientes después de haber realizado cambios en el objeto AdminSDHolder en un dominio:

1. Inicie **Ldp.exe**.
2. Haga clic en **conexión** en el cuadro de diálogo LDP y haga clic en **conectar**.

   ![Captura de pantalla que muestra la opción de menú conectar.](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_9.gif)

3. En el cuadro de diálogo **conectar** , escriba el nombre del controlador de dominio para el dominio que contiene el rol de emulador de PDC (PDCE) y haga clic en **Aceptar**.

   ![Captura de pantalla que muestra dónde escribir el nombre del controlador de dominio para el dominio que contiene el rol de emulador de PDC (PDCE).](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_10.png)

4. Compruebe que se ha conectado correctamente, como se indica en **DN: (RootDSE)** en la siguiente captura de pantalla, haga clic en **conexión** y haga clic en **enlazar**.

   ![Captura de pantalla que muestra dónde seleccionar la conexión y, a continuación, seleccione enlazar para comprobar que se ha conectado correctamente.](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_11.png)

5. En el cuadro de diálogo **enlazar** , escriba las credenciales de una cuenta de usuario que tenga permiso para modificar el objeto RootDSE. (Si inició sesión como ese usuario, puede seleccionar **enlazar como** usuario que ha iniciado sesión actualmente). Haga clic en **Aceptar**.

   ![Captura de pantalla que muestra el cuadro de diálogo enlazar.](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_12.png)

6. Una vez completada la operación de enlace, haga clic en **examinar** y, a continuación, en **modificar**.

   ![Captura de pantalla que muestra la opción de menú modificar en el menú examinar.](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_13.png)

7. En el cuadro de diálogo **modificar** , deje el campo **DN** en blanco. En el campo **Editar atributo de entrada** , escriba **FixUpInheritance** y, en el campo **valores** , escriba **sí**. Haga clic en **entrar** para rellenar la **lista de entradas** tal como se muestra en la siguiente captura de pantalla.

   ![Captura de pantalla que muestra el cuadro de diálogo modificar.](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_14.gif)

8. En el cuadro de diálogo modificar, haga clic en ejecutar y compruebe que los cambios realizados en el objeto AdminSDHolder han aparecido en ese objeto.

> [!NOTE]
> Para obtener información sobre cómo modificar AdminSDHolder para permitir que las cuentas sin privilegios designadas modifiquen la pertenencia de los grupos protegidos, vea el [Apéndice I: crear cuentas de administración para cuentas y grupos protegidos en Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).

Si prefiere ejecutar SDProp manualmente mediante LDIFDE o un script, puede crear una entrada de modificación como se muestra aquí:

![Captura de pantalla que muestra cómo se puede crear una entrada de modificación.](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_15.gif)

###### <a name="running-sdprop-manually-in-windows-server-2012-or-windows-server-2008-r2"></a>Ejecutar SDProp manualmente en Windows Server 2012 o Windows Server 2008 R2

También puede forzar que SDProp se ejecute mediante Ldp.exe o mediante la ejecución de un script de modificación de LDAP. Para ejecutar SDProp con Ldp.exe, realice los pasos siguientes después de haber realizado cambios en el objeto AdminSDHolder en un dominio:

1. Inicie **Ldp.exe**.

2. En el cuadro de diálogo **LDP** , haga clic en **conexión** y, a continuación, haga clic en **conectar**.

   ![Captura de pantalla que muestra el cuadro de diálogo LDP.](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_16.gif)

3. En el cuadro de diálogo **conectar** , escriba el nombre del controlador de dominio para el dominio que contiene el rol de emulador de PDC (PDCE) y haga clic en **Aceptar**.

   ![Captura de pantalla que muestra el cuadro de diálogo conectar.](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_17.gif)

4. Compruebe que se ha conectado correctamente, como se indica en **DN: (RootDSE)** en la siguiente captura de pantalla, haga clic en **conexión** y haga clic en **enlazar**.

   ![Captura de pantalla que muestra la opción de menú enlazar en el menú conexión.](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_18.gif)

5. En el cuadro de diálogo **enlazar** , escriba las credenciales de una cuenta de usuario que tenga permiso para modificar el objeto RootDSE. (Si inició sesión como ese usuario, puede seleccionar **enlazar como usuario que ha iniciado sesión actualmente**). Haga clic en **Aceptar**.

   ![Captura de pantalla que muestra dónde escribir las credenciales de una cuenta de usuario que tiene permiso para modificar el objeto rootDSE.](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_19.gif)

6. Una vez completada la operación de enlace, haga clic en **examinar** y, a continuación, en **modificar**.

   ![cuentas y grupos protegidos](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_20.gif)

7. En el cuadro de diálogo **modificar** , deje el campo **DN** en blanco. En el campo **Editar atributo de entrada** , escriba **RunProtectAdminGroupsTask** y, en el campo **valores** , escriba **1**. Haga clic en **entrar** para rellenar la lista de entradas como se muestra aquí.

   ![Captura de pantalla que muestra el campo editar atributo de entrada.](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_21.gif)

8. En el cuadro de diálogo **modificar** , haga clic en **Ejecutar** y compruebe que los cambios realizados en el objeto AdminSDHolder han aparecido en ese objeto.

Si prefiere ejecutar SDProp manualmente mediante LDIFDE o un script, puede crear una entrada de modificación como se muestra aquí:

![Captura de pantalla que muestra qué hacer si prefiere ejecutar SDProp manualmente mediante LDIFDE o un script.](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_22.gif)

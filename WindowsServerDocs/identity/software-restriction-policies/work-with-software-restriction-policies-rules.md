---
title: Trabajo con reglas de directivas de restricción de software
description: Obtenga información acerca de los procedimientos que trabajan con las reglas de certificado, ruta de acceso, zona de Internet y hash mediante directivas de restricción de software.
ms.topic: article
ms.assetid: 4a8047d5-9bb9-4bed-bc8f-583a237731e2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: d36483dec8d5802df042d3444e477bcb508a9819
ms.sourcegitcommit: d42b80f947dbfa8660d982be67d77745a28081e5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2021
ms.locfileid: "98113451"
---
# <a name="work-with-software-restriction-policies-rules"></a>Trabajo con reglas de directivas de restricción de software

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se describen los procedimientos para trabajar con reglas de certificado, ruta de acceso, zona de Internet y hash mediante directivas de restricción de software.

## <a name="introduction"></a>Introducción
Con las directivas de restricción de software, puede proteger su entorno informático de software que no es de confianza mediante la identificación y especificación del software que se puede ejecutar. Puede definir un nivel de seguridad predeterminado no **restringido** o no **permitido** para un objeto de directiva de grupo (GPO) para que el software se permita o no se permita su ejecución de forma predeterminada. Puede crear excepciones a este nivel de seguridad predeterminado mediante la creación de reglas de directivas de restricción de software para software específico. Por ejemplo, si el nivel de seguridad predeterminado se establece en **No permitido**, puede crear reglas que permitan la ejecución de un software específico. Los tipos de reglas son los siguientes:

-   **Reglas de certificado**

    Para obtener información sobre los procedimientos, consulte [Trabajar con reglas de certificado](#BKMK_Cert_Rules).

-   **Reglas de hash**

    Para obtener información sobre los procedimientos, consulte [Trabajar con reglas de hash](#BKMK_Hash_Rules).

-   **Reglas de zona de Internet**

    Para ver los procedimientos, consulte [trabajar con reglas de zona de Internet](#BKMK_Internet_Zone_Rules).

-   **Reglas de ruta de acceso**

    Para obtener información sobre los procedimientos, consulte [Trabajar con reglas de ruta de acceso](#BKMK_Path_Rules).

Para obtener información acerca de otras tareas para administrar directivas de restricción de software, consulte [Administrar directivas de restricción de software](administer-software-restriction-policies.md).

## <a name="working-with-certificate-rules"></a><a name="BKMK_Cert_Rules"></a>Trabajar con reglas de certificado
Las directivas de restricción de software también pueden identificar software por su certificado de firma. Puede crear una regla de certificado que identifique el software y permita o impida la ejecución del software dependiendo del nivel de seguridad. Por ejemplo, puede usar reglas de certificado para confiar automáticamente en un software de un origen de confianza en un dominio sin necesidad de alertar al usuario. También puede emplear reglas de certificado para ejecutar archivos en áreas no permitidas de su sistema operativo. Las reglas de certificado no está habilitadas de forma predeterminada.

Cuando se crean reglas para el dominio mediante directiva de grupo, debe tener permisos para crear o modificar un objeto directiva de grupo. Si crea reglas para un equipo local, debe tener credenciales administrativas en ese equipo.

#### <a name="to-create-a-certificate-rule"></a>Para crear una regla de certificado

1.  Abra Directivas de restricción de software.

2.  En el árbol de consola o en el panel de detalles, haga clic con el botón secundario en **reglas adicionales** y, a continuación, haga clic en **nueva regla de certificado**.

3.  Haga clic en **Examinar** y, a continuación, seleccione un certificado o archivo firmado.

4.  En **nivel de seguridad**, haga clic en no **permitido** o no **restringido**.

5.  En **Descripción**, escriba una descripción para esta regla y haga clic en **Aceptar**.

> [!NOTE]
> -   Es posible que sea necesario crear una nueva configuración de directiva de restricción de software para el objeto de directiva de grupo (GPO), si aun no lo ha hecho.
> -   Las reglas de certificado no está habilitadas de forma predeterminada.
> -   Los únicos tipos de archivo a los que afectan las reglas de certificado son los que aparecen en la lista **Tipos de archivo designados**, en el panel de detalles de Directivas de restricción de software. Hay una lista de tipos de archivo designados que todas las reglas comparten.
> -   Para que las directivas de restricción de software surtan efecto, los usuarios deben cerrar la sesión e iniciar sesión en sus equipos.
> -   Cuando se aplica más de una regla de directivas de restricción de software a la configuración de Directiva, existe una prioridad de reglas para controlar los conflictos.

### <a name="enabling-certificate-rules"></a>Habilitar reglas de certificado
Dependiendo del entorno, existen diferentes procedimientos para habilitar las reglas de certificado:

-   [Para el equipo local](#BKMK_1)

-   [Para un objeto directiva de grupo y se encuentra en un servidor que está unido a un dominio](#BKMK_2)

-   [En el caso de un objeto de directiva de grupo, y se encuentre en un controlador de dominio o en una estación de trabajo que tenga el Herramientas de administración remota del servidor instalado](#BKMK_3)

-   [Solo para controladores de dominio, y se encuentran en un controlador de dominio o en una estación de trabajo que tiene instalado el paquete de Herramientas de administración remota del servidor](#BKMK_4)

#### <a name="to-enable-certificate-rules-for-your-local-computer"></a><a name="BKMK_1"></a>Para habilitar reglas de certificado para el equipo local

1.  Abra Configuración de seguridad local

2.  En el árbol de consola, haga clic en **Opciones de seguridad** , que se encuentra en configuración de seguridad/Directivas locales.

3.  En el panel de detalles, haga doble clic en **Configuración del sistema: usar reglas de certificado en archivos ejecutables de Windows para directivas de restricción de software**.

4.  Realice una de las siguientes acciones y, a continuación, haga clic en **Aceptar**:

    -   Para habilitar las reglas de certificado, haga clic en **Habilitada**.

    -   Para deshabilitar las reglas de certificado, haga clic en **Deshabilitada**.

#### <a name="to-enable-certificate-rules-for-a-group-policy-object-and-you-are-on-a-server-that-is-joined-to-a-domain"></a><a name="BKMK_2"></a>Para habilitar reglas de certificado para un objeto de directiva de grupo y se encuentra en un servidor que está unido a un dominio

1.  Abra Microsoft Management Console (MMC).

2.  En el menú **Archivo**, haga clic en **Agregar o quitar complemento** y, a continuación, en **Agregar**.

3.  Haga clic en **Editor de directivas de grupo local** y, a continuación, haga clic en **Agregar**.

4.  En **Seleccionar un objeto de directiva de grupo**, haga clic en **Examinar**.

5.  En **Buscar un objeto de directiva de grupo**, seleccione un objeto de directiva de grupo (GPO) en el dominio, sitio o unidad organizativa correspondiente, o cree uno nuevo y, a continuación, haga clic en **Finalizar**.

6.  Haga clic en **Cerrar** y después, en **Aceptar**.

7.  En el árbol de consola, haga clic en **Opciones de seguridad** , que se encuentra en *GroupPolicyObject* [*NombreDeEquipo*] directiva/configuración del equipo/Configuración de Windows/configuración de seguridad/Directivas locales/.

8.  En el panel de detalles, haga doble clic en **Configuración del sistema: usar reglas de certificado en archivos ejecutables de Windows para directivas de restricción de software**.

9. Si esta configuración de directiva no está aun definida, seleccione la casilla **Definir esta configuración de directiva**.

10. Realice una de las siguientes acciones y, a continuación, haga clic en **Aceptar**:

    -   Para habilitar las reglas de certificado, haga clic en **Habilitada**.

    -   Para deshabilitar las reglas de certificado, haga clic en **Deshabilitada**.

#### <a name="to-enable-certificate-rules-for-a-group-policy-object-and-you-are-on-a-domain-controller-or-on-a-workstation-that-has-the-remote-server-administration-tools-installed"></a><a name="BKMK_3"></a>Para habilitar reglas de certificado para un objeto de directiva de grupo, y se encuentre en un controlador de dominio o en una estación de trabajo que tenga el Herramientas de administración remota del servidor instalado

1.  Abra Usuarios y equipos de Active Directory.

2.  En el árbol de consola, haga clic con el botón secundario en el objeto de directiva de grupo (GPO) para el que quiera habilitar las reglas de certificado.

3.  Haga clic en **Propiedades** y, a continuación, haga clic en la pestaña **Directiva de grupo**.

4.  Haga clic en **Editar** para abrir el GPO que desee editar. También puede hacer clic en **Nuevo** para crear un nuevo GPO y, a continuación, haga clic en **Editar**.

5.  En el árbol de consola, haga clic en **Opciones de seguridad** , que se encuentra en *GroupPolicyObject*[*NombreDeEquipo*] directiva/configuración del equipo/Configuración de Windows/configuración de seguridad/Directivas locales.

6.  En el panel de detalles, haga doble clic en **Configuración del sistema: usar reglas de certificado en archivos ejecutables de Windows para directivas de restricción de software**.

7.  Si esta configuración de directiva no está aun definida, seleccione la casilla **Definir esta configuración de directiva**.

8.  Realice una de las siguientes acciones y, a continuación, haga clic en **Aceptar**:

    -   Para habilitar las reglas de certificado, haga clic en **Habilitada**.

    -   Para deshabilitar las reglas de certificado, haga clic en **Deshabilitada**.

#### <a name="to-enable-certificate-rules-for-only-domain-controllers-and-you-are-on-a-domain-controller-or-on-a-workstation-that-has-the-remote-server-administration-tools-installed"></a><a name="BKMK_4"></a>Para habilitar reglas de certificado solo para controladores de dominio y está en un controlador de dominio o en una estación de trabajo que tiene instalado el Herramientas de administración remota del servidor

1.  Abra Configuración de seguridad de controlador de dominio.

2.  En el árbol de consola, haga clic en **Opciones de seguridad**, en *GroupPolicyObject* [*NombreDeEquipo*] Directiva/Configuración del equipo/Configuración de Windows/Configuración de seguridad/Directivas locales.

3.  En el panel de detalles, haga doble clic en **Configuración del sistema: usar reglas de certificado en archivos ejecutables de Windows para directivas de restricción de software**.

4.  Si esta configuración de directiva no está aun definida, seleccione la casilla **Definir esta configuración de directiva**.

5.  Realice una de las siguientes acciones y, a continuación, haga clic en **Aceptar**:

    -   Para habilitar las reglas de certificado, haga clic en **Habilitada**.

    -   Para deshabilitar las reglas de certificado, haga clic en **Deshabilitada**.

> [!NOTE]
> Debe llevar a cabo este procedimiento antes de que el certificado pueda surtir efecto.

### <a name="set-trusted-publisher-options"></a>Establecer opciones de editores de confianza
La firma de software se usa cada vez más entre los publicadores de software y los programadores para comprobar si las aplicaciones proceden de un origen de confianza. No obstante, muchos usuarios no entienden o prestan poca atención a los certificados de firma asociados a las aplicaciones que instalan.

La configuración de directiva de la pestaña **Editores de confianza** de la directiva de validación de rutas de certificados permite a los administradores controlar qué certificados se pueden aceptar como procedentes de un editor de confianza.

##### <a name="to-configure-the-trusted-publishers-policy-settings-for-a-local-computer"></a>Para establecer la configuración de la directiva Editores de confianza en un equipo local

1.  En la pantalla **Inicio** , escriba **gpedit. msc** y, a continuación, presione Entrar.

2.  En el árbol de consola, en **Directiva de equipo local\Configuración del equipo\Configuración de Windows\Configuración de seguridad**, haga clic en **Directivas de clave pública**.

3.  Haga doble clic en **Configuración de validación de rutas de certificados** y, a continuación, haga clic en la pestaña **Editores de confianza**.

4.  Active la casilla **Definir esta configuración de directiva**, seleccione la configuración de directiva que desea aplicar y, a continuación, haga clic en **Aceptar** para aplicar la nueva configuración.

##### <a name="to-configure-the-trusted-publishers-policy-settings-for-a-domain"></a>Para establecer la configuración de la directiva Editores de confianza en un dominio

1.  Abre **Administración de directivas de grupo**.

2.  En el árbol de consola, haga doble clic en **Directiva de grupo objetos** del bosque y el dominio que contengan el objeto de directiva de grupo de **Directiva de dominio** (GPO) predeterminado que desee editar.

3.  Haga clic con el botón secundario en el GPO de la **Directiva predeterminada de dominio** y, a continuación, haga clic en **Editar**.

4.  En el árbol de consola, en **Configuración del equipo\Configuración de Windows\Configuración de seguridad**, haga clic en **Directivas de clave pública**.

5.  Haga doble clic en **Configuración de validación de rutas de certificados** y, a continuación, haga clic en la pestaña **Editores de confianza**.

6.  Active la casilla **Definir esta configuración de directiva**, seleccione la configuración de directiva que desea aplicar y, a continuación, haga clic en **Aceptar** para aplicar la nueva configuración.

##### <a name="to-allow-only-administrators-to-manage-certificates-used-for-code-signing-for-a-local-computer"></a>Para que solo los administradores puedan administrar los certificados usados para la firma de código en un equipo local

1.  En la pantalla **Inicio** , escriba, **gpedit. msc** en **Buscar programas y archivos** o en Windows 8, en el escritorio y, a continuación, presione Entrar.

2.  En el árbol de consola, en **Directiva de dominio predeterminada** o **Directiva de equipo local**, haga doble clic en **configuración del equipo**, **configuración de Windows** y configuración de **seguridad** y, a continuación, haga clic en **directivas de clave pública**.

3.  Haga doble clic en **Configuración de validación de rutas de certificados** y, a continuación, haga clic en la pestaña **Editores de confianza**.

4.  Active la casilla **Definir esta configuración de directiva**.

5.  En **Administración de editores de confianza**, haga clic en **Permitir que solo los administradores administren editores de confianza** y, a continuación, haga clic en **Aceptar** para aplicar la nueva configuración.

##### <a name="to-allow-only-administrators-to-manage-certificates-used-for-code-signing-for-a-domain"></a>Para que solo los administradores puedan administrar los certificados usados para la firma de código en un dominio

1.  Abre **Administración de directivas de grupo**.

2.  En el árbol de consola, haga doble clic en **Directiva de grupo objetos** del bosque y el dominio que contiene el GPO de **Directiva de dominio predeterminado** que desea editar.

3.  Haga clic con el botón secundario en el GPO de la **Directiva predeterminada de dominio** y, a continuación, haga clic en **Editar**.

4.  En el árbol de consola, en **Configuración del equipo\Configuración de Windows\Configuración de seguridad**, haga clic en **Directivas de clave pública**.

5.  Haga doble clic en **Configuración de validación de rutas de certificados** y, a continuación, haga clic en la pestaña **Editores de confianza**.

6.  Active la casilla **Definir esta configuración de directiva**, realice los cambios oportunos y, a continuación, haga clic en **Aceptar** para aplicar la nueva configuración.

## <a name="working-with-hash-rules"></a><a name="BKMK_Hash_Rules"></a>Trabajar con reglas de hash
Un hash es una serie de bytes con una longitud fija que identifica de forma única un programa de software o un archivo. El hash se calcula mediante un algoritmo hash. Cuando se crea una regla de hash para un programa de software, las directivas de restricción de software calculan el hash del programa. Cuando un usuario intenta abrir un programa de software, el hash del programa se compara con las reglas de hash existentes en las directivas de restricción de software. El hash de un programa de software siempre es el mismo, sin importar la ubicación del programa en el equipo. Sin embargo, si un programa de software se modifica de alguna forma, el hash correspondiente también cambiará y ya no coincidirá con el hash de la regla de hash de las directivas de restricción de software.

Por ejemplo, puede crear una regla de hash y establecer el nivel de seguridad en **No permitido** para impedir que los usuarios ejecuten un archivo determinado. Aunque un archivo se mueva a otra carpeta o se cambie su nombre, el hash resultante será el mismo. Sin embargo, los cambios realizados en el propio archivo también harán que cambie el valor de su hash y permitirán que el archivo omita las restricciones.

#### <a name="to-create-a-hash-rule"></a>Para crear una regla de hash

1.  Abra Directivas de restricción de software.

2.  En el árbol de consola o en el panel de detalles, haga clic con el botón secundario en **reglas adicionales** y, a continuación, haga clic en **nueva regla de hash**.

3.  Haga clic en **examinar** para buscar un archivo.

    > [!NOTE]
    > En Windows XP es posible pegar un hash previamente calculado en el **hash de archivo**. En Windows Server 2008 R2, Windows 7 y versiones posteriores, esta opción no está disponible.

4.  En **nivel de seguridad**, haga clic en no **permitido** o no **restringido**.

5.  En **Descripción**, escriba una descripción para esta regla y haga clic en **Aceptar**.

> [!NOTE]
> -   Es posible que sea necesario crear una nueva configuración de directiva de restricción de software para el objeto de directiva de grupo (GPO), si aun no lo ha hecho.
> -   Se puede crear una regla de hash para impedir la ejecución de un virus o un caballo de Troya.
> -   Si desea que otras personas usen una regla de hash para que no se pueda ejecutar un virus, calcule el hash del virus mediante directivas de restricción de software y, a continuación, enviar por correo electrónico el valor hash a las otras personas. Nunca envía por correo electrónico el propio virus.
> -   Si se ha enviado un virus por correo electrónico, también puede crear una regla de ruta de acceso para evitar la ejecución de datos adjuntos de correo electrónico.
> -   Aunque un archivo se mueva a otra carpeta o se cambie su nombre, el hash resultante será el mismo. Cualquier cambio en el propio archivo da como resultado un hash diferente.
> -   Los únicos tipos de archivo afectados por las reglas de hash son los que aparecen en la lista **Tipos de archivo designados** en el panel de detalles de las directivas de restricción de software. Hay una lista de tipos de archivo designados que todas las reglas comparten.
> -   Para que las directivas de restricción de software surtan efecto, los usuarios deben cerrar la sesión e iniciar sesión en sus equipos.
> -   Cuando se aplica más de una regla de directivas de restricción de software a la configuración de Directiva, existe una prioridad de reglas para controlar los conflictos.

## <a name="working-with-internet-zone-rules"></a><a name="BKMK_Internet_Zone_Rules"></a>Trabajar con reglas de zona de Internet
Las reglas de zona de Internet solo se aplican a los paquetes de Windows Installer. Una regla de zona puede identificar el software de una zona que se especifica a través de Internet Explorer. Estas zonas son Internet, Intranet local, Sitios restringidos, Sitios de confianza y Mi PC. Una regla de zona de Internet está diseñada para impedir que los usuarios descarguen e instalen software.

#### <a name="to-create-an-internet-zone-rule"></a>Para crear una regla de zona de Internet

1.  Abra Directivas de restricción de software.

2.  En el árbol de consola o en el panel de detalles, haga clic con el botón secundario en **reglas adicionales** y, a continuación, haga clic en **nueva regla de zona de Internet**.

3.  En **Zona de Internet**, haga clic en una zona de Internet.

4.  En **nivel de seguridad**, haga clic en no **permitido** o no **restringido** y, a continuación, haga clic en **Aceptar**.

> [!NOTE]
> -   Es posible que sea necesario crear una nueva configuración de directiva de restricción de software para el objeto de directiva de grupo (GPO), si aun no lo ha hecho.
> -   Las reglas de zona solo se aplican a tipos de archivo .msi, que son paquetes de Windows Installer
> -   Para que las directivas de restricción de software surtan efecto, los usuarios deben cerrar la sesión e iniciar sesión en sus equipos.
> -   Cuando se aplica más de una regla de directivas de restricción de software a la configuración de Directiva, existe una prioridad de reglas para controlar los conflictos.

## <a name="working-with-path-rules"></a><a name="BKMK_Path_Rules"></a>Trabajar con reglas de ruta de acceso
Una regla de ruta de acceso identifica el software por su ruta de acceso de archivo. Por ejemplo, aunque un equipo tenga establecido el nivel de seguridad predeterminado en **No permitido**, puede permitir el acceso sin restricciones a una determinada carpeta para cada usuario. Para crear una regla de ruta de acceso, puede emplear la ruta de acceso de archivo y establecer el nivel de seguridad de la regla de ruta de acceso en **Sin restricción**. Algunas rutas de acceso comunes para este tipo de regla son %userprofile%, %windir%, %appdata%, %programfiles% y %temp%. También puede crear reglas de ruta de acceso de Registro que utilicen la clave de Registro del software como ruta de acceso.

Como estas reglas se especifican a través de la ruta de acceso, si se mueve un programa de software, la regla de ruta de acceso dejará de aplicarse.

#### <a name="to-create-a-path-rule"></a>Para crear una regla de ruta de acceso

1.  Abra Directivas de restricción de software.

2.  En el árbol de consola o en el panel de detalles, haga clic con el botón secundario en **reglas adicionales** y, a continuación, haga clic en **nueva regla de ruta de acceso**.

3.  En **Ruta de acceso**, escriba una ruta de acceso o haga clic en **Examinar** para buscar un archivo o una carpeta.

4.  En **nivel de seguridad**, haga clic en no **permitido** o no **restringido**.

5.  En **Descripción**, escriba una descripción para esta regla y haga clic en **Aceptar**.

> [!CAUTION]
> -   En determinadas carpetas, como la carpeta Windows, establecer el nivel de seguridad en no **permitido** puede afectar negativamente al funcionamiento del sistema operativo. Asegúrese de permitir la ejecución de componentes esenciales del sistema operativo o uno de los programas que dependen de estos.

> [!NOTE]
> -   Es posible que sea necesario crear nuevas directivas de restricción de software para el objeto de directiva de grupo (GPO), si aun no lo ha hecho.
> -   Si crea una regla de ruta de acceso para software con un nivel de seguridad **No permitido**, los usuarios podrán ejecutar el software si lo copian en otra ubicación.
> -   Los caracteres comodín admitidos por la regla de ruta de acceso son * y ?.
> -   En la regla de ruta de acceso puede utilizar variables de entorno como %programfiles% o %systemroot%.
> -   Si desconoce en qué lugar del equipo está almacenado el software para el que desea crear una regla de ruta de acceso pero tiene su clave de Registro, puede crear una regla de ruta de acceso del Registro.
> -   Para evitar que los usuarios ejecuten datos adjuntos de correo electrónico, puede crear una regla de ruta de acceso para el directorio de datos adjuntos del programa de correo electrónico que impide que los usuarios ejecuten datos adjuntos.
> -   Los únicos tipos de archivo afectados por las reglas de ruta de acceso son los que aparecen en la lista **Tipos de archivo designados**, en el panel de detalles de Directivas de restricción de software. Hay una lista de tipos de archivo designados que todas las reglas comparten.
> -   Para que las directivas de restricción de software surtan efecto, los usuarios deben cerrar la sesión e iniciar sesión en sus equipos.
> -   Cuando se aplica más de una regla de directivas de restricción de software a la configuración de Directiva, existe una prioridad de reglas para controlar los conflictos.

#### <a name="to-create-a-registry-path-rule"></a>Para crear una regla de ruta de acceso del Registro

1.  En la pantalla **Inicio** , escriba regedit.

2.  En el árbol de consola, haga clic con el botón secundario en la clave de Registro para la que desea crear la regla y, a continuación, haga clic en **Copiar nombre de clave**. Observe el nombre del valor en el panel de detalles.

3.  Abra Directivas de restricción de software.

4.  En el árbol de consola o en el panel de detalles, haga clic con el botón secundario en **reglas adicionales** y, a continuación, haga clic en **nueva regla de ruta de acceso**.

5.  En **ruta de acceso**, pegue el nombre de la clave del registro, seguido del nombre del valor.

6.  Incluya la ruta de acceso del registro entre signos de porcentaje (%), por ejemplo,% HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PlatformSDK\Directories\InstallDir%.

7.  En **nivel de seguridad**, haga clic en no **permitido** o no **restringido**.

8.  En **Descripción**, escriba una descripción para esta regla y haga clic en **Aceptar**.



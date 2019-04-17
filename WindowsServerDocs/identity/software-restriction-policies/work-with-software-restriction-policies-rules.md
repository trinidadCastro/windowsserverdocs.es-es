---
title: "Trabajar con reglas de directivas de restricción de Software"
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4a8047d5-9bb9-4bed-bc8f-583a237731e2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 2dd1810b50f4f02be99eb2e2c0893501f99d1e93
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="work-with-software-restriction-policies-rules"></a>Trabajar con reglas de directivas de restricción de Software

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema describe los procedimientos de trabajar con certificado, ruta de acceso, internet zona y hash reglas con las directivas de restricción de Software.

## <a name="introduction"></a>Introducción
Con las directivas de restricción de software, puede proteger su entorno informático de software de confianza mediante la identificación y especificar qué software se puede ejecutar. Puedes definir un nivel de seguridad predeterminado **Unrestricted** o **no permitido** para un objeto de directiva de grupo (GPO) para que el software se permite o no pueden ejecutarse de forma predeterminada. Puedes hacer que las excepciones a este nivel de seguridad de forma predeterminada mediante la creación de reglas de directivas de software específico de restricción de software. Por ejemplo, si se establece el nivel de seguridad de forma predeterminada en **no permitido**, puedes crear las reglas que permitan la ejecución de software específico. Los tipos de reglas son los siguientes:

-   **Reglas de certificado**

    Para conocer los procedimientos, consulta [trabajar con reglas de certificado](#BKMK_Cert_Rules).

-   **Reglas de hash**

    Para conocer los procedimientos, consulta [trabajar con reglas de hash](#BKMK_Hash_Rules).

-   **Reglas de la zona de Internet**

    Para conocer los procedimientos, consulta [trabajar con reglas de la zona de Internet](#BKMK_Internet_Zone_Rules).

-   **Reglas de ruta**

    Para conocer los procedimientos, consulta [trabajar con reglas de ruta de acceso](#BKMK_Path_Rules).

Para obtener información acerca de otras tareas para administrar las directivas de restricción de Software, consulta [administrar las directivas de restricción de Software](administer-software-restriction-policies.md).

## <a name="BKMK_Cert_Rules"></a>Trabajar con reglas de certificado
Las directivas de restricción de software también pueden identificar software por su certificado de firma. Puedes crear una regla de certificado que identifica el software y, a continuación, permite o no permite que el software para ejecutar, según el nivel de seguridad. Por ejemplo, puedes usar reglas de certificado para confiar automáticamente software de un origen de confianza en un dominio sin pedir confirmación al usuario. También puedes usar reglas de certificado para ejecutar archivos en no permitidas áreas del sistema operativo. Las reglas de certificado no están habilitadas de manera predeterminada.

Cuando se crean reglas para el dominio mediante la directiva de grupo, debe tener permisos para crear o modificar un objeto de directiva de grupo. Si vas a crear reglas para el equipo local, debes tener credenciales administrativas en ese equipo.

#### <a name="to-create-a-certificate-rule"></a>Para crear una regla de certificado

1.  Abre las directivas de restricción de Software.

2.  En el árbol de consola o en el panel de detalles, haz clic en **reglas adicionales**y, a continuación, haz clic en **nueva regla de certificado**.

3.  Haz clic en **examinar**y, a continuación, selecciona un certificado o un archivo firmado.

4.  En **nivel de seguridad**, haz clic en **no permitido** o **Unrestricted**.

5.  En **descripción**, escribe una descripción de la regla y, a continuación, haz clic en **Aceptar**.

> [!NOTE]
> -   Podrían ser necesario para crear una nueva configuración de directiva de restricción de software para el objeto de directiva de grupo (GPO) si aún no lo has hecho.
> -   Las reglas de certificado no están habilitadas de manera predeterminada.
> -   Los únicos tipos de archivo que se ven afectados por las reglas de certificado son los que se enumeran en **tipos de archivo designados** en el panel de detalles para las directivas de restricción de Software. Hay una lista de tipos de archivo designados que comparte todas las reglas.
> -   Para que las directivas de restricción de software surta efecto, los usuarios deben actualizar la configuración de directiva de cierre de sesión e iniciar sesión en sus equipos.
> -   Cuando más de una regla de directivas de restricción de software se aplica a la configuración de directiva, no hay una prioridad de las reglas para tratar los conflictos.

### <a name="enabling-certificate-rules"></a>Permite que las reglas de certificado
Hay diferentes procedimientos para habilitar las reglas de certificado en función del entorno:

-   [Para el equipo local](#BKMK_1)

-   [Para un objeto de directiva de grupo, está en un servidor que se ha unido a un dominio](#BKMK_2)

-   [Para un objeto de directiva de grupo, está en un controlador de dominio o una estación de trabajo que tenga instaladas las herramientas de administración de servidor remoto](#BKMK_3)

-   [Solo dominio controladores, si está en un controlador de dominio o en una estación de trabajo que tiene el paquete de herramientas de administración de servidor remoto instalado](#BKMK_4)

#### <a name="BKMK_1"></a>Para habilitar las reglas de certificado para el equipo local

1.  Abre la configuración de seguridad Local.

2.  En el árbol de consola, haz clic en **opciones de seguridad** ubicados en las directivas de seguridad Local o configuración.

3.  En el panel de detalles, haz doble clic en **configuración del sistema: usar reglas de certificado en ejecutables de Windows para directivas de restricción de Software**.

4.  Realiza una de las siguientes opciones y, a continuación, haz clic en **Aceptar**:

    -   Para habilitar las reglas de certificado, haz clic en **habilitado**.

    -   Para deshabilitar las reglas de certificado, haz clic en **deshabilitado**.

#### <a name="BKMK_2"></a>Para habilitar las reglas de certificado para un objeto de directiva de grupo, si se encuentra en un servidor que se ha unido a un dominio

1.  Abre Microsoft Management Console (MMC).

2.  En la **archivo** menú, haz clic en **agregar o quitar complemento**y, a continuación, haz clic en **agregar**.

3.  Haz clic en **Editor de objetos de directiva de grupo Local**y, a continuación, haz clic en **agregar**.

4.  En **seleccionar un objeto de directiva de grupo**, haz clic en **examinar**.

5.  En **buscar un objeto de directiva de grupo**, selecciona un objeto de directiva de grupo (GPO) en el dominio, un sitio o una unidad organizativa- o crear uno nuevo y, a continuación, haz clic en **finalizar**.

6.  Haz clic en **cerrar**y, a continuación, haz clic en **Aceptar**.

7.  En el árbol de consola, haz clic en **opciones de seguridad** ubicada debajo *GroupPolicyObject* [*ComputerName*] directivas de seguridad de configuración de Windows o el equipo de directiva de configuración/Local /.

8.  En el panel de detalles, haz doble clic en **configuración del sistema: usar reglas de certificado en ejecutables de Windows para directivas de restricción de Software**.

9. Si aún no se ha definido esta configuración de directiva, selecciona el **definir esta configuración de directiva** casilla de verificación.

10. Realiza una de las siguientes opciones y, a continuación, haz clic en **Aceptar**:

    -   Para habilitar las reglas de certificado, haz clic en **habilitado**.

    -   Para deshabilitar las reglas de certificado, haz clic en **deshabilitado**.

#### <a name="BKMK_3"></a>Para habilitar el certificado de reglas para un objeto de directiva de grupo y estás en un controlador de dominio o en una estación de trabajo que tenga instaladas las herramientas de administración de servidor remoto

1.  Abre equipos y usuarios de Active Directory.

2.  En el árbol de consola, haz clic en el objeto de directiva de grupo (GPO) para el que desea habilitar las reglas de certificado.

3.  Haz clic en **propiedades**y, a continuación, haz clic en el **directiva de grupo** pestaña.

4.  Haz clic en **editar** para abrir el GPO que quieras editar. También puedes hacer clic en **nueva** para crear un nuevo GPO y, a continuación, haz clic en **editar**.

5.  En el árbol de consola, haz clic en **opciones de seguridad** ubicada debajo *GroupPolicyObject*[*ComputerName*] o el equipo de directiva de configuración de Windows las directivas de configuración/Local de seguridad.

6.  En el panel de detalles, haz doble clic en **configuración del sistema: usar reglas de certificado en ejecutables de Windows para directivas de restricción de Software**.

7.  Si aún no se ha definido esta configuración de directiva, selecciona el **definir esta configuración de directiva** casilla de verificación.

8.  Realiza una de las siguientes opciones y, a continuación, haz clic en **Aceptar**:

    -   Para habilitar las reglas de certificado, haz clic en **habilitado**.

    -   Para deshabilitar las reglas de certificado, haz clic en **deshabilitado**.

#### <a name="BKMK_4"></a>Para habilitar las reglas de certificado solo controladores de dominio y están en un controlador de dominio o en una estación de trabajo que tenga instaladas las herramientas de administración de servidor remoto

1.  Abre la configuración de seguridad del controlador de dominio.

2.  En el árbol de consola, haz clic en **opciones de seguridad** ubicada debajo *GroupPolicyObject* [*ComputerName*] o el equipo de directiva de configuración de Windows las directivas de configuración/Local de seguridad.

3.  En el panel de detalles, haz doble clic en **configuración del sistema: usar reglas de certificado en ejecutables de Windows para directivas de restricción de Software**.

4.  Si aún no se ha definido esta configuración de directiva, selecciona el **definir esta configuración de directiva** casilla de verificación.

5.  Realiza una de las siguientes opciones y, a continuación, haz clic en **Aceptar**:

    -   Para habilitar las reglas de certificado, haz clic en **habilitado**.

    -   Para deshabilitar las reglas de certificado, haz clic en **deshabilitado**.

> [!NOTE]
> Debes realizar este procedimiento para que las reglas de certificado surtan efecto.

### <a name="set-trusted-publisher-options"></a>Establece opciones editores de confianza
Firma de software que se usa por un número creciente de editores de software y los desarrolladores de aplicaciones para comprobar que sus aplicaciones provienen de un origen de confianza. Sin embargo, muchos usuarios entender o atender poco a los certificados de firma asociados con aplicaciones que instalan.

La configuración de directiva en el **editores de confianza** ficha de la directiva de validación de certificado ruta de acceso permite a los administradores a controlar los certificados que pueden ser aceptados como procedentes de un editor de confianza.

##### <a name="to-configure-the-trusted-publishers-policy-settings-for-a-local-computer"></a>Para establecer la configuración de directiva de editores de confianza para un equipo local

1.  En la **inicio** , escriba**gpedit.msc** y, a continuación, presione ENTRAR.

2.  En el árbol de consola **Local equipo local\Configuración configuración del equipo\Configuración Windows\Configuración**, haz clic en **directivas de clave pública**.

3.  Haz doble clic en **configuración de validación de rutas de certificados**y, a continuación, haz clic en el **editores de confianza** pestaña.

4.  Selecciona el **definir esta configuración de directiva** casilla de verificación, selecciona la configuración de directiva que quieras aplicar y, a continuación, haz clic en **Aceptar** para aplicar la configuración nueva.

##### <a name="to-configure-the-trusted-publishers-policy-settings-for-a-domain"></a>Para establecer la configuración de directiva de editores de confianza para un dominio

1.  Abre **administración de directivas de grupo**.

2.  En el árbol de consola, haz doble clic en **objetos de directiva de grupo** en el bosque y dominio que contiene la **directiva de dominio predeterminada** objeto de directiva de grupo (GPO) que quieres editar.

3.  Haz clic en el **directiva de dominio predeterminada** GPO y después haz clic en **editar**.

4.  En el árbol de consola **configuración del equipo\Configuración de Windows\Configuración de**, haz clic en **directivas de clave pública**.

5.  Haz doble clic en **configuración de validación de rutas de certificados**y, a continuación, haz clic en el **editores de confianza** pestaña.

6.  Selecciona el **definir esta configuración de directiva** casilla de verificación, selecciona la configuración de directiva que quieras aplicar y, a continuación, haz clic en **Aceptar** para aplicar la configuración nueva.

##### <a name="to-allow-only-administrators-to-manage-certificates-used-for-code-signing-for-a-local-computer"></a>Para permitir que solo los administradores administrar los certificados usados para la firma del código en un equipo local

1.  En la **inicio** pantalla, tipo, **gpedit.msc** en la **buscar programas y archivos** o en Windows 8, en el escritorio y, a continuación, presione ENTRAR.

2.  En el árbol de consola **directiva de dominio predeterminada** o **directiva de equipo Local**, haz doble clic en **configuración del equipo**, **configuración de Windows**, y **la configuración de seguridad**y, a continuación, haz clic en **directivas de clave pública**.

3.  Haz doble clic en **configuración de validación de rutas de certificados**y, a continuación, haz clic en el **editores de confianza** pestaña.

4.  Selecciona el **definir esta configuración de directiva** casilla de verificación.

5.  En **administración de editores de confianza**, haz clic en **permitir que solo los administradores administrar los editores de confianza**y, a continuación, haz clic en **Aceptar** para aplicar la configuración nueva.

##### <a name="to-allow-only-administrators-to-manage-certificates-used-for-code-signing-for-a-domain"></a>Para permitir que solo los administradores administrar los certificados usados para la firma del código en un dominio

1.  Abre **administración de directivas de grupo**.

2.  En el árbol de consola, haz doble clic en **objetos de directiva de grupo** en el bosque y dominio que contiene la **directiva de dominio predeterminada** GPO que quieras editar.

3.  Haz clic en el **directiva de dominio predeterminada** GPO y después haz clic en **editar**.

4.  En el árbol de consola **configuración del equipo\Configuración de Windows\Configuración de**, haz clic en **directivas de clave pública**.

5.  Haz doble clic en **configuración de validación de rutas de certificados**y, a continuación, haz clic en el **editores de confianza** pestaña.

6.  Selecciona el **definir esta configuración de directiva** casilla de verificación, implementar los cambios que quieras y, a continuación, haz clic en **Aceptar** para aplicar la configuración nueva.

## <a name="BKMK_Hash_Rules"></a>Trabajar con reglas de hash
Un hash es una serie de bytes de longitud fija que identifica un archivo o el programa de software. El hash se calcula mediante un algoritmo hash. Cuando se crea una regla de hash de un programa de software, las directivas de restricción de software calculan un hash del programa. Cuando un usuario intenta abrir un programa de software, un hash del programa se compara con las reglas de hash existentes para las directivas de restricción de software. El hash de un programa de software es siempre el mismo, independientemente de dónde se encuentra el programa en el equipo. Sin embargo, si un programa de software se modifica de ninguna manera, su hash también cambia y deja de coincidir con el hash de la regla de hash de las directivas de restricción de software.

Por ejemplo, puedes crear una regla de hash y establecer el nivel a la seguridad **no permitido** para impedir que los usuarios ejecuten un archivo determinado. Un archivo puede cambiar el nombre o se mueve a otra carpeta y aun así, el resultado en el mismo hash. Sin embargo, cualquier cambio en el propio archivo también cambia su valor de hash y permitir que el archivo de omitir las restricciones.

#### <a name="to-create-a-hash-rule"></a>Para crear una regla de hash

1.  Abre las directivas de restricción de Software.

2.  En el árbol de consola o en el panel de detalles, haz clic en **reglas adicionales**y, a continuación, haz clic en **nueva regla de Hash**.

3.  Haz clic en **examinar** para buscar un archivo.

    > [!NOTE]
    > En Windows XP es posible pegar un hash calculado previamente en **hash del archivo**. En Windows Server 2008 R2, Windows 7 y versiones posteriores, esta opción no está disponible.

4.  En **nivel de seguridad**, haz clic en **no permitido** o **Unrestricted**.

5.  En **descripción**, escribe una descripción de la regla y, a continuación, haz clic en **Aceptar**.

> [!NOTE]
> -   Puede ser necesario crear una nueva configuración de directiva de restricción de software para el objeto de directiva de grupo (GPO) si aún no lo has hecho.
> -   Una regla de hash se puede crear para que un virus o un troyano para impedir que se ejecute.
> -   Si quieres que otras personas usando una regla de hash para que no se puede ejecutar un virus, calcular el hash del virus mediante las directivas de restricción de software y de correo electrónico, a continuación, el valor de hash a los demás usuarios. Correo electrónico nunca el propio virus.
> -   Si se ha enviado un virus por correo electrónico, también puedes crear una regla de ruta de acceso para impedir la ejecución de datos adjuntos de correo electrónico.
> -   Un archivo que se cambió de nombre o se mueve a otra carpeta en el mismo hash. Cualquier cambio en el propio archivo de resultados en un valor de hash diferentes.
> -   Los únicos tipos de archivo que se ven afectados por las reglas de hash son los que se enumeran en **tipos de archivo designados** en el panel de detalles para las directivas de restricción de Software. Hay una lista de tipos de archivo designados que comparte todas las reglas.
> -   Para que las directivas de restricción de software surta efecto, los usuarios deben actualizar la configuración de directiva de cierre de sesión e iniciar sesión en sus equipos.
> -   Cuando más de una regla de directivas de restricción de software se aplica a la configuración de directiva, no hay una prioridad de las reglas para tratar los conflictos.

## <a name="BKMK_Internet_Zone_Rules"></a>Trabajar con reglas de la zona de Internet
Las reglas de la zona de Internet se aplican solo a los paquetes de Windows Installer. Una regla de zona puede identificar software de una zona que se especifica a través de Internet Explorer. Estas zonas son Internet, intranet Local, sitios restringidos, sitios de confianza y Mi PC. Una regla de zona de Internet está diseñada para impedir que los usuarios descarguen e instalen software.

#### <a name="to-create-an-internet-zone-rule"></a>Para crear una regla de la zona de Internet

1.  Abre las directivas de restricción de Software.

2.  En el árbol de consola o en el panel de detalles, haz clic en **reglas adicionales**y, a continuación, haz clic en **nueva regla de la zona de Internet**.

3.  En **zona Internet**, haz clic en una zona de Internet.

4.  En **nivel de seguridad**, haz clic en **no permitido** o **Unrestricted**y, a continuación, haz clic en **Aceptar**.

> [!NOTE]
> -   Puede ser necesario crear una nueva configuración de directiva de restricción de software para el objeto de directiva de grupo (GPO) si aún no lo has hecho.
> -   Las reglas de zona solo se aplican a los archivos con un tipo de archivo .msi, que son paquetes de Windows Installer.
> -   Para que las directivas de restricción de software surta efecto, los usuarios deben actualizar la configuración de directiva de cierre de sesión e iniciar sesión en sus equipos.
> -   Cuando más de una regla de directivas de restricción de software se aplica a la configuración de directiva, no hay una prioridad de las reglas para tratar los conflictos.

## <a name="BKMK_Path_Rules"></a>Trabajar con reglas de ruta de acceso
Una regla de ruta de acceso identifica el software por su ruta de acceso de archivo. Por ejemplo, si tienes un equipo que tiene un nivel de seguridad predeterminado **no permitido**, puede conceder acceso sin restricciones a una carpeta específica para cada usuario. Puedes crear una regla de ruta de acceso mediante la ruta de acceso de archivo y establecer el nivel de seguridad de la regla de ruta de acceso que **Unrestricted**. Algunas rutas de acceso comunes para este tipo de regla son % userprofile %, % windir %, % appdata %, % programfiles % y % temp %. También puedes crear del registro de las reglas de ruta de acceso que usan la clave del registro del software como su ruta de acceso.

Dado que estas reglas se especifican mediante la ruta de acceso, si se mueve a un programa de software, ya no se aplica la regla de ruta de acceso.

#### <a name="to-create-a-path-rule"></a>Para crear una regla de ruta de acceso

1.  Abre las directivas de restricción de Software.

2.  En el árbol de consola o en el panel de detalles, haz clic en **reglas adicionales**y, a continuación, haz clic en **nueva regla de ruta de acceso**.

3.  En **ruta de acceso**, escriba una ruta de acceso o haz clic en **examinar** para buscar un archivo o carpeta.

4.  En **nivel de seguridad**, haz clic en **no permitido** o **Unrestricted**.

5.  En **descripción**, escribe una descripción de la regla y, a continuación, haz clic en **Aceptar**.

> [!CAUTION]
> -   En determinadas carpetas, como la carpeta de Windows, configuración de la seguridad nivel a **no permitido** puede afectar negativamente al funcionamiento del sistema operativo. Asegúrate de no deshabilitar un componente fundamental del sistema operativo o uno de sus programas dependientes.

> [!NOTE]
> -   Puede ser necesario crear nuevas directivas de restricción de software para el objeto de directiva de grupo (GPO) si aún no lo has hecho.
> -   Si creas una regla de ruta de acceso para software con la seguridad de un nivel de **no permitido**, los usuarios pueden ejecutar el software copiándolo a otra ubicación.
> -   ¿Los caracteres comodín que son compatibles con la regla de ruta de acceso son * y?
> -   Puedes usar variables de entorno, como % programfiles % o % systemroot %, en la regla de ruta de acceso.
> -   Si quieres crear una regla de ruta de acceso para software cuando no sabes dónde se almacena en un equipo, pero tenga su clave del registro, puedes crear una regla de ruta de acceso del registro.
> -   Para evitar que los usuarios ejecuten datos adjuntos de correo electrónico, puedes crear una regla de ruta de acceso de directorio de datos adjuntos del programa de correo electrónico que impide que los usuarios ejecuten archivos adjuntos de correo electrónico.
> -   Los únicos tipos de archivo que se ven afectados por las reglas de ruta de acceso son los que se enumeran en **tipos de archivo designados** en el panel de detalles para las directivas de restricción de Software. Hay una lista de tipos de archivo designados que comparte todas las reglas.
> -   Para que las directivas de restricción de software surta efecto, los usuarios deben actualizar la configuración de directiva de cierre de sesión e iniciar sesión en sus equipos.
> -   Cuando más de una regla de directivas de restricción de software se aplica a la configuración de directiva, no hay una prioridad de las reglas para tratar los conflictos.

#### <a name="to-create-a-registry-path-rule"></a>Para crear una regla de ruta de acceso del registro

1.  En la **inicio** , escribe regedit.

2.  En el árbol de consola, haz clic en la clave del registro que quieres crear una regla para y, a continuación, haz clic en **Copiar nombre de clave**. Ten en cuenta el nombre del valor en el panel de detalles.

3.  Abre las directivas de restricción de Software.

4.  En el árbol de consola o en el panel de detalles, haz clic en **reglas adicionales**y, a continuación, haz clic en **nueva regla de ruta de acceso**.

5.  En **ruta de acceso**, pega el nombre de clave del registro, seguido del nombre de valor.

6.  Encierra la ruta de acceso del registro signos de porcentaje (%), por ejemplo, % HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PlatformSDK\Directories\InstallDir%.

7.  En **nivel de seguridad**, haz clic en **no permitido** o **Unrestricted**.

8.  En **descripción**, escribe una descripción de la regla y, a continuación, haz clic en **Aceptar**.



---
title: Opciones de Slmgr.vbs para obtener información de activación de volúmenes
description: Muestra las opciones disponibles para el script Slmg.vbs y describe cómo usarlas.
TOCTitle: Slmgr.vbs Options
ms.date: 09/24/2019
ms.technology: server-general
ms.topic: article
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
appliesto:
- Windows Server 2012 R2
- Windows 10
- Windows 8.1
ms.openlocfilehash: 3b1ce54fe1e1e855d605aba58a0f0e37f0324469
ms.sourcegitcommit: 9855d6b59b1f8722f39ae74ad373ce1530da0ccf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2019
ms.locfileid: "71963071"
---
# <a name="slmgrvbs-options-for-obtaining-volume-activation-information"></a>Opciones de Slmgr.vbs para obtener información de activación de volúmenes

A continuación se describe la sintaxis del script Slmgr.vbs, y en las tablas de este artículo se describe cada opción de línea de comandos.

```cmd
slmgr.vbs [<ComputerName> [<User> <Password>]] [<Options>]
```

> [!NOTE]
> En este artículo, los corchetes rectos \[] encierran los argumentos opcionales, y los corchetes angulares \< > delimitan los marcadores de posición. Cuando escribas estas instrucciones, omite los corchetes y reemplaza los marcadores de posición con los valores correspondientes.

> [!NOTE]
> Para obtener información sobre otros productos de software que usen la activación de volumen, consulta los documentos redactados específicamente para dichas aplicaciones.

## <a name="using-slmgr-on-remote-computers"></a>Usar Slmgr en equipos remotos

Para administrar clientes remotos, usa la Herramienta de administración de activación por volumen (VAMT) versión 1.2 o posterior, o bien crea scripts de WMI personalizados para detectar las diferencias entre plataformas. Para obtener más información sobre las propiedades y los métodos de WMI para la activación de volumen, consulta [WMI Properties and Methods for Volume Activation](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn502536(v=ws.11)) (Propiedades y métodos de WMI para la activación de volumen).

> [!IMPORTANT]
> Debido a los cambios de WMI en Windows 7 y Windows Server 2008 R2, el script Slmgr.vbs no funcionará en otras plataformas. No se admite el uso de Slmgr.vbs para administrar un sistema Windows 7 o Windows Server 2008 R2 desde el sistema operativo Windows Vista®. Si intentas administrar un sistema anterior desde Windows 7 o Windows Server 2008 R2, se generará un error específico de falta de coincidencia de versiones. Por ejemplo, al ejecutar **cscript slmgr.vbs \<nombre\_máquina\_vista\> /dlv** se produce el resultado siguiente:
>  
>> Microsoft (R) Windows Script Host versión 5.8 Copyright (C) Microsoft Corporation. Todos los derechos reservados.
>>  
>> El equipo remoto no admite esta versión de SLMgr.vbs

## <a name="general-slmgrvbs-options"></a>Opciones generales de Slmgr.vbs

|Opción |Descripción |
| - | - |
|\[\<NombreDeEquipo\>] |Nombre de un equipo remoto (el valor predeterminado es equipo local) |
|\[\<Usuario\>] |Cuenta que tiene los privilegios necesarios en el equipo remoto |
|\[\<Contraseña\>] |Contraseña de la cuenta que tiene los privilegios necesarios en el equipo remoto |

## <a name="global-options"></a>Opciones globales

|Opción |Descripción |
| - | - |
|\/ipk&nbsp;&lt;ClaveDeProducto&gt; |Intenta instalar una clave del producto de 5×5. La clave del producto proporcionada por el parámetro se confirma como válida y apropiada para el sistema operativo instalado.<br />En caso contrario, se devolverá un error.<br />Si la clave es válida y apropiada, se instalará. Si ya hay instalada una clave, se reemplazará sin mensajes.<br />Para garantizar la estabilidad del servicio de licencias es necesario reiniciar el sistema o el servicio de protección de software.<br />Esta operación debe ejecutarse desde una ventana del símbolo del sistema con privilegios elevados o, como alternativa, puede establecerse el valor del Registro de operaciones de usuario estándar para permitir a los usuarios sin privilegios acceso adicional al servicio de protección de software. |
|/ato&nbsp;\[\<Id de&nbsp;activación\>] |Para ediciones comerciales y sistemas de volumen que tengan instalada una clave de host de KMS o una clave de activación múltiple (MAK), **/ato** solicita a Windows que intente realizar la activación en línea.<br />Para sistemas que tengan instalada una clave de licencias por volumen genérica (GVLK), esta opción solicita una activación de KMS. Los sistemas configurados para suspender automáticamente los intentos de activación de KMS ( **/stao**) sí que intentarán realizar la activación de KMS al ejecutar **/ato**.<br />**Nota:** A partir de Windows 8 (y Windows Server 2012), la opción **/stao** está en desuso. En su lugar, usa la opción **/act-type**.<br />El parámetro \<**Id. de activación**\> amplía la capacidad de **/ato** para identificar una edición de Windows instalada en el equipo. Al especificar el parámetro \<**Id. de activación**\>, se aíslan los efectos de la opción a la edición asociada con dicho identificador de activación. Ejecuta **slmgr.vbs /dlv all** para obtener los identificadores de activación de la versión instalada de Windows. Si tienes que admitir otras aplicaciones, consulta las instrucciones específicas para cada aplicación en la guía.<br />La activación de KMS no requiere privilegios elevados. Pero la activación en línea sí que requiere el uso de privilegios elevados o, como alternativa, puedes establecerse el valor del Registro de operaciones de usuario estándar para permitir que los usuarios sin privilegios tengan acceso adicional al servicio de protección de software. |
|\/dli&nbsp;\[<Id. de&nbsp;activación\>&nbsp;\|&nbsp;All\] |Mostrar información de licencia.<br />De manera predeterminada, **/dli** muestra la información de licencia para la edición de Windows activa que esté instalada. Al especificar el parámetro \<**Id. de activación**\>, se muestra la información de licencia de la edición especificada que esté asociada con dicho identificador de activación. Al especificar **All** como parámetro, se muestra la información de licencia de todos los productos instalados correspondientes.<br />Esta operación no requiere privilegios elevados. |
|\/dlv&nbsp;\[<Id. de&nbsp;activación\>&nbsp;\|&nbsp;All\] |Mostrar información de licencia detallada.<br />De manera predeterminada, **/dlv** muestra la información de licencia del sistema operativo instalado. Al especificar el parámetro \<**Id. de activación**\>, se muestra la información de licencia de la edición especificada que esté asociada con dicho identificador de activación. Al especificar el parámetro **All**, se muestra la información de licencia de todos los productos instalados correspondientes.<br />Esta operación no requiere privilegios elevados. |
|\/xpr \[\<Id. de&nbsp;activación\>] |Muestra la fecha de expiración de la activación para el producto. De manera predeterminada, esto hace referencia a la edición de Windows actual y es útil principalmente para clientes de KMS, ya que la activación comercial y de MAK es perpetua.<br />Al especificar el parámetro \<**Id. de activación**\>, se muestra la fecha de expiración de activación de la edición especificada que está asociada con dicho identificador de activación. Esta operación no requiere privilegios elevados. |

## <a name="advanced-options"></a>Opciones avanzadas

|Opción |Descripción |
| - | - |
|\/cpky |Algunas operaciones de mantenimiento requieren que la clave del producto esté disponible en el Registro durante operaciones de configuración rápida (OOBE). La opción **/cpky** elimina la clave del producto del Registro para evitar que pueda ser sustraída por código malintencionado.<br />Se recomienda ejecutar esta opción en instalaciones que implementen claves. Esta opción no es obligatoria para claves de host de MAK y KMS, ya que es el comportamiento predeterminado para dichas claves. Esta opción solo es obligatoria para otros tipos de claves en los que el comportamiento predeterminado no es borrar la clave del Registro.<br />Esta operación debe ejecutarse en una ventana del símbolo del sistema con privilegios elevados. |
|\/ilc&nbsp;&lt;archivo_de_licencia&gt; |Esta opción instala el archivo de licencia especificado por el parámetro necesario. Estas licencias pueden instalarse como medida para la solución de problemas, para permitir la activación basada en token o como parte de una instalación manual de una aplicación integrada.<br />Las licencias no se validan durante este proceso: La validación de licencias no está dentro del ámbito de Slmgr.vbs. En lugar de ello, la validación es gestionada por el servicio de protección de software en el tiempo de ejecución.<br />Esta operación debe ejecutarse desde una ventana del símbolo del sistema con privilegios elevados o, como alternativa, puede establecerse el valor del Registro de **operaciones de usuario estándar** para permitir a los usuarios sin privilegios acceso adicional al servicio de protección de software. |
|\/rilc |Esta opción reinstala todas las licencias almacenadas en %SystemRoot%\system32\oem y %SystemRoot%\System32\spp\tokens. Estas son copias "válidas reconocidas" que se han almacenado durante la instalación.<br />Se reemplazarán todas las licencias que coincidan en el almacén de confianza. Las licencias adicionales&mdash;por ejemplo, de una autoridad de confianza [TA], licencias de emisión [IL] o licencias para aplicaciones&mdash;no se verán afectadas.<br />Esta operación debe ejecutarse en una ventana del símbolo del sistema con privilegios elevados o, como alternativa, puede establecerse el valor del Registro de **operaciones de usuario estándar** para permitir a los usuarios sin privilegios acceso adicional al servicio de protección de software. |
|\/rearm |Esta opción restablece los temporizadores de activación. El proceso **/rearm** también es invocado por **sysprep /generalize**.<br />Esta operación no hace nada si la entrada del Registro **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SoftwareProtectionPlatform\SkipRearm** se ha establecido en **1**. Consulte [Configuración del Registro para activación de volumen](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn502532(v=ws.11)) para obtener más información acerca de esta entrada del Registro.<br />Esta operación debe ejecutarse en una ventana del símbolo del sistema con privilegios elevados o, como alternativa, puede establecerse el valor del Registro de **operaciones de usuario estándar** para permitir a los usuarios sin privilegios acceso adicional al servicio de protección de software. |
|\/rearm-app &lt;Id. de&nbsp;aplicación&gt; |Restablece el estado de licencia de la aplicación especificada. |
|\/rearm-sku &lt;Id. de&nbsp;aplicación&gt; |Restablece el estado de licencia de la SKU especificada. |
|\/upk&nbsp;\[&lt;Id. de&nbsp;aplicación&gt;] |Esta opción desinstala la clave del producto de la edición de Windows actual. Después de un reinicio, el sistema estará en un estado sin licencia hasta que se instale una clave del producto.<br />De manera opcional, puedes usar el parámetro \<**Id. de activación**\> para especificar un producto instalado distinto.<br />Esta operación debe ejecutarse desde una ventana del símbolo del sistema con privilegios elevados. |
|\/dti&nbsp;\[\<Id. de&nbsp;activación\>] |Muestra el identificador de la instalación para la activación sin conexión. |
|\/atp &lt;Id. de&nbsp;confirmación&gt; |Activa el producto con el identificador de confirmación proporcionado por el usuario. |

## <a name="kms-client-options"></a>Opciones de cliente de KMS

|Opción |Descripción |
| - | - |
|\/skms \<Nombre\[:Puerto]&nbsp;\|&nbsp;\:&nbsp;puerto\> \[\<Id. de&nbsp;activación\>] |Esta opción especifica el nombre y, de manera opcional, el puerto del equipo host de KMS que se contactará. Al establecer este valor se deshabilita la detección automática del host de KMS.<br />Si el host de KMS usa únicamente el protocolo de internet versión 6 (IPv6), la dirección debe especificarse en el formato \<nombre de host\>:\<puerto\>. Las direcciones IPv6 contienen un carácter de dos puntos (:), que el script Slmgr.vbs no analiza de forma correcta.<br />Esta operación debe ejecutarse en una ventana del símbolo del sistema con privilegios elevados. |
|\/skms-domain&nbsp;&lt;FQDN&gt; \[\<Id. de&nbsp;activación\>] |Establece el dominio DNS específico donde pueden encontrarse todos los registros SVR de KMS. Esta opción no surte efecto si el host de KMS único específico se establece mediante la opción **/skms**. Esta opción se usa, especialmente en entornos de espacio de nombres, para obligar a KMS a ignorar la lista de búsqueda de sufijos DNS y, en su lugar, buscar los registros del host de KMS en el dominio DNS especificado. |
|\/ckms&nbsp;\[\<Id. de&nbsp;activación\>] |Esta opción elimina el nombre de host de KMS especificado, la dirección y el puerto del Registro y restaura el comportamiento detección automática.<br />Esta operación debe ejecutarse en una ventana del símbolo del sistema con privilegios elevados. |
|\/skhc |Esta opción habilita el almacenamiento en caché de hosts de KMS (predeterminado). Después de que el cliente detecta un host de KMS en funcionamiento, esta configuración evita que la prioridad y el peso del sistema de nombres de dominio (DNS) afecten a la comunicación adicional con el host. Si el sistema ya no puede ponerse en contacto con el host de KMS en funcionamiento, el cliente intenta detectar un nuevo host.<br />Esta operación debe ejecutarse en una ventana del símbolo del sistema con privilegios elevados. |
|\/ckhc |Esta opción deshabilita el almacenamiento en caché de hosts de KMS. Esta opción indica al cliente que use la detección automática de DNS siempre que intente realizar una activación de KMS (recomendado al usar prioridad y peso).<br />Esta operación debe ejecutarse en una ventana del símbolo del sistema con privilegios elevados. |

## <a name="kms-host-configuration-options"></a>Opciones de configuración de host de KMS

|Opción |Descripción |
| - | - |
|\/sai&nbsp;&lt;Intervalo&gt; |Esta opción establece el intervalo en minutos para clientes sin activar que intentan conectarse a KMS. El intervalo de activación debe ser de 15 minutos como mínimo y de 30 días como máximo, aunque se recomienda usar el valor predeterminado (2 horas).<br />Inicialmente, el cliente de KMS toma este intervalo del Registro, pero cambia a la opción KMS después de recibir la primera respuesta de KMS.<br />Esta operación debe ejecutarse en una ventana del símbolo del sistema con privilegios elevados. |
|\/sri &lt;Intervalo&gt; |Esta opción establece el intervalo de renovación en minutos los clientes activados que intentan conectarse a KMS. El intervalo de renovación debe ser de 15 minutos como mínimo y de 30 días como máximo. Esta opción se establece inicialmente tanto en el lado servidor como en el lado cliente de KMS. El valor predeterminado es 10.080 minutos (7 días).<br />Inicialmente, el cliente KMS toma este intervalo del Registro, pero cambia a la opción KMS después de recibir la primera respuesta de KMS.<br />Esta operación debe ejecutarse en una ventana del símbolo del sistema con privilegios elevados. |
|\/sprt&nbsp;&lt;Puerto&gt; |Esta opción establece el puerto en el que el host de KMS busca solicitudes de activación de clientes. El puerto TCP predeterminado es el 1688.<br />Esta operación debe ejecutarse desde una ventana del símbolo del sistema con privilegios elevados. |
|\/sdns |Habilita la publicación en DNS por el host de KMS (predeterminado).<br />Esta operación debe ejecutarse en una ventana del símbolo del sistema con privilegios elevados. |
|\/cdns |Deshabilita la publicación en DNS por el host de KMS.<br />Esta operación debe ejecutarse en una ventana del símbolo del sistema con privilegios elevados. |
|\/spri |Establece la prioridad de KMS en normal (predeterminado).<br />Esta operación debe ejecutarse en una ventana del símbolo del sistema con privilegios elevados. |
|\/cpri |Establece la prioridad de KMS en baja.<br />Usa esta opción para minimizar la contención de KMS en un entorno de hospedaje compartido. Ten en cuenta que esto podría causar el colapso de KMS, según las aplicaciones o roles de servidor que estén activos. Úsalo con precaución.<br />Esta operación debe ejecutarse en una ventana del símbolo del sistema con privilegios elevados. |
|\/act-type \[\<Tipo-Activación\>] \[\<Id. de&nbsp;activación\>] |Esta opción establece un valor en el Registro que limita la activación de volumen a un solo tipo. El tipo de activación **1** limita la activación a Active Directory solo, **2** limita a la activación de KMS y **3** a la activación basada en token. La opción **0** permite cualquier tipo de activación y es el valor predeterminado. |

## <a name="token-based-activation-configuration-options"></a>Opciones de configuración de activación basada en token

|Opción |Descripción |
| - | - |
|/lil |Proporciona la lista de licencias de emisión de activación basada en token que están instaladas. |
|\/ril&nbsp;&lt;ILID&gt;&nbsp;&lt;ILvID&gt; |Permite quitar una licencia de emisión de activación basada en token instalada.<br />Esta operación debe ejecutarse desde una ventana del símbolo del sistema con privilegios elevados. |
|\/stao |Establece la marca **Token-based Activation Only**, lo que deshabilita la activación de KMS automática.<br />Esta operación debe ejecutarse en una ventana del símbolo del sistema con privilegios elevados.<br />Esta opción se ha quitado en Windows Server 2012 R2 y Windows 8.1. En su lugar, usa la opción **/act–type**. |
|\/ctao |Borra la marca **Solo activación basada en token** (predeterminado), lo que habilitará la activación de KMS automática.<br />Esta operación debe ejecutarse en una ventana del símbolo del sistema con privilegios elevados.<br />Esta opción se ha quitado en Windows Server 2012 R2 y Windows 8.1. En su lugar, usa la opción **/act-type**. |
|\/ltc |Proporciona una lista de certificados válidos de activación basada en token que pueden activar software instalado. |
|\/fta &lt;Huella digital&nbsp;de certificado&gt; \[&lt;PIN&gt;] |Fuerza la activación basada en token mediante el certificado identificado. El número de identificación personal (PIN) es opcional y se proporciona para desbloquear la clave privada sin la solicitud del PIN si usas certificados protegidos mediante hardware (por ejemplo, tarjetas inteligentes). |

## <a name="active-directory-based-activation-configuration-options"></a>Opciones de configuración de activación basada en Active Directory

|Opción |Descripción |
| - | - |
|\/ad-activation-online &lt;Clave de&nbsp;producto&gt; \[\<Nombre de&nbsp;objeto de&nbsp;activación\>] |Recopila datos de Active Directory e inicia la activación del bosque de Active Directory mediante las credenciales ejecutadas por el símbolo del sistema. No es necesario disponer de acceso de administrador local. Sin embargo, sí es necesario disponer de acceso de lectura y escritura al contenedor del objeto de activación, en el dominio raíz del bosque. |
|\/ad-activation-get-IID &lt;Clave de&nbsp;producto&gt; |Esta opción inicia la activación del bosque de Active Directory en modo de teléfono. Como resultado, se obtiene el identificador de la instalación (IID), que puede usarse para activar el bosque por teléfono si no se dispone de conexión a Internet. Después de proporcionar el IID en la llamada telefónica de activación, se obtendrá un CID necesario para completar la activación. |
|\/ad-activation-apply-cid &lt;Clave de&nbsp;producto&gt; &lt;Id. de&nbsp;confirmación&gt; \[\<Nombre de&nbsp;objeto de&nbsp;activación>] |Al usar esta opción, especifica el CID que se proporcionó en la llamada telefónica para completar la activación. |
|\[/name: &lt;Nombre_OA&gt;] |De manera opcional, puedes anexar la opción **/name** a cualquiera de estos comandos para especificar un nombre para el objeto de activación almacenado en Active Directory. El nombre no debe superar los 40 caracteres Unicode. Usa comillas dobles para definir explícitamente la cadena de nombre.<br />En Windows Server 2012 R2 y Windows 8.1, puedes anexar el nombre directamente después de **/ad-activation-online &lt;Clave de&nbsp;producto&gt;** y **/ad-activation-apply-cid**, sin que sea necesario usar la opción **/name**. |
|\/ao-list |Muestra todos los objetos de activación disponibles para el equipo local. |
|\/del-ao &lt;DN_OA&gt;<br />\/del-ao &lt;RDN_OA&gt; |Elimina del bosque el objeto de activación especificado. |

## <a name="see-also"></a>Consulte también

- [Referencia técnica de la activación de volumen](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn502529%28v%3dws.11%29)
- [Introducción a la activación de volumen](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831612%28v%3dws.11%29)


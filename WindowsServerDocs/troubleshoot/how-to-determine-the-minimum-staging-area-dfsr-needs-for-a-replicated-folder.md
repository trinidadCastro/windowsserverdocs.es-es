---
title: Cómo determinar el área de ensayo mínima que necesita DFSR para una carpeta replicada
description: Este artículo es una guía de referencia rápida sobre cómo calcular el área de ensayo mínima necesaria para que DFSR funcione correctamente.
ms.date: 06/10/2020
author: Deland-Han
ms.author: delhan
ms.topic: troubleshooting
ms.openlocfilehash: c15dd18c5c479ea2e280a333f8b4ec808ab70f90
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946931"
---
# <a name="how-to-determine-the-minimum-staging-area-dfsr-needs-for-a-replicated-folder"></a>Cómo determinar el área de ensayo mínima que necesita DFSR para una carpeta replicada

Este artículo es una guía de referencia rápida sobre cómo calcular el área de ensayo mínima necesaria para que DFSR funcione correctamente. Los valores inferiores a estos pueden hacer que la replicación se realice lentamente o se detenga por completo.

Tenga en cuenta que *solo son mínimos*. Al considerar el tamaño del área de ensayo, cuanto mayor sea el área de almacenamiento provisional, mayor será el tamaño de la carpeta replicada. Consulte la sección "cómo determinar si tiene un problema de área de ensayo" y las entradas de blog vinculadas al final de este artículo para obtener más información sobre por qué es importante tener un área de ensayo con el tamaño correcto.

> [!Note]
> También tenemos una revisión para ayudarle a calcular los tamaños de almacenamiento provisional. [La actualización de la interfaz de administración de Replicación DFS (DFSR) está disponible](https://support.microsoft.com/kb/2607047)

## <a name="rules-of-thumb"></a>Reglas generales

**Windows Server 2003 R2** : la cuota del área de almacenamiento provisional debe ser tan grande como los 9 archivos de mayor tamaño de la carpeta replicada.

**Windows Server 2008 y 2008 R2** : la cuota del área de almacenamiento provisional debe ser tan grande como los 32 archivos de mayor tamaño de la carpeta replicada.

La replicación inicial hará mucho más uso del área de ensayo que la replicación diaria. Se recomienda encarecidamente establecer el área de ensayo superior al mínimo durante la replicación inicial si tiene el espacio de la unidad disponible.

## <a name="where-do-i-get-powershell"></a>¿Dónde puedo obtener PowerShell?

PowerShell se incluye en Windows 2008 y versiones posteriores. Debe instalar PowerShell en Windows Server 2003. Puede descargar PowerShell para Windows 2003 [aquí](https://support.microsoft.com/kb/968930).

## <a name="how-do-you-find-these-x-largest-files"></a>¿Cómo se encuentran estos X archivos más grandes?

Use un script de PowerShell para buscar los archivos de mayor tamaño 32 o 9 y determinar cuántos gigabytes agrega hasta (gracias a tendrá Pyle para los comandos de PowerShell). En realidad, voy a presentar tres scripts de PowerShell. Cada es útil por sí solo; sin embargo, el número 3 es el más útil.

1. Ejecute el siguiente comando:
   ```Powershell
   Get-ChildItem c:\\temp -recurse | Sort-Object length -descending | select-object -first 32 | ft name,length -wrap –auto
   ```

   Este comando devolverá los nombres de archivo y el tamaño de los archivos en bytes. Resulta útil si desea saber qué archivos de 32 son los más grandes en la carpeta replicada para que pueda "visitar" sus propietarios.

2. Ejecute el siguiente comando:
   ```Poswershell
   Get-ChildItem c:\\temp -recurse | Sort-Object length -descending | select-object -first 32 | measure-object -property length –sum
   ```
   Este comando devolverá el número total de bytes de los archivos 32 de mayor tamaño de la carpeta sin enumerar los nombres de archivo.

3. Ejecute el siguiente comando:
   ```Poswershell
   $big32 = Get-ChildItem c:\\temp -recurse | Sort-Object length -descending | select-object -first 32 | measure-object -property length –sum

   $big32.sum /1gb
   ```
   Este comando obtendrá el número total de bytes de 32 archivos de mayor tamaño en la carpeta y realizará las operaciones matemáticas para convertir los bytes en gigabytes. Este comando es dos líneas independientes. Puede pegarlas en el shell de comandos de PowerShell a la vez o volver a ejecutarlas de nuevo.

## <a name="manual-walkthrough"></a>Tutorial manual

Para demostrar el proceso y, con suerte, aumentar la comprensión de lo que estamos haciendo, voy a recorrer manualmente cada parte.

Al ejecutar el comando 1 se devolverán resultados similares a la salida siguiente. En este ejemplo solo se usan 16 archivos por motivos de brevedad. Usar siempre 32 para Windows 2008 y sistemas operativos posteriores y 9 para Windows 2003 R2

### <a name="example-data-returned-by-powershell"></a>Datos de ejemplo devueltos por PowerShell

<table>
<tbody>
<tr class="odd">
<td>Nombre</td>
<td>Length</td>
</tr>
<tr class="even">
<td><strong>File5.zip</strong></td>
<td>10286089216</td>
</tr>
<tr class="odd">
<td><strong>archive.zip</strong></td>
<td>6029853696</td>
</tr>
<tr class="even">
<td><strong>BACKUP.zip</strong></td>
<td>5751522304</td>
</tr>
<tr class="odd">
<td> <strong>file9.zip</strong></td>
<td>5472683008</td>
</tr>
<tr class="even">
<td><strong>MENTOS.zip</strong></td>
<td>5241586688</td>
</tr>
<tr class="odd">
<td><strong>File7.zip</strong></td>
<td>4321264640</td>
</tr>
<tr class="even">
<td><strong>file2.zip</strong></td>
<td>4176765952</td>
</tr>
<tr class="odd">
<td><strong>frd2.zip</strong></td>
<td>4176765952</td>
</tr>
<tr class="even">
<td><strong>BACKUP.zip</strong></td>
<td>4078994432</td>
</tr>
<tr class="odd">
<td><strong>File44.zip</strong></td>
<td>4058424320</td>
</tr>
<tr class="even">
<td><strong>file11.zip</strong></td>
<td>3858056192</td>
</tr>
<tr class="odd">
<td><strong>Backup2.zip</strong></td>
<td>3815138304</td>
</tr>
<tr class="even">
<td><strong>BACKUP3.zip</strong></td>
<td>3815138304</td>
</tr>
<tr class="odd">
<td><strong>Current.zip</strong></td>
<td>3576931328</td>
</tr>
<tr class="even">
<td><strong>Backup8.zip</strong></td>
<td>3307488256</td>
</tr>
<tr class="odd">
<td><strong>File999.zip</strong></td>
<td>3274982400</td>
</tr>
</tbody>
</table>

### <a name="how-to-use-this-data-to-determine-the-minimum-staging-area-size"></a>Cómo usar estos datos para determinar el tamaño mínimo del área de ensayo:

  - Nombre = nombre del archivo.
  - Longitud = bytes
  - Un Gigabyte = 1073741824 bytes

En primer lugar, debe sumar el número total de bytes. A continuación, divida el total por 1073741824. Sugiero usar Excel o la hoja de cálculo que prefiera para realizar las operaciones matemáticas.

### <a name="example"></a>Ejemplo

En el ejemplo anterior, el número total de bytes = 75241684992. Para obtener la cuota de área de ensayo mínima necesaria, es necesario dividir 75241684992 por 1073741824.

> 75241684992/1073741824 = 70,07 GB

En función de estos datos, establecería mi área de ensayo en 71 GB si redondeo al número entero más cercano.

### <a name="real-world-scenario"></a>Escenario real:

Aunque un tutorial manual es interesante, es probable que no sea el mejor uso de su tiempo para realizar las operaciones matemáticas. Para automatizar el proceso, use el comando 3 de los ejemplos anteriores. Los resultados tendrán el siguiente aspecto

> [![impresión](https://msdnshared.blob.core.windows.net/media/TNBlogsFS/prod.evol.blogs.technet.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/58/02/metablogapi/8204.image_thumb_02CB3914.png "imagen")](https://msdnshared.blob.core.windows.net/media/TNBlogsFS/prod.evol.blogs.technet.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/58/02/metablogapi/0876.image_03A39EFE.png)

Con el comando de ejemplo 3 sin ningún esfuerzo adicional, excepto para el redondeo al número entero más próximo, puedo determinar que necesito una cuota de área de ensayo de 6 GB para d: \\ docs.

## <a name="do-i-need-to-reboot-or-restart-the-service-for-the-changes-to-be-picked-up"></a>¿Es necesario reiniciar o reiniciar el servicio para que se recojan los cambios?

Los cambios en la cuota del área de ensayo no requieren el reinicio o el reinicio del servicio para que surtan efecto. Tendrá que esperar a la replicación de AD y al ciclo de sondeo de AD de DFSR para que se apliquen los cambios.

## <a name="how-to-determine-if-you-have-a-staging-area-problem"></a>Cómo determinar si tiene un problema de área de ensayo

Los problemas del área de ensayo se detectan supervisando los identificadores de eventos específicos en los servidores de DFSR. La lista de eventos es 4202, 4204, 4206, 4208 y 4212. Los textos de estos eventos se enumeran a continuación. Es importante distinguir entre 4202 y 4204 y otros eventos. Es posible registrar un gran número de eventos 4202 y 4204 en condiciones de funcionamiento normales. Imagine que los eventos 4202 y 4204 son análogos a tomar el pulso, mientras que los de 4206, 4208 y 4212 son como dolor de pecho. Explicaremos a continuación cómo interpretar los eventos 4202 y 4204 a continuación.

### <a name="staging-area-events"></a>Eventos del área de ensayo

> ID. de evento: **4202** gravedad: **ADVERTENCIA**
>
> El servicio de Replicación DFS ha detectado que el espacio de almacenamiento provisional que se usa para la carpeta replicada en la ruta de acceso local (PATH) está por encima del límite máximo. El servicio intentará eliminar los archivos provisionales más antiguos. El rendimiento puede verse afectado.
>
> ID. de evento: **4204** gravedad: **informativo**
>
> El servicio de Replicación DFS ha eliminado correctamente los archivos de almacenamiento provisional antiguos de la carpeta replicada en la ruta de acceso local (ruta de acceso). El espacio de almacenamiento provisional ahora está por debajo del límite máximo.
>
> ID. de evento: **4206** gravedad: **ADVERTENCIA**
>
> El servicio Replicación DFS no pudo limpiar los archivos provisionales antiguos de la carpeta replicada en la ruta de acceso local (ruta de acceso). Es posible que el servicio no pueda replicar algunos archivos grandes y que la carpeta replicada no esté sincronizada. El servicio volverá a intentar automáticamente la limpieza del espacio de ensayo en (x) minutos. El servicio puede iniciar la limpieza antes si detecta que se han desbloqueado algunos archivos de almacenamiento provisional.
>
> ID. de evento: **4208** gravedad: **ADVERTENCIA**
>
> El servicio Replicación DFS detectó que el uso del espacio de almacenamiento provisional está por encima de la cuota de almacenamiento provisional de la carpeta replicada en la ruta de acceso local (ruta de acceso). Es posible que el servicio no pueda replicar algunos archivos grandes y que la carpeta replicada no esté sincronizada. El servicio intentará limpiar automáticamente el espacio de almacenamiento provisional.
>
> ID. de evento: **4212** gravedad: **error**
>
> El servicio Replicación DFS no pudo replicar la carpeta replicada en la ruta de acceso local (ruta de acceso) porque la ruta de acceso de almacenamiento provisional no es válida o no es accesible.

## <a name="what-is-the-difference-between-4202-and-4208"></a>¿Cuál es la diferencia entre 4202 y 4208?

Los eventos 4202 y 4208 tienen texto similar. es decir, DFSR detectó que el uso del área de almacenamiento provisional supera el límite máximo. La diferencia es que 4208 se registra una vez que se ha ejecutado la limpieza del área de ensayo y se sigue superando la cuota de ensayo. 4202 es un evento normal y esperado, mientras que 4208 es anómalo y requiere intervención.

## <a name="how-many-4202-4204-events-are-too-many"></a>¿Cuántos eventos 4202 y 4204 son demasiado numerosos?

No hay ninguna respuesta única a esta pregunta. A diferencia de los eventos 4206, 4208 o 4212, que siempre son incorrectos e indican que es necesaria la acción, los eventos 4202 y 4204 se producen en condiciones de funcionamiento normales. Ver muchos eventos 4202 y 4204 *puede* indicar un problema. Puntos que se deben tener en cuenta:

1.  ¿El registro de la carpeta replicada (RF) 4202 realiza la replicación inicial? Si es así, es normal registrar los eventos 4202 y 4204. Querrá mantenerlos hasta el menor número posible durante la replicación inicial al proporcionar la mayor cantidad de área de ensayo posible
2.  Simplemente no es suficiente comprobar el número total de eventos 4202. Tiene que saber cuántos se registraron por RF. Si registra 20 4202 eventos para un RF en un período de 24 horas que es alto. Sin embargo, si tiene 20 carpetas replicadas y hay un evento por cada carpeta, lo está haciendo bien.
3.  Debe examinar varios días de datos para establecer tendencias.

Normalmente, los clientes no pueden permitir más de 1 4202 evento por cada carpeta replicada al día en condiciones de funcionamiento normales. "Normal", lo que significa que no se está produciendo la replicación inicial. Esto se basa en la razón por la que:

1.  El tiempo empleado en la limpieza del área de ensayo es el tiempo invertido en no replicar archivos. La replicación se pausa mientras se borra el área de ensayo.
2.  DFSR se beneficia de un área de ensayo completa para RDC y RDC entre archivos o la replicación de los mismos archivos en otros miembros.
3.  Los eventos más 4202 y 4204 en los que se registra la mayor probabilidad de que se ejecuten en la condición en la que DFSR no puede limpiar el área de almacenamiento provisional o tendrá que purgar los archivos del área de ensayo prematuramente.
4.  los eventos 4206, 4208 y 4212 son, en mi experiencia, siempre precedidos y seguidos de un gran número de eventos 4202 y 4204.

Aunque permitir solo 1 4202 evento por RF al día es conservador, reduce en gran medida las probabilidades de que se ejecuten en los problemas del área de ensayo y utiliza mejor los recursos del servidor de DFSR para la finalidad prevista de la replicación de archivos.



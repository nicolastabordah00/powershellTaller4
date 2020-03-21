## EJERCICIOS
1. ¿Cuál clase puede emplearse para consultar la dirección IP de un adaptador
   de red? ¿Posee dicha clase algún método para liberar un préstamo de
   dirección (lease) DHCP?
   
   Se puede consultar con la clase ``win32_NetworkAdapterconfiguration`` y no tiene un metodo para liberar el prestamo de dirección.
   
2. Despliegue una lista de parches empleando WMI (Microsoft se refiere a los
   parches con el nombre **quick-fix engineering**). Es diferente el listado al
   que produce el cmdlet ``Get-Hotfix``?
   ```console
   PS C:\Users\Nicolas> Get-WmiObject win32_quickfixengineering
   ```
   Si es diferente.
   
3. Empleando WMI, muestre una lista de servicios, que incluya su status actual,
   su modalidad de inicio, y las cuentas que emplean para hacer login.
   ```console
   PS C:\Users\Nicolas> Get-WmiObject win32_service | Select-Object status, startmode, systemname
   ```
4. Empleando cmdlets de CIM, liste todas las clases del namespace
   ``SecurityCenter2``, que tengan **product** como parte del nombre.
   ```console
   PS C:\Users\Nicolas> Get-CimClass -Namespace root/SecurityCenter2 | where -Filter {$_.CimClassName -like "*product*"}
   ```
5. Empleando cmdlets de CIM, y los resultados del ejercicio anterior, muestre
   los nombres de las aplicaciones antispyware instaladas en el sistema.
   También puede consultar si hay productos antivirus instalados en el sistema.
   ```console
   PS C:\Users\Nicolas> Get-CimInstance -Namespace root/SecurityCenter2 -ClassName AntiSpywareProduct | Select-Object displayName
   ```
   ```
   displayName     
   -----------     
   Windows Defender
   ```
   ```console
   PS C:\Users\Nicolas> Get-CimInstance -Namespace root/SecurityCenter2 -ClassName AntiVirusProduct | Select-Object displayName
   ```
   ```
   displayName     
   -----------     
   Windows Defender
   ```

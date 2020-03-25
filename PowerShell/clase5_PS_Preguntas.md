## EJERCICIOS
1. ¿Cuál clase puede emplearse para consultar la dirección IP de un adaptador
   de red? ¿Posee dicha clase algún método para liberar un préstamo de
   dirección (lease) DHCP?
   
		Se puede emplear la clase ``win32_NetworkAdapterConfiguration`` y esta clase posee algunos metodos para liberar
		como ``RenewDHCPLease`` y ``RenewDHCPLeaseAll``
```PowerShell 
		Get-CimInstance win32_NetworkAdapterConfiguration | select IP
```
2. Despliegue una lista de parches empleando WMI (Microsoft se refiere a los
   parches con el nombre **quick-fix engineering**). Es diferente el listado al
   que produce el cmdlet ``Get-Hotfix``?
los listados son iguales, se puede evidenciar al guardar la salida de cada uno en un archivo diferente y usar diff para encontrar algun cambio y en este caso no presento ninguno
```PowerShell 
		Get-WmiObject win32_QuickFixEngineering
```
```PowerShell 
		Get-WmiObject win32_QuickfixEngineering |Out-File -FilePath wmiser
		Get-HotFix | Out-File -FilePath cmlser
		diff -ReferenceObject $(Get-Content wmiser) -DifferenceObject $(Get-Content cmlser)
```
3. Empleando WMI, muestre una lista de servicios, que incluya su status actual,
   su modalidad de inicio, y las cuentas que emplean para hacer login.
```PowerShell 
		Get-WmiObject -ClassName Win32_Service | select name, status,startmode,startname
```

4. Empleando cmdlets de CIM, liste todas las clases del namespace
   ``SecurityCenter2``, que tengan **product** como parte del nombre.
```PowerShell 
		Get-CimClass -Namespace root/SecurityCenter2 | where -Filter {$_.CimClassName -like "*product*"}
```
5. Empleando cmdlets de CIM, y los resultados del ejercicio anterior, muestre
   los nombres de las aplicaciones antispyware instaladas en el sistema.
   También puede consultar si hay productos antivirus instalados en el sistema.
``antispyware``
```PowerShell 
		Get-CimInstance -Namespace root/SecurityCenter2 -ClassName AntiSpywareProduct | select displayName
```
``antivirus``
```PowerShell 
		Get-CimInstance -Namespace root/SecurityCenter2 -ClassName AntiVirusProduct | select displayName
```

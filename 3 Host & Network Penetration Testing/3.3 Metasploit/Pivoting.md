1. `run autoroute -s 10.10.11.0/20` : only applicable to MSF
	-> then run `portfwd add -l LOCALPORT -p REMOTEPORT -r VICTIM-IP`
	
	-> In order to get a reverse shell on the machine on a pivoted network
		-> we have to use `bind_tcp` payload.
		-> `windows/meterpreter/bind_tcp`


1. you can use `chisel` | `SSH` too. 


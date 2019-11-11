#------------------------------------------------------------------------------------
#  Koldo Santisteban
#  Programado en Windows PowerShell 5.0, no probado en otras versiones
#------------------------------------------------------------------------------------


#------------------------------------------------------------------------------------
#  HoneyPot options
#  El hash debe ser calculado previamente.He utilizado el algoritmo MD5 porque tiene menos impacto en el rendimiento que SHA256, aunque sea menos seguro.
#  Para calcular el hash se puede utilizar el comando powershell siguiente: Get-FileHash filename -Algorithm MD5
#  i have use MD5 just for the performance. Hash have to be calcuted prior to running the script.
#  to get file hash in powershell, use this command: Get-FileHash filename -Algorithm MD5
#------------------------------------------------------------------------------------


$file1Hash = 'E405137DF121AE8ABF633619E898138A'
$file2Hash = '7DE0F8728C22CF4DF90D231F882F5928'
$file3Hash = '11D89D994F53923CA824DE8A248C2BAE'

$FilePath = 'C:\Users\ksantisteban\Desktop\HoneyPot\'

$File1Name = $FilePath + 'Chess-Teaching-Manual.pdf'
$File2Name = $FilePath + 'Don_Quijote_de_la_Mancha-Cervantes_Miguel.pdf'
$File3Name = $FilePath + 'filosofia-1-a-1.pdf'


#------------------------------------------------------------------------------------
#  Mail Server options
#  Check relay option in the mail server
#------------------------------------------------------------------------------------

$MailSender = "HoneyPot  <honeypot@batz.com>"
$MailRecipient = "ksantisteban@batz.com"
$SMTPServer = 'posta.batz.com'
$MailSubject = 'Honeypot warning!!'
$MailBody1 = '<p> One or more files hash have been modified , please check files directly for further details ' +' <p> Modificaction time: <strong> ' + $Today + '</strong>'
$MailBody2 = '<p> One or more files are missing from the honey pot directoy, please check them ' +' <p> Modificaction time: <strong> ' + $Today + '</strong>'
$Today = get-date;
#------------------------------------------------------------------------------------
#  Logging
#  Better to not save log in the honey pot folder 
#------------------------------------------------------------------------------------
$Logfile = "C:\Users\ksantisteban\Desktop\HoneyPot\$(gc env:computername).log"

Function LogWrite
{
   Param ([string]$logstring)
   
   
   $log = "$(Get-Date): " + $logstring
   Add-content $Logfile -value  $log
}


#------------------------------------------------------------------------------------
#  Starting...
#------------------------------------------------------------------------------------

# Check if files exist

LogWrite 'Starting...'

if ( !((Test-Path $File1Name)  -and (Test-Path $File2Name) -and (Test-Path $File3Name )))
{
  LogWrite 'Warning: One or more files are missing'
  Send-MailMessage -To $MailRecipient -From $MailSender -SmtpServer $SMTPServer -Subject $MailSubject -BodyAsHtml $MailBody2 -Encoding "UTF32" 
  LogWrite 'Mail sent and exit'
  Exit
}
else 
{
LogWrite 'Files found'
}


LogWrite 'Calculating hash'
$hashFromFile1 = Get-FileHash  $File1Name -Algorithm MD5
$hashFromFile2 = Get-FileHash  $File2Name -Algorithm MD5  
$hashFromFile3 = Get-FileHash  $File3Name -Algorithm MD5 
LogWrite 'Hash saved'



if (!(($hashFromFile1.Hash -eq $file1Hash) -and ($hashFromFile2.Hash -eq $file2Hash) -and ($hashFromFile3.Hash -eq $file3Hash )))
{
    LogWrite 'Warning, one or more files hash are not correct!!'
    Send-MailMessage -To $MailRecipient -From $MailSender -SmtpServer $SMTPServer -Subject $MailSubject -BodyAsHtml $MailBody1 -Encoding "UTF32" -Priority High
    LogWrite 'Mail sent'
    
}
else
{
 LogWrite 'Hash OK'
}


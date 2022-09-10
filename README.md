# implementing-DHCP-server  
ابتدا 
DHCP  server 
رو نصب میکنیم     
```
apt install isc-dhcp-server
```
![image](https://user-images.githubusercontent.com/88885103/189475762-69252235-ee0a-406e-b93f-e47bf06b9860.png)    

سپس 
> #authoritative;    
to    
> authoritative;

> subnet 10.10.10.0 netmask 255.255.255.248 {   
 range 10.10.10.2  10.10.10.6;      
 }    
 
 مک آدرس کلاینت مورد نظر با قالب

>ff:ff:ff:ff:ff:ff
 
 
 را در فایل مینویسیم
 
```
host archmachine {
hardware ethernet e0:91:53:31:af:ab;       
fixed-address 10.10.10.3;        
}
```  
بعد از آن اینترفیس که باید
DHCP server 
از آن شنود کند رو در فایل زیر مشخص میکنیم 
```
nano /etc/default/isc-dhcp-server
```
>INTERFACESv4=""
to 
>INTERFACESv4="enp0s3"

و بعد سرویس 
DHCP 
رو ری استارت میکنیم
   

   
   
```
systemctl restart isc-dhcp-server.service
```
  که با اخطار زیر مواجه میشویم 
  
  ![image](https://user-images.githubusercontent.com/88885103/189479628-2a89d0c7-7c75-4e1d-9c97-4fcca33b0de6.png)


با اجرای کامند 
‍‍
```
journalctl -xe
```
![image](https://user-images.githubusercontent.com/88885103/189479857-5d46b6e2-5544-4620-84e7-d86c63cfab1e.png)

که علت آن ست نکردن 
ip
 استاتیک است 
 
 
 برای برطرف شدن این مشکل 


```
nano /etc/network/interfaces
```
> allow-hotplug enp0s3   
iface enp0s3 inet dhcp 

to 

>auto enp0s3     
iface enp0s3 inet static    
address 10.10.10.1   
netmask 255.255.255.248    

با ریبوت کردن سیستم 
dhcp 
فعال شده 
برای برسی درستی عملکرد 
dhcp server 
میتوان یک ماشین کلاینت را به وسیله 
NAT Network
در یک شبکه قرار داد 
و در بخش 
Advanced > Promiscuous Mode 
به  
Allow All 
تغییر دهید 


# OPEN-SOURCE-EX-2

## NAME: GuruRaghav Ponjeevith V
## REGNO: 212223220027
## DEPT: IT

## AIM:
To configure SELinux and allow a web server running on a non-standard port **82** to serve content from `/var/www/html` without modifying or deleting any files in the directory.

## PROCEDURE:

### **STEP 1:**  
Check SELinux status  
```bash
sestatus
```

### **STEP 2:**  
Verify that Apache is running on port **82**  
```bash
sudo ss -tulnp | grep httpd
```

### **STEP 3:**  
Allow Apache to use non-standard ports  
```bash
sudo semanage port -a -t http_port_t -p tcp 82
```

(If port already exists, modify instead)  
```bash
sudo semanage port -m -t http_port_t -p tcp 82
```

### **STEP 4:**  
Set proper SELinux contexts for `/var/www/html`  
```bash
sudo restorecon -Rv /var/www/html
```

### **STEP 5:**  
Allow Apache to read/write content (if required)  
```bash
sudo setsebool -P httpd_read_user_content 1
sudo setsebool -P httpd_enable_homedirs 1
```

### **STEP 6:**  
Start and enable Apache  
```bash
sudo systemctl start httpd
sudo systemctl enable httpd
```

### **STEP 7:**  
Test access to the web server  
```
http://<server-ip>:82
```

### **STEP 8:**  
If access is denied, check AVC logs  
```bash
sudo sealert -a /var/log/audit/audit.log
```

---

## OUTPUT:


<img width="678" height="518" alt="image" src="https://github.com/user-attachments/assets/febb076b-2e8e-4537-b44c-ef88cdd86dab" />





<img width="954" height="533" alt="image" src="https://github.com/user-attachments/assets/08249ab3-c567-4d66-ae2f-422bc1d2bc86" />




<img width="691" height="531" alt="image" src="https://github.com/user-attachments/assets/7b3a5a28-de40-47a1-b387-a1ddb2e272b8" />





---

![Screenshot](https://github.com/user-attachments/assets/a62e3130-bbf3-45bb-bb06-6faaeee6b58f)

---


## RESULT:
The SELinux configuration was successfully adjusted, allowing the Apache web server to serve all content from `/var/www/html` on port **82**, making the web content fully accessible.


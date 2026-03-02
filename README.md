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

![Screenshot](https://github.com/user-attachments/assets/ae7a6c0f-a4b1-4bfb-a9fa-bba382b41dec)

---


![Screenshot](https://github.com/user-attachments/assets/6882e153-5b72-48f2-b848-1bbe912def53)

---

![Screenshot](https://github.com/user-attachments/assets/1b987265-78c4-4a61-8de1-e56513083dab)

---

![Screenshot](https://github.com/user-attachments/assets/889c08e8-2265-416a-bd8e-5acf43a4940b)

---

![Screenshot](https://github.com/user-attachments/assets/e3f063f1-c212-4d64-9d7d-379b835c7781)

---

![Screenshot](https://github.com/user-attachments/assets/78524493-bf9c-4aed-87bd-97be742d26ca)

---

![Screenshot](https://github.com/user-attachments/assets/cc5d1cb8-edb0-494f-b5dd-7d53ff7f9e60)

---

![Screenshot](https://github.com/user-attachments/assets/a62e3130-bbf3-45bb-bb06-6faaeee6b58f)

---


## RESULT:
The SELinux configuration was successfully adjusted, allowing the Apache web server to serve all content from `/var/www/html` on port **82**, making the web content fully accessible.


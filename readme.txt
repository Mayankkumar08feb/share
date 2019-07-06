---
 - hosts: all
   become: yes
   tasks:
     - name: install epel-relaese
       yum: name=epel-release update_cache=yes state=present

     - name: install httpd
       yum: name=httpd update_cache=yes state=latest

     - name: start httpd
       systemd:
        name: httpd
        state: started

     - name: copy html code to document dir
       copy:
        src: /home/mayank/test.html
        dest: /var/www/html

     - name: copy SSL certificate to host machine
       copy:
        src: /home/mayank/ssl/29-June/

     - name: Backup original ssl.conf file
       command: mv /etc/httpd/conf.d/ssl.conf /etc/httpd/conf.d/ssl.conf.org

     - name: Need to Edit the ssl.conf file
       copy:
        src: /home/mayank/ssl.conf
        dest: /etc/httpd/conf.d/

     - name: restarting httpd services
       systemd:
        name: httpd
        state: restarted

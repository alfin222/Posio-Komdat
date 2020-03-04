<h1 align="center"><img src="https://github.com/alfin222/Posio-Komdat/blob/master/posio_insert_name.png"></h1>

[Sekilas Tentang](#sekilas-tentang) | [Instalasi](#instalasi) | [Cara Bermain](#cara-bermain) | [Pembahasan](#pembahasan) | [Referensi](#referensi)
:---:|:---:|:---:|:---:|:---:



# Sekilas Tentang
[`^ kembali ke atas ^`](#)

**__Posio__** adalah sebuah *multiplayer game* berbasis web. Repository game ini dibuat oleh kontributor bernama [abrenaut](https://github.com/abrenaut/). Lokasi repository terdapat di https://github.com/abrenaut/posio.




# Instalasi
[`^ kembali ke atas ^`](#)

#### Kebutuhan Sistem :
- VM Linux Ubuntu 18.04
- Python-pip
- Flask
- Flask-socketio
- Eventlet
- nginx

#### Proses Instalasi :
1. Login kedalam server pada virtual server menggunakan *username* `student` dan *password* `student`

2. Pinda direktori ke `$HOME` lalu unduh **posio**
    ```
    cd && git clone https://github.com/abrenaut/posio/
    ```

3. Unduh kebutuhan sistem dan *install*. 
    ```
    sudo apt install python3 python3-pip
    cd ~/posio && python3 -m pip install -r requirements.txt
    ```

4. Buat daemon system service untuk posio
    ```
    sudo tee /etc/systemd/system/posio.service << !
    [Unit]
    Description=posio
    After=network.target

    [Service]
    User=student
    WorkingDirectory=/home/student/posio
    ExecStart=/usr/bin/python3 /home/student/posio/run.py
    Restart=always

    [Install]
    WantedBy=multi-user.target
    !
    # enable posio
    sudo systemctl enable posio

    # start posio
    sudo systemctl start posio
    ```

5. *Disable* dan *stop* Apache2
    ```
    sudo systemctl stop apache2
    sudo systemctl disable apache2
    ```

6. *Install* nginx pada virtual server
    ```
    sudo apt install nginx-core
    ```

7. Lakukan konfigurasi nginx untuk posio
    ```
    sudo tee /etc/nginx/sites-available/default << !
    server {
        listen 80;

        location / {
            include proxy_params;
           proxy_pass http://127.0.0.1:5000;
       }

        location /static {
            alias /home/student/posio/app/static;
            expires 30d;
        }

        location /socket.io {
            include proxy_params;
            proxy_http_version 1.1;
            proxy_buffering off;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection "Upgrade";
           proxy_pass http://127.0.0.1:5000/socket.io;
       }
    }
    !
    ```

8. *Restart* nginx
    ```
    sudo systemctl restart nginx
    ```

9. Buka *link* `http://localhost/`



## Cara Bermain
[`^ kembali ke atas ^`](#)

Cara memainkan **Posio** cukup dengan menekan tombol `left-click` untuk memilih lokasi tebakan.
![](https://github.com/alfin222/Posio-Komdat/blob/master/Select_Location.png)

Lalu tunggu sesaat untuk melihat hasil seberapa akurat hasil tebakannya.
![](https://github.com/alfin222/Posio-Komdat/blob/master/Result.png)

## Pembahasan
[`^ kembali ke atas ^`](#)

Posio ditulis dalam bahasa pemrograman Python. Permainan ini cukup mudah dimainkan tanpa perlu <i>tutorial</i> karena hanya cukup dengan menebak lokasi suatu kota di seluruh dunia.

## Referensi
[`^ kembali ke atas ^`](#)

1. [Posio](https://github.com/abrenaut/posio) - abrenaut Github
2. [Komunikasi Data dan Jaringan Komputer Praktikum 01](https://github.com/auriza/komdat-lab/blob/master/p01.md) - Auriza Github
3. [README](https://gist.github.com/kiror0/35987420a27e54f0e432e32d79215aa1) - kiror0


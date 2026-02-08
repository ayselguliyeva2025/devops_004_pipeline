# AWS

### DevOps - Local
![DevOps Local.jpg](src/img/DevOps%20Local.jpg)

### DevOps - Cloud
![DevOps Cloud.jpg](src/img/DevOps%20Cloud.jpg)

####  AWS kaynaklarının ARN standart adres yolları

```
arn:aws:resource-groups:us-east-1:1111111:group/Arge_Departmani_Kaynaklari

arn:aws:account        :          :1111111:account

arn:aws:iam:           :1111111:user/AbdussamedBey
```

AWS Management Console Giriş Adresleri

AWS_ID_NUMARAN : 1111 2222 3333

https://111122223333.signin.aws.amazon.com/console

---
AWS ID yerine oluşturduğun bir FIRMA ADI ile giriş adresi

FIRMA ADI : MurlindaSoft

https://MurlindaSoft.signin.aws.amazon.com/console

---
Parola kuralımız

1 büyük harf

1 küçük harf

1 sayı

1 simge

```
Adana_01
Ankara_06
Antalya_07
Izmir_35
Istanbul_34
Trabzon_61
Agri_004
```


---
Amazon CLI - Setup

https://aws.amazon.com/cli/

---
AWS CLI Command Reference

https://docs.aws.amazon.com/cli/latest/

---

Termimalden AWS'ye erişmek için Access key oluşturacağız.

Kullanıcı adı ->  Security credentials -> Create access key

Adres burası
https://us-east-1.console.aws.amazon.com/iam/home?region=us-east-1#security_credential

---

Access key
AAAAAAAAAAA

Secret access key
BBBBBBBBBBBB

---
Terminali aç.

```
aws --version
```
```
aws configure
```
```
AWS Access Key ID [****************]: AAAAAAAAAAAAA
AWS Secret Access Key [****************]: BBBBBBBBB
Default region name [us-east-1]: us-east-1
Default output format [None]: json
```

```
aws ec2 describe-instances
```

---

PuTTY
https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html



==============================





===== Makine 1 : My Jenkins Master =====

Public IP numaralarını Elastic IP ile mutlaka sabitliyoruz!!!

https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#Addresses:





MacOS üzerinden terminalden bağlantı için instances makineyi seç.
Connect düğmesine bas.
SSH client sekmesini aç.

Terminalden  My-Ubuntu-Key.pem konumuna   git.
```
chmod 400 "My-Ubuntu-Key.pem"
```

```
ssh -i "My-Ubuntu-Key.pem" ubuntu@ec2-PUBLIC_IP.compute-1.amazonaws.com
```





Windows üzerinden MobaXterm ile SSH bağlantısını kurduk.



Makineyi güncelliyorum.

```
sudo apt update

sudo apt upgrade -y
```

İç IP yerine makineye bir isim veriyoruz.

```
sudo nano /etc/hostname
```

Makinememizin adı: My-Jenkins-Master

Ctrl + X'e bas.
Onaylamak için Y tuşuna bas.
En sonda da Enter'a bas.

Makineyi yeniden başlatacağız.

```
sudo reboot
```
Ya da

```
sudo init 6
```




===========================

EC2 -> Security Groups -> Inbound rules  -> Edit inbound rules
8080 portunu erişime açıyoruz.

======== Java'yı kuracağız. ===================

Terminale sadece "java" yaz ve enter tuşuna bas. Açılan komutlardan 21. sürümü al.
```
sudo apt install openjdk-21-jre-headless -y

java --version
```

======== Jenkins kuracağız. ===================

https://www.jenkins.io/doc/book/installing/linux/

```
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
https://pkg.jenkins.io/debian/jenkins.io-2026.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
https://pkg.jenkins.io/debian binary/ | sudo tee \
/etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update
sudo apt install jenkins   -y
```

```
sudo systemctl enable jenkins

sudo systemctl start jenkins

sudo systemctl status jenkins
```



http://PUBLIC_IP:8080/login

Bu adresin içindeki yazıyı terminalde göster.
sudo cat  /var/lib/jenkins/secrets/initialAdminPassword


Varsayılan kurulumları yap. O arada Git aracı da makineye otomatik olarak kurulacak.


http://PUBLIC_IP:8080/manage/pluginManager/available

Pipeline: Stage View bu eklentiyi kur.











===== Makine 2 : My Jenkins Agent =====


Public IP numaralarını Elastic IP ile mutlaka sabitliyoruz!!!

https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#Addresses:





MacOS üzerinden terminalden bağlantı için instances makineyi seç.
Connect düğmesine bas.
SSH client sekmesini aç.

Terminalden  My-Ubuntu-Key.pem konumuna   git.
```
chmod 400 "My-Ubuntu-Key.pem"

ssh -i "My-Ubuntu-Key.pem" ubuntu@ec2-PUBLIC_IP.compute-1.amazonaws.com
```



Windows üzerinden MobaXterm ile SSH bağlantısını kurduk.



Makineyi güncelliyorum.
```
sudo apt update

sudo apt upgrade -y
```

İç IP yerine makineye bir isim veriyoruz.
```
sudo nano /etc/hostname
```

Makinememizin adı: My-Jenkins-Agent

Ctrl + X'e bas.
Onaylamak için Y tuşuna bas.
En sonda da Enter'a bas.

Makineyi yeniden başlatacağız.

```
sudo reboot
```


======== Java'yı kuracağız. ===================

Terminale sadece "java" yaz ve enter tuşuna bas. Açılan komutlardan 21. sürümü al.

```
sudo apt install openjdk-21-jre-headless -y

java --version
```



======== Docker'ı kuracağız. ===================

Terminale sadece "docker" yaz ve enter tuşuna bas.

Açılan komutlardan yararlanacağız.
```
sudo apt  install docker.io  -y
```

Grup adını ekliyoruz.
```
sudo groupadd docker
```

```
sudo usermod -a -G docker $USER
```
veya

```
sudo usermod -aG docker $USER
```

Yeniden başlat.
```
sudo reboot
```
================================

Makineleri birbirine tanıtacağız.

=== My Jenkins Agent için ===
```
sudo nano /etc/ssh/sshd_config
```

Şu 2 satırın önündeki # işaretini kaldırıyorum.

PubkeyAuthentication yes

AuthorizedKeysFile      .ssh/authorized_keys .ssh/authorized_keys2


Ctrl + X'e bas.
Onaylamak için Y tuşuna bas.
En sonda da Enter'a bas.


=== My Jenkins Master için ===
```
sudo nano /etc/ssh/sshd_config
```

### Authentication:
Authentication kısmına gel.
Aşağıdaki şu iki satırın önündeki açıklama işaretini # kaldır.


PubkeyAuthentication yes

AuthorizedKeysFile      .ssh/authorized_keys .ssh/authorized_keys2


Ctrl + X'e bas.
Onaylamak için Y tuşuna bas.
En sonda da Enter'a bas.


=== My Jenkins Master için ===

```
sudo service ssh reload
```
veya
```
sudo systemctl reload ssh
```


=== My Jenkins Master için ===

```
sudo service ssh reload
```
veya
```
sudo systemctl reload ssh
```


=== My Jenkins Master için ============================

Konumumuz  /home/ubuntu   olacak.

pwd konumu bize gösterir.

```
pwd

cd /home/ubuntu

ssh-keygen
```


Master makinenin takip edilebilmesi için bir şifre anahtar oluşturuyorum.

```
cd /home/ubuntu/.ssh/

ll

sudo cat  id_ed25519.pub
```


İçindeki böyle yazan satırı alıp kopyalayın.

```
ssh-ed25519 AAAAAAAAAAAAAAAAA ubuntu@My-Jenkins-Master
```


=== My Jenkins Agent için ============================
```
cd /home/ubuntu/.ssh/

ll

sudo cat authorized_keys
```

Bu dosyanın için aç. Agent Takip eden taraftır.

```
ssh-rsa BBBBBBBBBBBBBBBBBB MyAWSKeyPair
```

```
sudo nano authorized_keys
```


Master'dan aldığın şu satırı en alta yapıştır.
```
ssh-ed25519 AAAAAAAAAAAAAAAAA ubuntu@My-Jenkins-Master

```

Master ve Agent makinelerini yeniden başlat.
```
sudo reboot
```

======================================
Master makinedeki Jenkins'i aç.

http://PUBLIC_ID:8080/computer/(built-in)/configure

Jenkins'i aç. Nodes kısmına gel.

Built-In Node makinesinin içine gir.

Nodes -> Built-In Node -> Configure

Number of executors kısmını SIFIR 0 yap.

Agent makineyi Jenkins'e eklemek için

Nodes -> New node

http://PUBLIC_IP:8080/computer/new

Ona "My-Jenkins-Agent" diye bir isim verdik. Permanent Agent olduğunu seçtik.

Jenkins'te Agent'ı eklerken Agent'ın kendi private özel yani iç IP'sini alacaksın.


====== Master Makinedeki bu anahtarı okuyup kopyalayın ve Jenkins'e gelin. Credentials için ====

```
cd  /home/ubuntu/.ssh/
sudo cat id_ed25519
```

```
-----BEGIN OPENSSH PRIVATE KEY-----
CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC
-----END OPENSSH PRIVATE KEY-----
```




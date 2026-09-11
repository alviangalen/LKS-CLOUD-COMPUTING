# Setting VPC, Subnets, Route Tables, Internet Gateways, Instances EC2 dan Docker di AWS

## 1. Buat VPC terlebih dahulu
Di VPC dashboard klik create VPC di pojok kanan atas.

![Create VPC](images/create-vpc.png)

Di dalam VPC settings, Resources to create pilih yang `VPC only`, lalu buat nama di bagian name tag agar mudah misal `VPC-Galen`. Lalu pilih bagian Ipv4 CIDR manual input dan ketik Ipv4 terserah, misal disini saya `192.168.0.0/20`. Setelah itu biarkan sisanya default lalu klik create.

![VPC Settings](images/vpc-settings.png)

Nanti akan tampil seperti ini.

![VPC Success](images/vpc-success.png)

Dan dibagian Your VPCs ada VPC yang kalian buat.

![Your VPCs](images/your-vpcs.png)

## 2. Buat Internet Gateway
Dibagian Internet gateways pilih Create internet gateway di pojok kanan atas

![Create IGW](images/create-igw.png)

Dibagian Nametag masukkan nama gateway, misalnya `gateway-galen`. Setelah itu klik create internet gateway.

![IGW Settings](images/igw-settings.png)

Kalau sudah, tampilannya akan seperti ini.

![IGW Success](images/igw-success.png)

Klik kanan pada gateway yang tadi kalian buat, lalu klik **Attach to VPC**.

![Attach IGW](images/attach-igw.png)

Pilih VPC milik kalian lalu klik Attach internet gateway di pojok kanan bawah.

![Attach to VPC](images/attach-to-vpc.png)

Tampilannya setelah itu akan seperti ini.

![IGW Attached](images/igw-attached.png)

## 3. Buat Route tables
Dibagian Route tables, klik Create route table di pojok kanan atas.

![Create Route Table](images/create-route-table.png)

Dibagian name, beri nama ke rute public kalian, misalnya disini `rute-galen-public`. Lalu kalian pilih VPC yang kalian buat. Lalu klik create route table di bagian pojok kanan bawah.

![Route Table Settings](images/route-table-settings.png)

Tampilannya akan seperti ini, klik Edit Routes di bagian kanan.

![Edit Routes](images/edit-routes.png)

Klik Add route.

![Add Route](images/add-route.png)

Pilih yang `0.0.0.0/0` dan Internet Gateway dan dibawahnya pilih gateway yang sudah kalian buat. Lalu klik Save changes.

![Save Routes](images/save-routes.png)

Tampilannya akan seperti ini, jadi akan ada 2 rute.

![Routes Success](images/routes-success.png)

## 4. Buat Subnets
Disini kita akan membuat Subnet public A, Subnet Public B, dan Subnet private C.

Di bagian subnets, klik Create subnet di pojok kanan atas.

![Create Subnet](images/create-subnet.png)

Dibagian VPC ID pilih VPC yang kalian buat.

![Subnet VPC](images/subnet-vpc.png)

Di subnet 1 buat subnet name nya bebas, misal `subnet-public-galen-A`, lalu Availability zone nya pilih yang ada huruf **a** nya, misal `us-east-2a` dan di bagian Ipv4 subnet CIDR block isi dengan Ipv4 bebas, misal `192.168.0.0/24`. Lalu klik add new subnet di bagian bawah untuk membuat subnet 2.

![Subnet A](images/subnet-a.png)

Di subnet 2 juga buat subnet name nya bebas, misal `subnet-public-galen-B`, lalu Availability zone nya pilih yang ada huruf **b** nya, misal `us-east-2b` dan di bagian Ipv4 subnet CIDR block isi dengan Ipv4 bebas, tapi jangan sama dengan subnet 1, misal `192.168.1.0/24`. Lalu klik add new subnet lagi dibawah.

![Subnet B](images/subnet-b.png)

Di subnet 3 juga buat subnet name nya bebas, misal `subnet-private-galen-C`, lalu Availability zone nya pilih yang ada huruf **c** nya, misal `us-east-2c` dan di bagian Ipv4 subnet CIDR block isi dengan Ipv4 bebas, tapi jangan sama dengan subnet 1 dan 2 misal `192.168.3.0/24`. Lalu klik Create subnet di pojok kanan bawah.

![Subnet C](images/subnet-c.png)

Tampilannya akan seperti ini.

![Subnet Success](images/subnet-success.png)

Di bagian ini, klik subnet public yang A, lalu klik tab Route Table, lalu klik Edit route table association.

![Edit Route Table Assoc](images/edit-route-table-assoc.png)

Pilih route yang tadi dibuat. Lakukan hal yang sama ke Public B dan jangan lakukan ke Private C.

![Route Assoc Save](images/route-assoc-save.png)

Jika sudah, di bagian Resource map VPC kalian akan tampil seperti ini, dimana Subnet public A dan Subnet public B → rute publik → gateway. Sementara yang private tidak. Setelah itu klik teks VPC yang ada di pojok kanan paling atas.

![Resource Map](images/resource-map.png)

Kita akan membuat EC2 Instances, klik Launch EC2 Instances.

![Launch EC2](images/launch-ec2.png)

Beri nama pada EC2 nya, misal disini `server-galen`, dan pilih Amazon Linux untuk OS nya.

![EC2 Name and OS](images/ec2-name-os.png)

Pilih instance type nya `t3.micro`.

![EC2 Instance Type](images/ec2-instance-type.png)

Di bagian Key Pair, klik Create key pair, beri nama key nya dan pilih tipe sesuai kebutuhan. Lalu klik Create key Pair.

![Create Key Pair](images/create-key-pair.png)

Klik save.

![Save Key Pair](images/save-key-pair.png)

Di bagian Network settings klik Edit.

![Network Settings](images/network-settings.png)

Tampilannya akan berubah seperti ini, Pilih VPC yang kalian buat, lalu pilih subnet yang Public A, lalu auto-assign public IP enable. Biarkan sisanya default, lalu klik Launch Instance di pojok kanan bawah.

![Launch Instance](images/launch-instance.png)

Jika sudah akan seperti ini, tinggal klik di bagian yang hijau.

![Instance Success](images/instance-success.png)

Disini kalian liat apakah Public Ipv4 itu ada? Jika ada bisa lanjut klik Connect di bagian atas.

![Instance Summary](images/instance-summary.png)

Pilih Connect using a Public IP. Lalu klik Connect.

![Connect Instance](images/connect-instance.png)

Dan akan terbuka tab baru, tampilannya seperti ini.

![Terminal Access](images/terminal-access.png)

Ini berarti sudah berhasil membuat EC2 nya.
Kalian tinggal masukkan command untuk uji coba install docker:

```bash
sudo yum install docker -y
sudo systemctl enable docker
sudo systemctl start docker
sudo docker pull hello-world:latest
sudo docker images
sudo docker run --name hello hello-world:latest
sudo docker run hello-world:latest
```

Jika tidak ada pesan error atau gagal berarti sudah berhasil. Hasil akhirnya kira kira seperti ini.

![Docker Success](images/docker-success.png)
<a href="https://github.com/wiseupdata/wiseupdata">
  <img align="left" alt="Wise Up Data's Instagram" width="22px" src="https://raw.githubusercontent.com/wiseupdata/wiseupdata/main/assets/instagram.png" />   
</a> 
<a href="https://github.com/wiseupdata/wiseupdata">
  <img align="left" alt="wise Up Data's Discord" width="22px" src="https://raw.githubusercontent.com/wiseupdata/wiseupdata/main/assets/discord.png" />
</a>
<a href="https://github.com/wiseupdata/wiseupdata">
  <img align="left" alt="wise Up Data | Twitter" width="22px" src="https://raw.githubusercontent.com/wiseupdata/wiseupdata/main/assets/twitter.png" />
</a>
<a href="https://github.com/wiseupdata/wiseupdata">
  <img align="left" alt="wise Up Data's LinkedIN" width="22px" src="https://raw.githubusercontent.com/wiseupdata/wiseupdata/main/assets/linkedin.png" />
</a>

![visitors](https://visitor-badge.glitch.me/badge?page_id=wiseupdata.shell-linux&left_color=green&right_color=black)
![license](https://img.shields.io/github/license/wiseupdata/shell-linux)

---

<a name="readme-top"></a>

<a href="https://github.com/wiseupdata/wiseupdata">
<img align="left" alt="img" src="https://raw.githubusercontent.com/wiseupdata/shell-linux/master/assets/linux.png" width="300" />
</a>

<h1>
Shell Linux
</h1>
Util Commands | Ubuntu, Debian, Wsl2
<br>
<br>
<br>
<br>
<br>

<details>
<summary>
    Install GUI RDP, remote desktop in one VM ⏬
</summary>

<br>
<details>
<summary>
    Information 🔎
</summary>

> X2Go is a remote desktop software solution that enables you to access graphical desktops of Linux servers over a low bandwidth connection. It is a fast and lightweight alternative to other remote desktop protocols, such as VNC or RDP.
> 
> X2Go is a good choice for setting up RDP on your Ubuntu VM running in Cloud for several reasons:
> 
> - It is open-source and free.
> - It provides a secure remote desktop solution by using the SSH protocol for encryption.
> - It supports various desktop environments, such as Xfce, Mate, LXDE, KDE, and more.
> - It is easy to install and configure on Ubuntu.
> - It has a low memory footprint, making it suitable for low-spec VMs.
> 
> Overall, X2Go is a reliable and efficient option for setting up RDP on your Ubuntu VM in Cloud.

</details>


<details>
<summary>
  Installation 🧑‍💻
</summary>


### Let's update the Ubuntu system

```
sudo apt update
sudo apt upgrade
```

<img align="center" alt="gif" src="assets/update.gif" width="700" />
</img>
<br>

### Install the X2Go server

```
sudo apt install software-properties-common
sudo add-apt-repository ppa:x2go/stable
sudo apt update
sudo apt install x2goserver x2goserver-xsession
```
<a href="https://github.com/wiseupdata/wiseupdata">
<img align="center" alt="gif" src="assets/x2go.gif" width="700" />
</img>
</a>
<br>

### Install the xfce4 server

```
sudo apt install xfce4
```
<a href="https://github.com/wiseupdata/wiseupdata">
<img align="center" alt="gif" src="assets/xfce4.gif" width="700" /></img>
</a>
<br>


### You can also install the gnome

```
sudo apt-get install ubuntu-gnome-desktop
```



### Start the X2Go server

```
sudo systemctl start x2goserver
sudo service x2goserver status
```

<a href="https://github.com/wiseupdata/wiseupdata">
<img align="center" alt="gif" src="assets/x2goserver.gif" width="700" />
</a>

<br>
<br>

</details>

<details>
<summary>
  Extra - Install the X2Go Client in the Windows 11 ♨️
</summary>

### Let's download the client ✈️

[X2GoClient](https://wiki.x2go.org/doku.php/doc:installation:x2goclient)

<a href="https://github.com/wiseupdata/wiseupdata">
<img align="center" alt="gif" src="assets/x2goclientdownload.gif" width="700" />
</img>
</a>
<br>

### Install the software

<img align="center" alt="gif" src="assets/x2goclientinstall.gif" width="700" />
</img>
<br>

</details>

</details>

😍 Shell commands 

<details>
<summary>
  Mount OCI Bucket 🧑‍💻
</summary>


### Let's install the s3fs 🛠️


> Requirements
> - jq  `sudo apt install jq`
> - [oci cli](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/cliinstall.htm) 

```
sudo apt-get update

sudo apt install s3fs -y 
```

<a href="https://github.com/wiseupdata/wiseupdata">
<img align="center" alt="gif" src="assets/install.gif" width="700" />
</a>

<br>
<br>

### Generate the credentials in the Oracle Painel 🔐

> Profile [icon] > My profile > Customer secret keys

<a href="https://github.com/wiseupdata/wiseupdata">
<img align="center" alt="gif" src="assets/secret.gif" width="700" />
<a/>

### Save the credentials💲

```
echo $ACCESS_KEY_OCI_ID:$SECRET_ACCESS_OCI_KEY > $HOME/.passwd-s3fs

chmod 600 ${HOME}/.passwd-s3fs
```
Replace with the values you got from the web browser.


You can check the result with 
```
cat $HOME/.passwd-s3fs
```

### let's create the mount 🦄 🗂️

```
mountPath=/mnt/oci
mkdir -p $mountPath

nameSpace=$(oci os ns get | jq -r '.data') # you need the oci cli
bucketName="dl-wiseupdata-data-dev"
region="us-ashburn-1"
url=https://$nameSpace.compat.objectstorage.$region.oraclecloud.com

sudo s3fs $bucketName $mountPath \
-o passwd_file=$HOME/.passwd-s3fs \
-o url=$url \
-o endpoint=$region \
-o nomultipart \
-o use_path_request_style \
-ouid=1000,gid=1000,allow_other,mp_umask=002

ls -la $mountPath && echo $mountPath
```

### Test the connection 🔌

```
cd $mountPath
ll
cd tmp
touch works!
rm  works!

```

### Result 🏅 🏆

<a href="https://github.com/wiseupdata/wiseupdata">
<img align="center" alt="gif" src="assets/mount.gif" width="700" />
<a/>

### Unmount 🌧️
```
sudo umount -l $mountPath

#not mounted anymore
ls -la $mountPath && echo $mountPath

# alternative 
sudo fusermount -u $mountPath
```

# Clean the resources 🧹
```
sudo rm -Rf $mountPath
```


### Troubleshoot in the creating 😕 🔦

```
#check the variables values:

echo $nameSpace
echo $bucketName
echo $region
echo $url

ls -la $mountPath
cat $HOME/.passwd-s3fs
```

### Troubleshoot unmount 😕 📉

```
lsof /home/wiseupdata/.oci_mount
sudo kill 42277
```


</details>

<br>
<br>

# References 🌍 🗄️

1. [Wise Up Data](https://github.com/wiseupdata)
1. [RDP alternative](https://www.youtube.com/watch?v=Moscv2moML8)
1. [Emojis](https://github.com/ikatyang/emoji-cheat-sheet/blob/master/README.md)

<br><br>
---

#### Maintainer 🤗 👨‍💻

Sivio Liborio

📧 silvio.liborio@wiseupdata.com

<a href="https://www.linkedin.com/in/silvio-de-melo-liborio">silvio-de-melo-liborio <img align="left" alt="LinkedIN" width="18px" src="https://raw.githubusercontent.com/wiseupdata/wsl-latest/main/assets/linkedin.svg" />
</a>

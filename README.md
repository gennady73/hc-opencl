# hc-opencl
Exerimental:    
A standalone runtime OpenCL library packages from Intel openAPI repository.    
Built on Red Hat UBI 8


# Setup
* ## Get hashcat application 
* ###  Download binaries
    ```bash
    wget  https://hashcat.net/files/hashcat-6.2.6.7z
    ```
  ### Unpack and copy into docker/hashcat directory, Note that 7zip archiver must be installed on system.
  ### As alternative, use yum or dnf package manager
  ```bash    
  yum -y install hashcat
  ```
  or
  ```bash
  dnf -y install hashcat
  ```
* ## Build image based on Inter oneAPI with OpenCL library
    ```bash
    podman build -f ./oneapi-opencl-ubi8.x86_64/Dockerfile -t docker.io/gennady73/hashcat-web:0.0.1 
    ```

* ## Build image based on hashcat
    ```bash
    podman build -f ./hashcat/Dockerfile -t docker.io/gennady73/hashcat:0.0.1 
    ```

* ## Build image based on hashcat with basic Python server(port 8080)
    ```bash
    podman build -f ./hashcat-web/Dockerfile -t docker.io/gennady73/hashcat-web:0.0.1 
    ```

* ## As alternative use images from DockerHub (latest) in order to build your own images on top of it:
    ```bash
    podman pull docker.io/gennady73/oneapi-opencl-ubi8.x86_64
    podman pull docker.io/gennady73/hashcat
    podman pull docker.io/gennady73/hashcat-web
    ```
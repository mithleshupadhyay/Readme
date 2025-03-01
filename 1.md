# FastSurfer Installation and Usage Guide (Using Docker)

## Introduction
FastSurfer is a deep-learning-based neuroimaging pipeline for rapid and accurate brain MRI analysis. This guide provides step-by-step instructions to install and run FastSurfer using Docker on a system without a GPU.

---

## **1. Install Docker**
### **Step 1: Update the System**
Open a terminal and run the following command to update your system:
```bash
sudo apt update
```
### **Step 2: Install Docker**
```bash
sudo apt install -y docker.io
```
### **Step 3: Enable and Start Docker**
```bash
sudo systemctl enable --now docker
```
### **Step 4: Allow User to Run Docker Without sudo**
```bash
sudo usermod -aG docker $USER
newgrp docker
```
### **Step 5: Verify Docker Installation**
```bash
docker --version
```
---

## **2. Pull the FastSurfer Docker Image**

Download the latest FastSurfer image by running:
```bash
docker pull deepmi/fastsurfer:latest
```

---

## **3. Get a FreeSurfer License**
FastSurfer requires a FreeSurfer license. Follow these steps to obtain and set up the license:

### **Step 1: Create a License Directory**
```bash
mkdir -p /home/$USER/freesurfer_license
```

### **Step 2: Register and Download the License**
1. Visit the [FreeSurfer Registration Page](https://surfer.nmr.mgh.harvard.edu/registration.html).
2. Register for a FreeSurfer license.
3. Download the `license.txt` file.

### **Step 3: Move the License File to the Correct Location**
Replace `/path/to/downloaded/license.txt` with the actual path where your file was downloaded (usually `~/Downloads/license.txt`).
```bash
mv /path/to/downloaded/license.txt /home/$USER/freesurfer_license/license.txt
```

### **Step 4: Verify the License File**
```bash
ls -l /home/$USER/freesurfer_license/
```
You should see `license.txt` listed in the output.

---

## **4. Run FastSurfer**
### **Step 1: Prepare Your Data and Output Directories**
Ensure your dataset and output folder exist. Suppose your dataset is located at:
```bash
/home/rudra/Desktop/old/core/
```
and the output folder is:
```bash
/home/rudra/Desktop/old/core/Output/
```

### **Step 2: Run FastSurfer Using Docker (CPU Mode)**
Run the following command, replacing the dataset filename accordingly:
```bash
docker run --rm \
    --user root \
    -v /home/rudra/Desktop/old/core:/data \
    -v /home/rudra/Desktop/old/core/Output:/output \
    -v /home/rudra/freesurfer_license:/fs_license \
    deepmi/fastsurfer:latest \
    --allow_root \
    --fs_license /fs_license/license.txt \
    --t1 /data/ADNI_023_S_1247_MR_MPR__GradWarp__B1_Correction__N3__Scaled_Br_20070412172006512_S25741_I48857.nii \
    --sid subjectX --sd /output \
    --device cpu
```

### **Step 3: Restart Docker Desktop (if needed)**
If using Docker Desktop, restart it:
```bash
systemctl --user restart docker-desktop
```

---

## **5. Troubleshooting**
### **Issue 1: "Cannot connect to the Docker daemon"**
Run the following command to check if Docker is running:
```bash
systemctl status docker
```
If Docker is not running, start it:
```bash
sudo systemctl start docker
```
If needed, enable it at startup:
```bash
sudo systemctl enable docker
```

### **Issue 2: "Permission denied" when running Docker**
Ensure your user is added to the Docker group:
```bash
sudo usermod -aG docker $USER
newgrp docker
```
Then restart your system.

### **Issue 3: "No such file or directory" for license.txt**
Check that the file exists in the correct directory:
```bash
ls -l /home/$USER/freesurfer_license/license.txt
```
If not, move it there again.

---

## **6. Conclusion**
You have successfully installed and run FastSurfer using Docker on a CPU-only system. If you have a GPU, modify the Docker command by replacing `--device cpu` with `--gpus all`. FastSurfer will process the MRI data and save the results in the specified output folder.

For further details, refer to the [FastSurfer GitHub Repository](https://github.com/Deep-MI/FastSurfer).

Happy processing!


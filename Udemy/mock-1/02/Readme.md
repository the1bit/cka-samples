# Mock Exam 1 - 02

## Task: Install cri-docker on node01

This question needs to be solved on node node01. To access the node useing SSH, use the credentials below:

```sh
username: bob
password: caleston123
```

As an administrator, you need to prepare node01 to install kubernetes. One of the steps is installing a container runtime. Install the cri-docker_0.3.16.3-0.debian.deb package located in /root and ensure that the cri-docker service is running and enabled to start on boot.

Requirements:

- Is the package installed successfully on node01?
- Is the service running?
- Is the service enabled?

## Solution Steps

1. **SSH into node01:**

   ```sh
    ssh bob@node01
   ```

2. Enter to root mode:

   ```sh
   sudo -i

   cd /root
   ```

3. **Install cri-docker:**

   ```sh
    dpkg -i /root/cri-docker_0.3.16.3-0.debian.deb
   ```

4. Fix any dependency issues if they arise:

   ```sh
   apt-get install -f -y
   ```

5. Start and enable the cri-docker service:

   ```sh
   systemctl daemon-reexec # optional, but recommended
   systemctl start cri-docker
   systemctl enable cri-docker
   ```

6. Verify the installation:

   ```sh
   systemctl status cri-docker
   ```

   You should see output indicating that the service is active (running).
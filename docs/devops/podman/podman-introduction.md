# Podman Introduction

Podman (the POD MANager) is an open-source, daemonless, and rootless tool for developing, managing, and running OCI (
Open Container Initiative) containers. It is often described as a more secure and lightweight alternative to Docker.

---

## 1. Why Podman Instead of Docker?

While Podman shares a similar CLI with Docker, its architecture is fundamentally different:

- **Daemonless:** Unlike Docker, Podman does not require a background service (the Docker Daemon) to run. This
  eliminates a single point of failure.
- **Rootless by Default:** Podman allows users to run containers without root privileges, significantly improving system
  security.
- **Pods:** Podman can group containers into "pods" (similar to Kubernetes), allowing them to share network resources.

---

## 2. Installation on macOS

On macOS, Podman runs inside a lightweight virtual machine. You can easily install it using Homebrew.

**Step 1: Install Podman via Brew**

```bash
brew install podman
```

**Step 2: Initialize the Podman Machine**
Since macOS doesn't run Linux containers natively, you need to set up the Podman VM:

```bash
podman machine init
```

**Step 3: Start the Machine**

```bash
podman machine start
```

**Step 4: Verify Installation**

```bash
podman info
```

---

## 3. Basic Commands (The "Alias" Trick)

Podman's CLI is intentionally compatible with Docker. In most cases, you can simply replace the word "docker" with "
podman".

**Running a Container:**

```bash
podman run --name web-server -dt -p 8080:80 nginx
```

**Listing Running Containers:**

```bash
podman ps
```

**Stopping and Removing:**

```bash
podman stop web-server
podman rm web-server
```

**Pro Tip: The Alias**
If you want to keep using the "docker" command but run Podman under the hood, add this to your `.zshrc` or
`.bash_profile`:

```text
alias docker=podman
```

---

## 4. Working with Images

Podman pulls images from standard registries (like Docker Hub or Quay.io).

**Pull an Image:**

```bash
podman pull alpine
```

**List Local Images:**

```bash
podman images
```

**Build an Image from a Dockerfile:**

```bash
podman build -t my-custom-app .
```

---

## 5. Podman Desktop (Optional)

If you prefer a Graphical User Interface (GUI) similar to Docker Desktop, you can install Podman Desktop:

```bash
brew install --cask podman-desktop
```

This tool provides a dashboard to manage your containers, pods, and images visually.

---

## Conclusion

Podman provides a seamless transition for Docker users while offering a more secure and Kubernetes-aligned architecture.
With its simple Homebrew setup, it is an excellent choice for developers on macOS.
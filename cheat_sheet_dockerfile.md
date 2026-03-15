# Creating Docker Images (Dockerfile & Image Build)

Creating custom images is the process of automating your environment setup. Instead of manually configuring a container, you describe the steps in a text file that Docker transforms into a "template."

---

## 1. Key Instructions in a Dockerfile
Each instruction in a Dockerfile creates a new layer in the image.

* **FROM** – Defines the base image (e.g., `FROM ubuntu:22.04`). This must always be the first line.
* **WORKDIR** – Sets the working directory inside the image (equivalent to `cd`). It helps keep the file system organized.
* **COPY** – Copies files from your VM/Host to the interior of the image (e.g., `COPY . /app`).
* **RUN** – Executes commands during the build process (e.g., `RUN apt update && apt install -y curl`).
* **ENV** – Sets permanent environment variables (e.g., `ENV PORT=8080`).
* **EXPOSE** – Informs which port the application listens on (primarily for documentation purposes).
* **CMD** – Defines the default command to run when the container starts (e.g., `CMD ["nginx", "-g", "daemon off;"]`).

---

## 2. Building the Image (New Syntax)
To transform a Dockerfile into an image, we use the `image build` command.

**Basic Syntax:**
`docker image build -t image-name:tag .`

* **-t** (tag) – Assigns a name and a version to your image (e.g., `my-api:1.0`).
* **.** (dot) – Specifies the current directory as the build context (where the Dockerfile is located).

**Full Process Example:**
1. Create file: `nano Dockerfile`
2. Build: `docker image build -t my-app:v1 .`
3. Check the list: `docker image ls`

---

## 3. Layering Mechanism and Caching

* **Layers:** Docker remembers the result of every instruction in the Dockerfile.
* **Caching:** If you don't change a line like `FROM` or `RUN`, but only change a file in `COPY`, Docker will use the "Cached" results for the previous steps during the next build. This makes building take seconds instead of minutes.

---

## 4. Best Practices
* **Use Alpine versions:** Whenever possible, choose images ending in `-alpine` (e.g., `node:alpine`). They are significantly smaller and more secure.
* **Group RUN commands:** Instead of using two `RUN` lines, use `RUN apt update && apt install -y git`. This creates only one layer instead of two.
* **Copy only what is necessary:** Do not include unnecessary logs or documentation in the image to keep it as lightweight as possible.

---

## 5. Useful Image Management Commands
* `docker image ls` – List all images on the VM.
* `docker image inspect <id>` – Detailed information about an image (e.g., its ENV variables).
* `docker image rm <id>` – Remove an unnecessary image.
* `docker image prune` – Cleans up all unused and "dangling" layers/images.
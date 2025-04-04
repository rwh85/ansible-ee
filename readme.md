# Building an Ansible Execution Environment (EE)

## Step 1: Prepare Your Environment

1. **Ensure Docker Is Installed**  
   Use your preferred installation method (package manager, official installer, etc.). Verify Docker is operational:
   ```bash
   docker version
   ```

2. **Install ansible-builder**  
   Install ansible-builder from PyPI:
   ```bash
   pip install ansible-builder
   ```

3. **Authenticate with the Base Image Registry**  
   For pulling an EE base image, log in with the appropriate registry. For Red Hat’s container catalog, run:
   ```bash
   docker login registry.redhat.io
   ```
   Replace `registry.redhat.io` with another registry address if using a different source for the base image.

**Example Base Image**  
You can pull the minimal RHEL 9 Execution Environment base image from the Red Hat container catalog by running:
```bash
docker pull registry.redhat.io/ansible-automation-platform-25/ee-minimal-rhel9:latest
```
This image contains a minimal set of packages required to build an Ansible Execution Environment on Red Hat Enterprise Linux 9.

## Step 2: Inspect or Update Dependencies

- **requirements.txt**: Python modules.
- **requirements.yml**: Ansible collections.
- **execution-environment.yml**: Points to the above dependencies and configures the build process.

## Step 3: Build the Container Image

Use [ansible-builder](https://docs.ansible.com/ansible-builder/latest/). In the repository root, run:

```bash
ansible-builder build \
  --file execution-environment.yml \
  --tag my-ansible-ee:latest \
  --container-runtime docker
```

Replace `my-ansible-ee:latest` with your preferred image name.

## Step 4: Verify the Image

1. Check local images:
   ```bash
   docker images
   ```
2. Test the Ansible version:
   ```bash
   docker run --rm -it my-ansible-ee:latest ansible --version
   ```

## Step 5: (Optional) Push to a Registry

Tag and push your built image to your chosen registry. For example, using Docker Hub:

```bash
docker tag my-ansible-ee:latest <REGISTRY>/<USERNAME>/<REPO>:latest
docker push <REGISTRY>/<USERNAME>/<REPO>:latest
```

You can substitute `<REGISTRY>/<USERNAME>/<REPO>` with your own values.

## **1. Minimizing Image Size**

Smaller images are faster to pull, faster to deploy, and consume less storage.

### **Use Alpine or Slim Base Images**

The base image significantly impacts the final size.

- **Avoid:** `FROM node` (often ~1GB)
- **Prefer:** `FROM node:alpine` (often <100MB) or `FROM node:slim`
- **Note:** Alpine uses `musl` instead of `glibc`. If your application relies on C libraries compiled against `glibc`, use `debian-slim`.

### **Multi-Stage Builds**

This is the single most effective technique. It allows you to use a heavy image for building your app (compiling binaries, installing dependencies) and a tiny image for running it.

**Example (Go Application):**

Dockerfile

```
# Stage 1: Build
FROM golang:1.21-alpine AS builder
WORKDIR /app
COPY . .
RUN go build -o myapp main.go

# Stage 2: Run
FROM alpine:latest
WORKDIR /root/
# Copy only the binary from the builder stage
COPY --from=builder /app/myapp .
CMD ["./myapp"]
```

### **Clean Up After Installation**

When using package managers like `apt`, `apk`, or `yum`, temporary files are often created. Clean them up in the _same_ `RUN` instruction to prevent them from being committed to the layer.

**Bad:**

Dockerfile

```
RUN apt-get update
RUN apt-get install -y python3
RUN rm -rf /var/lib/apt/lists/*
```

_Why it's bad:_ The files are deleted in layer 3, but they still exist in layer 1 and 2, bloating the history.

**Good:**

Dockerfile

```
RUN apt-get update && apt-get install -y \
    python3 \
    && rm -rf /var/lib/apt/lists/*
```

### **Use `.dockerignore`**

Just like .gitignore, this file prevents unnecessary files from being sent to the Docker daemon during the build context.

Common entries:

- `.git`
- `node_modules` (install these inside the container)
- `Dockerfile`
- `README.md`
- Tests and temporary files

## **2. Maximizing Build Speed (Caching)**

Docker builds images in layers. If a layer hasn't changed, Docker uses the cached version. You want to maximize cache hits.

### **Order Matters (Least to Most Frequent Changes)**

Place instructions that change less frequently (installing system dependencies) at the top, and instructions that change often (copying source code) at the bottom.

**Bad:**

Dockerfile

```
COPY . .
RUN npm install
```

_Why:_ Every time you change a single line of code, the cache for `COPY . .` is invalidated, forcing `npm install` to run again.

**Good:**

Dockerfile

```
COPY package.json package-lock.json ./
RUN npm install
COPY . .
```

_Why:_ `npm install` only runs if you actually change your dependencies.

### **Combine RUN Instructions**

Each `RUN` instruction creates a new layer. While recent Docker versions optimize this better, combining commands reduces overhead and keeps related operations together.

Dockerfile

```
RUN apt-get update && apt-get install -y \
    curl \
    vim \
    git
```

## **3. Security Best Practices**

Running containers securely is just as important as building them efficiently.

### **Don't Run as Root**

By default, Docker containers run as root. This is a security risk if the container is compromised. Create a specific user.

Dockerfile

```
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser
```

### **Pin Versions Explicitly**

Using tags like `latest` is unpredictable. If the base image updates with a breaking change, your build breaks.

- **Bad:** `FROM node:latest`
- **Good:** `FROM node:18.16.0-alpine`

### **Scan for Vulnerabilities**

Integrate scanning tools into your CI/CD pipeline.

- **Tools:** Docker Scout, Trivy, Grype.
- **Usage:** `trivy image my-app:latest`

### **Secrets Management**

**Never** put secrets (API keys, passwords) in the Dockerfile.

- **Bad:** `ENV DB_PASSWORD=password123`
- **Good:** Use Docker BuildKit secrets or inject them as environment variables at runtime (`docker run -e`).

## **4. Maintainability & Linting**

### **Add Metadata with LABEL**

Labels help organize images, track maintainers, and versioning.

Dockerfile

```
LABEL maintainer="jane@example.com"
LABEL version="1.0"
LABEL description="Production API for User Service"
```

### **Lint Your Dockerfile**

Use a linter to catch syntax errors and bad practices automatically.

- **Tool:** [Hadolint](https://github.com/hadolint/hadolint)
- **Run:** `hadolint Dockerfile`

## **Summary Cheat Sheet**

|**Optimization**|**Technique**|**Benefit**|
|---|---|---|
|**Size**|Multi-stage builds|Removes build tools/source code from final image.|
|**Size**|`alpine` / `distroless` images|Drastically reduces base OS overhead.|
|**Speed**|Copy dependency files first|Maximizes layer caching for `npm install` / `pip install`.|
|**Security**|`USER appuser`|Prevents root access in case of a breach.|
|**Context**|`.dockerignore`|Prevents uploading massive local files to the daemon.|

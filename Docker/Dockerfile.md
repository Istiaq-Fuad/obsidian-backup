# Dockerfile creation guideline

A practical, compact guide to writing clear, efficient and secure Dockerfiles you can reuse as a checklist.

## 1. Start with a clear goal

State what the image must contain and how it will run.  
Keep a separation between build-time steps and runtime behavior. This mental model makes the Dockerfile simpler and easier to optimize.

## 2. Choose the right base image

Prefer official, minimal images that match your runtime needs.  
Use a well-scoped tag (for example, `python:3.11-slim` or `node:20-alpine`) rather than `:latest` to avoid unexpected changes.

## 3. Basic Dockerfile structure

Use a small, logical sequence of instructions so layers are meaningful and cache-friendly. Typical order:

1. `FROM` base
2. `LABEL` metadata
3. `ARG` and `ENV` for build-time and runtime variables
4. `WORKDIR`
5. `COPY` or `ADD` only what is required for the following `RUN` steps
6. `RUN` commands to install packages and build artifacts
7. `COPY` application source if build produced artifacts
8. `EXPOSE`, `VOLUME` as hints
9. `USER` to drop privileges
10. `HEALTHCHECK` and `STOPSIGNAL` if needed
11. `ENTRYPOINT` or `CMD` to set container runtime

## 4. Common directives and how to use them

FROM  
Set the base image. This must be the first instruction unless using multiple stages.

WORKDIR  
Create and switch to a working directory. Prefer `WORKDIR /app` over `RUN cd /app`.

COPY versus ADD  
COPY copies local files into the image and is the preferred, explicit choice.  
ADD has extra features (remote URLs, auto-extraction) and should only be used when you need those features.

RUN  
Use a single `RUN` with an appropriate shell form to combine package manager operations and cleanup in one layer. When installing packages, clean caches in the same RUN to reduce image size.

ENV and ARG  
Use `ARG` for build-time variables and `ENV` for runtime defaults. Do not store secrets in either.

EXPOSE  
Document which ports the container will listen on; this is informational and not a runtime requirement.

VOLUME  
Declare mount points for persistent or shared data, but avoid creating volumes for build-time temp files.

USER  
Run your application as a non-root user where possible. Create the user and set ownership with `chown` in the same layer that copies files.

ENTRYPOINT versus CMD  
Use `ENTRYPOINT` to define the binary to run and `CMD` for default arguments. Prefer exec form arrays so signals propagate correctly: `["/usr/bin/myapp", "--opt"]`.

HEALTHCHECK  
Add a `HEALTHCHECK` to allow orchestrators to detect unhealthy containers. Keep checks lightweight and fast.

LABEL  
Include useful metadata such as maintainer, version, and source. Use standardized keys when possible.

STOPSIGNAL  
Set a custom stop signal if your application needs different termination semantics.

## 5. Layering and cache efficiency

1. Put less-frequently-changing instructions early, more-frequently-changing sources later.
2. Install system packages and language runtimes before copying application code to maximize cache reuse during development.
3. Combine commands that belong together into a single `RUN` to reduce the number of layers.
4. Use multi-stage builds to separate build tools from runtime image, producing lean final images.

## 6. Image size optimization

1. Choose small base images like `-slim` or `alpine` when compatible.
2. Clean package manager caches in the same `RUN` that installs packages.
3. Remove build-time dependencies before final stage or use multi-stage build to avoid including them in the final image.
4. Use `.dockerignore` to avoid copying unnecessary files into the build context (node_modules, .git, docs, local env files).

## 7. Security considerations

1. Do not bake secrets into images. Use build-time secrets or runtime secret management.
2. Run as a non-root user unless absolutely necessary.
3. Pin versions for OS packages and language dependencies when possible.
4. Scan images for vulnerabilities before pushing to registries.
5. Restrict capabilities and run containers with least privilege in runtime configuration.

## 8. Build arguments, secrets and caching

1. Use `ARG` for values needed only during build. Remember `ARG` values are not persisted to final image unless also set as `ENV`.
2. For sensitive build secrets use Docker buildkit secrets or external secret managers.
3. Use `--cache-from` or careful layering to speed CI builds.

## 9. Multi-stage builds

Use multiple `FROM` lines to build artifacts in one stage and copy only the final artifact into a minimal runtime stage. This is essential for compiled languages and for removing development dependencies.

Example multi-stage pattern for a Go binary

```dockerfile
# Builder stage
FROM golang:1.20 AS builder
WORKDIR /src
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o /out/myapp ./cmd/myapp

# Final runtime stage
FROM scratch
COPY --from=builder /out/myapp /myapp
EXPOSE 8080
USER 1000
ENTRYPOINT ["/myapp"]
```

## 10. Example: simple Node.js Dockerfile

```dockerfile
FROM node:20-slim
LABEL maintainer="you@example.com"
WORKDIR /app

COPY package.json package-lock.json ./
RUN npm ci --only=production

COPY . .
USER node
EXPOSE 3000
CMD ["node", "index.js"]
```

## 11. Example: multi-stage build for a static site

```dockerfile
FROM node:20-alpine AS build
WORKDIR /site
COPY package.json yarn.lock ./
RUN yarn install --frozen-lockfile
COPY . .
RUN yarn build

FROM nginx:stable-alpine
COPY --from=build /site/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

## 12. Testing, linting and CI integration

1. Lint Dockerfiles with tools such as hadolint or similar to catch common anti-patterns.
2. Build images in CI using reproducible flags and cache strategies.
3. Run containerized tests and healthchecks in CI before publishing images.

## 13. Versioning and publishing

1. Tag images with semantic versions and a `latest` or rolling tag if needed.
2. Push to a registry with appropriate access control.
3. Use immutable tags for production deployments.

## 14. Quick checklist before committing a Dockerfile

1. `.dockerignore` created and excluding dev files
2. Base image tag is pinned and appropriate
3. No secrets inside the file
4. App runs as non-root user
5. Multi-stage build used if build artifacts should be excluded
6. Number of layers kept reasonable and cache-friendly order is used
7. Healthcheck and appropriate ENTRYPOINT/CMD present
8. Image size acceptable for your distribution method
9. Linting passed
10. Build and run tested locally and in CI

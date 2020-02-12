buildpack-deps
===

Custom versions of the official `buildpack-deps` image, used by our projects, allowing us to:

- Consolidate the installation and configuration of common system packages in one place; and

- Use a scheduled or manual build of the base image as a mechanism to bust the build cache of derivative project images, and ensure we deploy the latest security updates only during a specific window.

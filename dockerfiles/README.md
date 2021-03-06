Most of these Dockerfiles rely on `utility` dir to `COPY` some files from it.
This means if you want to build these Dockerfiles you **must** do it from `dockerfiles` dir.
Like that:

```bash
cd dockerfiles
docker build --no-cache
  --build-arg VCS_REF="12345"
  --build-arg BUILD_DATE="$(date +%Y%m%d)"
  --tag my-local:substrate
  --file <DOCKERFILE_DIR>/Dockerfile .
```

Rust 1.39.0

The current version of cmake is [3.16.0](https://github.com/Kitware/CMake/tree/v3.16.0)

For installation in our images we use the version hosted on github. Check the hash sum taken from the file with [sha256 hashes](https://github.com/Kitware/CMake/releases/download/v3.16.0/cmake-3.16.0-SHA-256.txt) and verify the [install script](https://github.com/Kitware/CMake/releases/download/v3.16.0/cmake-3.16.0-Linux-x86_64.sh).
If everything is correct, copy this file to utility folder and rename it to cmake.sh.

```bash
# download cmake
cd utility;
wget https://github.com/Kitware/CMake/releases/download/v3.16.0/cmake-3.16.0-Linux-x86_64.sh cp cmake-3.16.0-Linux-x86_64.sh cmake.sh
# install cmake
echo "c87dc439a8d6b1b368843c580f0f92770ed641af8ff8fe0b706cfa79eed3ac91 cmake.sh" | sha256sum -c - || exit 1;
chmod +x cmake.sh;
```

For Windows CI

```bash
choco install cmake --version=3.16.0
```

For macOS

```bash
brew install cmake
```

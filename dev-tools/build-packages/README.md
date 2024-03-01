# Package building
This folder contains tools used to create `rpm` and `deb` packages. 

## Requirements
 - A system with Docker.
 - Internet connection (to download the docker images the first time).

## Builders

### Tarball

To system packages (deb and rpm), a tarball of Fortishield dashboard `.tar.gz` is required.
This tarball contains the [Fortishield plugin][fortishield-plugin], the [Fortishield Security plugin][fortishield-security-plugin], 
a set of OpenSearch plugins and the default configuration for the app. 

The `generate_base.sh` script generates a `.tar.gz` file using the following inputs:
- `-a` | `--app`: URL to the zipped Fortishield plugin.*
- `-b` | `--base`: URL to the Fortishield dashboard `.tar.gz`, as generated with `yarn build --skip-os-packages --release`.*
- `-s` | `--security`: URL to the zipped Fortishield Security plugin, as generated with `yarn build`.*
- `-v` | `--version`: the Fortishield version of the package.
- `-r` | `--revision`: [Optional] Set the revision of the build. By default, it is set to 1.
- `-o` | `--output` [Optional] Set the destination path of package. By default, an output folder will be created in the same directory as the script.

*Note:* use `file://<absolute_path>` to indicate a local file. Otherwise, the script will try to download the file from the given URL.

Example:
```bash
bash generate_base.sh \
    --app https://packages-dev.fortishield.github.io/pre-release/ui/dashboard/fortishield-4.6.0-1.zip \
    --base file:///home/user/fortishield-dashboard/target/opensearch-dashboards-2.4.1-linux-x64.tar.gz \
    --security file:///home/user/fortishield-security-dashboards-plugin/build/security-dashboards-2.4.1.0.zip \
    --version 4.6.0
```

### DEB

The `launcher.sh` script generates a `.deb` package based on the previously generated `.tar.gz`. 
A Docker container is used to generate the package. It takes the following inputs:
- `-v` | `--version`: the Fortishield version of the package.
- `-p` | `--package`: the location of the `.tar.gz` file. It can be a URL or a PATH, with the format `file://<absolute_path>`
- `-r` | `--revision`: [Optional] Set the revision of the build. By default, it is set to 1.
- `-o` | `--output` [Optional] Set the destination path of package. By default, an output folder will be created in the same directory as the script. 
- `--dont-build-docker`: [Optional] Locally built Docker image will be used instead of generating a new one.

Example:
```bash
bash launcher.sh \
    --version 4.6.0 \
    --package file:///home/user/fortishield-dashboard/dev_tools/build_packages/base/output/fortishield-dashboard-4.6.0-1-linux-x64.tar.gz
```

### RPM

The `launcher.sh` script generates a `.rpm` package based on the previously generated `.tar.gz`. 
A Docker container is used to generate the package. It takes the following inputs:
- `-v` | `--version`: the Fortishield version of the package.
- `-p` | `--package`: the location of the `.tar.gz` file. It can be a URL or a PATH, with the format `file://<absolute_path>`
- `-r` | `--revision`: [Optional] Set the revision of the build. By default, it is set to 1.
- `-o` | `--output` [Optional] Set the destination path of package. By default, an output folder will be created in the same directory as the script. 
- `--dont-build-docker`: [Optional] Locally built Docker image will be used instead of generating a new one.

Example:
```bash
bash launcher.sh \
    --version 4.6.0 \
    --package file:///home/user/fortishield-dashboard/dev_tools/build_packages/base/output/fortishield-dashboard-4.6.0-1-linux-x64.tar.gz
```

[fortishield-plugin]: https://github.com/fortishield/fortishield-kibana-app
[fortishield-security-plugin]: https://github.com/fortishield/fortishield-security-dashboards-plugin
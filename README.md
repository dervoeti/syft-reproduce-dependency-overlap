## Test repo to reproduce mixed up dependency tree in Syft SBOM

Generate SBOMs for both Rust apps using [cargo-cyclonedx](https://github.com/CycloneDX/cyclonedx-rust-cargo):
```
pushd app1 && cargo cyclonedx --spec-version 1.5 -f json && popd
pushd app2 && cargo cyclonedx --spec-version 1.5 -f json && popd
```

Build a container image:
```
docker build . -t sbom:test
```

Scan the image with Syft 1.14:
```
syft scan --output cyclonedx-json@1.5=testbom.json --select-catalogers +sbom-cataloger sbom:test
```

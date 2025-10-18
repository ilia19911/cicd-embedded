# Reusable CI — CMake + Conan

Этот репозиторий содержит параметризуемый reusable workflow GitHub Actions,
который выполняет:
- подготовку окружения (Conan, remotes),
- сборку x86 (с покрытием),
- сборку target (опционально),
- тесты, отчёты, coverage,
- публикацию пакета в Conan (опционально),
- semantic-release (опционально).

## Подключение в проекте

В своем репозитории создайте `.github/workflows/ci.yml`:

```yaml
name: Project CI

on:
  push:
    branches: [ main, develop, testing ]
  pull_request:
  workflow_dispatch:

jobs:
  call-shared-ci:
    uses: org/gha-templates/.github/workflows/cmake-conan-ci.yml@v1
    with:
      runner_labels_json: '["self-hosted","Linux","X64"]'
      cmake_build_type: Release
      conan_remote_name: insitech
      pkg_name: protolib

      do_build_target: true
      do_tests: true
      do_coverage: true
      do_conan_publish: false
      do_semantic_release: false

      extra_cmake_flags_x86: ""
      extra_build_flags_x86: ""
      extra_cmake_flags_arm: ""
      extra_build_flags_arm: ""

    secrets:
      CONAN_REMOTE_URL: ${{ secrets.CONAN_REMOTE_URL }}
      NEXUS_USER:       ${{ secrets.NEXUS_USER }}
      NEXUS_PASS:       ${{ secrets.NEXUS_PASS }}

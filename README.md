# infinityx-gsi
Learning build
name: Build GSI

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-22.04

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup build environment
        run: |
          sudo apt update
          sudo apt install -y git repo openjdk-17-jdk

      - name: Init Infinity X Source
        run: |
          mkdir rom
          cd rom
          repo init -u https://github.com/ProjectInfinity-X/manifest.git -b qpr2

      - name: Repo Sync
        run: |
          cd rom
          repo sync -c --force-sync --no-clone-bundle --no-tags -j2

      - name: Build GSI
        run: |
          cd rom
          source build/envsetup.sh
          lunch infinity_gsi_arm64-userdebug
          make -j2

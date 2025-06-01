![deployment](https://img.shields.io/github/deployments/WebCryptomining/WebCryptomining.github.io/github-pages?label=deployment) ![Platform](https://img.shields.io/badge/platform-Linux-important) ![license](https://img.shields.io/github/license/WebCryptomining/WebCryptomining.github.io) ![size](https://img.shields.io/github/repo-size/WebCryptomining/WebCryptomining.github.io)

## 0. Table of Contents

* [Introduction](#1-introduction)
* [Code](#2-code)
* [Data](#3-data)
* [Demo](#4-demo)

## 1. Introduction

As a keystone of the cryptocurrency ecosystem, democracy has been severely threatened by the monopolization of mining farms in recent years. Existing countermeasures by enforcing new mining algorithms can effectively address this, but on the other hand have rendered web cryptomining unprofitable and gradually fading.

To address this, we design and implement an open-source web miner named Vectra to make web cryptomining efficient and profitable, thus reviving the democracy of the cryptocurrency ecosystem. Vectra employs a novel methodology of just-in-time transformation of mining programs, which discovers the pervasive isomorphic mining instructions for instruction merging and acceleration.

This repository contains the source code of Vectra, the data involved in this work, and a system demo.

## 2. Code

### Build from source

Vectra consists of two components including WebRandomX and WRXProxy.

WebRandomX is a high-performance web miner. It implements the key mechanisms of just-in-time transformations of mining programs for RandomX. The core of WebRandomX is built into a WASM binary so that one can load it and perform web mining after the page is loaded from the remote server. We provide an example web page to bootstrap the binary. 

WRXProxy is a mining pool proxy running on a remote server. It collects mining tasks from the mining pool (binding to a user's RandomX wallet address), and distributes the tasks to active WebRandomX instances. When a WebRandomX instance finds a nonce that satisfies the difficulty criteria, it will submit it to WRXProxy and WRXProxy then proxies the nonce to the mining pool. In this way, one can gain profits from the submitted nonces.

#### Prerequisites

* **Cloud server** (for building and hosting Vectra): at least 4 GB memory and 2 Gbps bandwidth
* **OS**: Ubuntu 22.04.4 LTS
* **Emscripten emcc**: 3.1.5
* **npm**: 8.5.1
* **Node.js**: 12.22.9

#### Compile and deploy WebRandomX

Clone and build the WebRandomX binary:

```shell
git clone https://github.com/WebCryptomining/WebRandomX.git
cd WebRandomX
mkdir build && cd build
emcmake cmake -DARCH=native ..
make
```

The built binary *web-randomx.wasm* will locate at *WebRandomX/build/web-randomx.wasm*

Afterwards, install npm dependencies to build the bootstrap page and run the web server:

```shell
npm install
npm run dev
```

The bootstrap page can be visited at `[Your Server IP]:9999`.

**Note**: The proxy server address should be configured in `src/js/job.js`.

#### Build and deploy WRXProxy

To enable web mining on WebRandomX page, WRXProxy should be built and deployed on the server:

```shell
git clone https://github.com/WebCryptomining/WRXProxy.git
cd WRXProxy && npm install
```

The RandomX wallet should be configured in *config.json*:

```json
{
  "miner": {
    "port": 80
  },
  "pool": {
    "host": "gulf.moneroocean.stream",
    "port": 10001
  },
  "info": {
    "wallet": "# Monero Wallet Address",
    "password": "# Monero Wallet Password"
  }
}
```

Then, simply run `npm run start` to start the WRXProxy

The WebRandomX page should communicate with the proxy and mine automatically.


## 3. Data

We provide our measurement data regarding the tested mining programs at [WebCryptomining.github.io/dataset](https://github.com/WebCryptomining/WebCryptomining.github.io/tree/main/dataset)

Each file contains the RandomX instructions in a mining program, and each line in a file is a specific instruction organized as:

```
<op code>, <dst reg>, <src reg>, <mod>, <imm>
```

## 4. Demo

We also provide our pre-complied demo for Vectra [here](http://24.199.105.99:9999/).

<img width="1453" alt="demo" src="https://github.com/user-attachments/assets/7e3190e1-c254-4323-9d3c-56948cae239d" />

